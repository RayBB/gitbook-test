# Deployment-Guide

## Checklist for Deployment

1. [ ] [Verify all Automated Tests pass](https://github.com/internetarchive/openlibrary/wiki/Testing)
2. [ ] [Test & QA changes on Staging](deployment-guide.md#testing-on-staging)
3. [ ] [Deploy to Production](deployment-guide.md#deploying-to-production)
   * [ ] [Deploy olsystem](deployment-guide.md#deploying-olsystem)
   * [ ] [Deploy Open Library](deployment-guide.md#deploying-openlibrary)
   * [ ] [Test the Deployment](deployment-guide.md#testing-deployment)
   * [ ] [Restart OpenLibrary Service](deployment-guide.md#restarting-services)
4. [ ] [Monitor Deployment](deployment-guide.md#monitoring-deployment)
5. [ ] [Git Tag Successful Release](deployment-guide.md#git-tagging-a-release)
6. [ ] [Troubleshoot a Failed Deployment](deployment-guide.md#recovering-from-a-failed-deployment)
   * [ ] [Diagnosing Failed Deployment](deployment-guide.md#failed-deploys)
   * [ ] [Rolling Back](deployment-guide.md#rolling-back)

## Walkthrough Video

This quickstart video is not a full replacement for the list of instructions above \(which including testing\). Also, this video omits the final step: [tagging a release](https://github.com/internetarchive/openlibrary/wiki/Deployment-Guide#git-tagging-a-release)

[![archive org\_details\_openlibrary-tour-2020\_internal-staff-deployment mp4](https://user-images.githubusercontent.com/978325/93828018-56e11a00-fc1f-11ea-9a6b-89ddc4b9feb3.png)](https://archive.org/embed/openlibrary-tour-2020/internal-staff-deployment.mp4)

## Introduction

Greetings deployer! You are one of the select few who has privileged access to Internet Archive infrastructure and is tasked with the important responsibility of deploying the Open Library website. Deploys typically occur on Thursday, after the candidate code has had a few days to be tested on staging \([https://staging.openlibrary.org](https://staging.openlibrary.org)\). The following are instructions that we hope will ease your journey towards a successful deployment of the Open Library system. For more information on the system architecture, please refer to the [Production Provisioning Guide](https://github.com/internetarchive/openlibrary/wiki/Production-Service-Architecture)

## Testing on Staging

Before code is pushed to production, it pass all tests -- see [Testing Guide](https://github.com/internetarchive/openlibrary/wiki/Testing) -- and should be QA tested on development, i.e. `ol-dev1` \([https://staging.openlibrary.org](https://staging.openlibrary.org)\). Ensure any missing packages are added to the `requirements*.txt` and `scripts/bootstrap.sh` files so that these dependencies are updated on the next build \(\#Satisfying\_Dependency\_Changes\).

```bash
# ol-dev01.us.archive.org is http://staging.openlibrary.org

ssh -A ol-dev1
cd /opt/openlibrary  # where Open Library code lives

# Predefine the Docker Compose files that are required for a staging build
export COMPOSE_FILE="docker-compose.yml:docker-compose.staging.yml"

# Ensure that the hostname will appear on http://staging.openlibrary.org/status and in logs
export HOSTNAME=$HOSTNAME

git branch
git status  # see if there are any changes that you want to save or stash

git checkout master
git status  # see if there are any changes that you want to save or stash

git pull origin master

# If you want to test out one or more branches...
sudo vi _dev-merged.txt && sudo ./scripts/make-integration-branch.sh _dev-merged.txt dev-merged

# To see what this software version should be at http://staging.openlibrary.org/status
git rev-parse --short HEAD

# Restart the server http://staging.openlibrary.org ...
docker-compose down && \
    PYENV_VERSION=3.8.6 docker-compose up -d --no-deps memcached web && \
    docker-compose logs -f --tail=10 web
# In your browser, check http://staging.openlibrary.org/status to verify the Python version and hostname

For a normal build...
docker-compose build --pull web
# Followed by the docker-compose down/up commands above

For a full rebuild...
docker build -t openlibrary/olbase:latest -f docker/Dockerfile.olbase .
# Followed by the docker-compose down/up commands above
```

## Satisfying Dependency Changes

I dependencies are changed, perform a build as discussed above.

\(Not much left to say here except solr-updater?\)

## Deploying to Production

### Background

`ol-home0` actually has 2 openlibrary repos on it. One is in `/opt/openlibrary/openlibrary` \(the same convention as all other Open Library servers\). In addition, `ol-home0` also has a repo within `/1/var/lib/openlibrary/deploy/openlibrary`. When a new deploy of `openlibrary` is performed, it is executed on `/1/var/lib/openlibrary/deploy/openlibrary` and then rsync'ed to each server's `/opt/openlibrary/openlibrary` path. This is why deploys must be done from `ol-home1`.

### Strategy

On production, Open Library practices [blue-green](http://blog.christianposta.com/deploy/blue-green-deployments-a-b-testing-and-canary-releases/) deployment. The purpose of a blue-green deploy is to gracefully deploy without downtime. Blue-green deployment is a 2-stage strategy whereby deployment is performed on a non-active secondary node \(blue\) without any changes being made to the primary \(green\) active node. Once it has been ensured that deployment of the blue node has completed successfully, a command is issued to switch the roles of the blue and green node \(the newly deployed blue node becomes the green node\). The previous green node may then be used as a secondary green node within a load-balanced pool until it is needed again as a blue node \(as is the case with our setup\). Open Library has two web nodes \(`ol-web1` and `ol-web2`\) which are load balanced using `haproxy` behind `nginx` \(`ol-www1`\).

For Open Library, we have a haproxy load-balancer \(`ol-www1`\) which coordinates several services, including 2 instances of the Open Library website \(`ol-web1` and `ol-web2`\) which it distributes balanced workloads to. During our blue-green deploy, we deploy to both `ol-web1` and `ol-web2` but we only restart `ol-web1` \(our blue node\) while `ol-web2` \(green\) continues to serve clients using the stable code. This way, if there is a problem with deployment, we can take down `ol-web1` and revert it to the software from the previous deployment.

Caution: One of our two web servers \(namely `ol-web1`, our blue node\) is effectively being recommissioned as a staging server during a deployment. This means that additional stress will be applied to `ol-web2` in the event where `ol-web1` experiences a failure and goes offline. Therefore, it is not advised to deploy during periods of high user traffic.

### Deploying olsystem

* [ ] Check for [red flags in nagios](https://monitor.archive.org/cgi-bin/nagios3/status.cgi?hostgroup=24.openlibrary&style=detail&limit=0&start=100&limit=100)
* [ ] Warn Slack channels `openlibrary` and `openlibrary-g` of imminent downtime!
* [ ] Open [https://openlibrary.org/admin?stats](https://openlibrary.org/admin?stats) so that you can monitor server status

Note: If you are following these instructions while provisioning new servers for the Open Library service \(e.g. adding a memcached server to the pool\) please also refer to the [Provisioning Guide](https://github.com/internetarchive/openlibrary/wiki/Provisioning-Guide).

[Olsystem](https://github.com/internetarchive/olsystem) is the configuration repository for Open Library. Most deployments shouldn't require changes to these configurations. If you're deploying `olsystem` changes which affect memcached servers, solr, or databases, it is best practice to stop Open Library services which use these configs before deploying and then restart them after the config is deployed:

* [ ] stop `ol-web1` and `ol-web2`: `ssh ol-webX cd /opt/openlibrary ; docker-compose down`  Repeat on all ol-webX servers.
* [ ] stop `ol-staging` \(which uses the production config\): `ssh ol-dev1 cd /opt/openlibrary ; docker-compose down`
* [ ] stop `ol-mem[3-5]`: e.g. `ssh ol-mem3 sudo /etc/init.d/memcached stop`
* [ ] stop `ol-home0` services \(import-bot, solr-updater, infobase\): e.g. `ssh ol-home0 cd /opt/openlibrary ; docker-compose down`

**TODO:** Once your `olsystem` configuration changes are tested on `ol-dev1` \(staging\) and merged to master, you may deploy `olsystem` from `ol-home` by running:

```bash
/olsystem/bin/deploy-code olsystem
```

At this point, if a deployment of `openlibrary` is also necessary, **now would be a good time to continue with the** [**Deploying OpenLibrary**](deployment-guide.md#deploying-openlibrary) **instructions** prior to restarting these services.

Otherwise, \(if your change only affects `olsystem` configs and not `openlibrary`, then once the deploy succeeds, restart the above services in reverse order. Note we use [supervisorctl update](http://supervisord.org/running.html#supervisorctl-actions) here so that the config files are reloaded from disk.

* [ ] start `ol-home` services \(import-bot, solr-updater, infobase\): run\_olserver.sh
* [ ] for `ol-mem[3-5]` run `sudo /etc/init.d/memcached start` 
* [ ] on ol-dev1 redo the \(\#Testing\_on\_Staging\) process
* [ ] On `ol-web1` and `ol-web2` run `SERVICE=web scripts/run_olserver.sh`

### Deploying OpenLibrary

On ol-home \(make sure you ssh with -A to forwards ssh keys, required if you don't want "permission denied for pubkey" errors or restart\) issue the following [commands](https://github.com/internetarchive/openlibrary/tree/master/scripts/deployment) to initiate a deployment to all production nodes:

```bash
ssh -A ol-home0
scripts/deployment/deploy.sh
```

If `deploy.sh` exits with a failure, see the [failed deploys](deployment-guide.md#failed-deploys) troubleshooting guide.

* [ ] Check for [red flags in nagios](https://monitor.archive.org/cgi-bin/nagios3/status.cgi?hostgroup=24.openlibrary&style=detail&limit=0&start=100&limit=100)
* [ ] Warn Slack channels `openlibrary` and `openlibrary-g` of imminent downtime!
* [ ] Open [https://openlibrary.org/admin?stats](https://openlibrary.org/admin?stats) so that you can monitor server status

See [https://github.com/internetarchive/openlibrary/pull/4457](https://github.com/internetarchive/openlibrary/pull/4457)

```bash
scripts/deployment/restart_servers.sh ol-home0 ol-covers0 ol-web1
```

Once ol-web1 turns green in [https://openlibrary.org/admin?stats](https://openlibrary.org/admin?stats) ... ... Visit [https://ol-web1.us.archive.org:8080/status](https://ol-web1.us.archive.org:8080/status) and ensure that site is functioning as expected.

If you are satisfied that ol-web1 is functioning as expected then reboot ol-web2 as well.

```bash
scripts/deployment/restart_servers.sh ol-web2
```

... Visit [https://ol-web2.us.archive.org:8080/status](https://ol-web2.us.archive.org:8080/status) and ensure that site is functioning as expected.

### Testing Deployment

`ssh` into the public-facing web-head \(i.e. `ol-www1`\), activate the VM's virtualenv, and try importing the openlibrary package directly from python and start the service on a different port for testing:

```bash
source /opt/openlibrary/venv/bin/activate
python3 import openlibrary  # detect any obvious breaking problems

# Run Open Library (in stand-alone debug mode) on a different port
sudo /olsystem/bin/upstart-service openlibrary-server :1234

`cd /opt/openlibrary && docker-compose down`
```

### Restarting Services

See code for [`scripts/run_olserver.sh`](https://github.com/internetarchive/openlibrary/blob/master/scripts/run_olserver.sh)

From `ol-home`, once the deployment\(s\) are complete and you've [tested deployment](deployment-guide.md#Testing_Deployment), you are ready to restart the production web-nodes `ol-web1` and `ol-web2` in blue-green fashion. Consult [Monitoring Deployment](deployment-guide.md#Monitoring_Deployment) after **each** web-node is restarted:

```bash
ssh ol-web1 cd /opt/openlibrary ; PY_ENV=3.8.6 SERVICE=web sudo scripts/run_service.sh  # blue node;
```

Before proceeding, [check haproxy loadbalancer](https://openlibrary.org/admin?stats) to ensure the blue node succeeds before attempting to restart the green node.

```bash
ssh ol-web2 cd /opt/openlibrary ; PY_ENV=3.8.6 SERVICE=web sudo scripts/run_service.sh  # green node;
```

## Monitoring Deployment

After deploying Open Library, verify all systems are green via the following procedures:

* Check [HAProxy](https://openlibrary.org/admin?stats) to ensure both `ol-web1` and `ol-web2` are up \(as is `ol-www1`\)
* Check for [red flags in nagios](https://monitor.archive.org/cgi-bin/nagios3/status.cgi?hostgroup=24.openlibrary&style=detail&limit=0&start=100&limit=100)

## Git Tagging a Release

After you have deployed to ol-home0 \(production Open Library\), tag the master branch accordingly:

Check that you're on master...

```bash
git branch
```

Fill in the years, months, days where appropriate...

```bash
git tag deploy-yyyy-mm-dd
git push origin deploy-yyyy-mm-dd
```

## Recovering from a Failed Deployment

### Failed Deploys

In some cases, a deployment fails to complete and exits pre-maturely with an error. Here are a few common reasons this may occur:

#### Unstashed Changes on ol-home0

The most common cause of failed `openlibrary` deployment is unstashed changes within `/1/var/lib/openlibrary/deploy/openlibrary`. This is especially true for the `package-lock.json` files which sometimes gets modified while the deploy is building `nodejs` dependencies. This error will manifest as something like:

```text
[ol-home0] out: Updating 811fbbf..058c6ce
[ol-home0] out: error: Your local changes to the following files would be overwritten by merge:
[ol-home0] out:     package-lock.json
[ol-home0] out: Please, commit your changes or stash them before you can merge.
[ol-home0] out: Aborting
```

To resolve, `cd` to `/1/var/lib/openlibrary/deploy/openlibrary` and either `git stash` or `git checkout` any files which are causing conflicts.

### Rolling Back

Sometimes deploys don't fail to succeed, they just succeed in deploying or buggy code. The zero-day fix is to roll-back. Once a deploy is completed, previous deploys will be preserved \(within directories named by their git commit hash\) within `/opt/openlibrary/deploys`. The currently deployed/live code directory is symlinked from `/opt/openlibrary/openlibrary`.

To roll back, go to the particular server\(s\), e.g. ol-web1 and ol-web2, reset the `/opt/openlibrary/openlibrary` symlink to point to the desired earlier directory in `/opt/openlibrary/deploys` and then restart openlibrary.

The same procedure applies to olsystem \(although olsystem doesn't need to be restart; however, openlibrary may need a restart after deploying olsystem\).

TODO: Make this more compact/elegant

For each server:

```bash
ssh -A ol-web1
sudo su openlibrary
cd /opt/openlibrary
rm openlibrary
ln -s deploys/openlibrary/{{COMMITSHA}} openlibrary
logout
sudo supervisorctl restart openlibrary
```

## Docker-based deploys \(Beta\)

This will change quickly and frequently. Up-to-date as of 2020-09-29

### Servers for running Open Library on Python 3 on Ubuntu 20.04.1 LTS \(Focal Fossa\)

| Server | Setup script | Run script | /etc/os-release |
| :--- | :--- | :--- | :--- |
| ol-covers0 | scripts/setup\_olserver.sh | SERVICE=covers scripts/run\_olserver.sh | 20.04.1 |
| ol-dev1 | scripts/setup\_olserver.sh | N/A | _**16.04.7**_ |
| ol-home0 | scripts/setup\_olserver.sh | SERVICE=home scripts/run\_olserver.sh | 20.04.1 |
| ol-mem3 | ??? | ??? | _**16.04.7**_ |
| ol-mem4 | ??? | ??? | _**16.04.7**_ |
| ol-mem5 | ??? | ??? | _**16.04.7**_ |
| ol-solr0 | ? scripts/setup\_olserver.sh ? | See scripts/solr\_builder readme | 20.04.1 |
| ol-solr1 | ? scripts/setup\_olserver.sh ? | See scripts/solr\_builder readme | 20.04.1 |
| ol-web1 | scripts/setup\_olserver.sh | SERVICE=web scripts/run\_olserver.sh | 20.04.1 |
| ol-web2 | scripts/setup\_olserver.sh | SERVICE=web scripts/run\_olserver.sh | 20.04.1 |
| ol-www1 | scripts/setup\_olserver.sh | ???? | _**14.04.1**_ |

#### Initial setup:

1. export GITHUB\_USERNAME=cclauss
2. export GITHUB\_TOKEN=123abc  \# Create this token at [https://github.com/settings/tokens](https://github.com/settings/tokens)
3. Manually copy `scripts/setup_olserver.sh` to your home directory and run it.
4. Extras for some servers
   * `ol-covers` requires `cd /opt && sudo git clone https://git.archive.org/jake/booklending_utils`
   * ??? required volume mount `/olsystem/etc/ia.ini:/openlibrary/.config/ia.ini`
5. export SERVICE = xxx  \# Options for xxx are web, covers, home, infobase
6. Run the script at the bottom of this page to launch the Docker containers on the server.

#### Transitioning to Python 3: \(Hopefully these steps will not be needed again.\)

1. Ensure the above servers are setup and running
2. Warn Slack channels `openlibrary` and `openlibrary-g`
3. Open [https://openlibrary.org/admin?stats](https://openlibrary.org/admin?stats) to monitor server status
4. On ol-web1 and ol-web2:
   1. `sudo docker volume prune`
   2. run the run_olserver.sh script but **\_not**_ the last `docker-compose up` command
5. On ol-www1:
   1. `sudo vi /etc/haproxy/haproxy.cfg` to uncomment web{1,2} and comment out web3
   2. `sudo service haproxy restart`
6. on ol-web3 `sudo supervisorctl stop openlibrary`
7. On ol-web1 and ol-web2 run the `docker-compose up` command
8. Go to [https://openlibrary.org/admin?stats](https://openlibrary.org/admin?stats) and ensure that web{1,2,4} are all green.
9. On ol-www1:
   1. `sudo vi /etc/haproxy/haproxy.cfg` to comment out web4
   2. `sudo service haproxy restart`
10. On ol-web4 `sudo supervisorctl stop openlibrary`
11. On `ol-mem{3,4,5}` do `sudo service memcached restart`
12. Go to [https://openlibrary.org/admin?stats](https://openlibrary.org/admin?stats) and ensure that web{1,2} are all green.

#### Reverting to legacy Python: Repeat steps 3. thru 5. reversing web{1,2} and web{3,4}

