# Deployment-Guide-for-Python2

## Checklist for Deployment

1. [ ] [Verify all Automated Tests pass](https://github.com/internetarchive/openlibrary/wiki/Testing)
2. [ ] [Test & QA changes on Staging](deployment-guide-for-python2.md#testing-on-staging)
3. [ ] [Handle dependency changes](deployment-guide-for-python2.md#satisfying-dependency-changes)
4. [ ] [Deploy to Production](deployment-guide-for-python2.md#deploying-to-production)
   * [ ] [Deploy olsystem](deployment-guide-for-python2.md#deploying-olsystem)
   * [ ] [Deploy Open Library](deployment-guide-for-python2.md#deploying-openlibrary)
   * [ ] [Test the Deployment](deployment-guide-for-python2.md#testing-deployment)
   * [ ] [Restart OpenLibrary Service](deployment-guide-for-python2.md#restarting-services)
5. [ ] [Monitor Deployment](deployment-guide-for-python2.md#monitoring-deployment)
6. [ ] [Git Tag Successful Release](deployment-guide-for-python2.md#git-tagging-a-release)
7. [ ] [Troubleshoot a Failed Deployment](deployment-guide-for-python2.md#recovering-from-a-failed-deployment)
   * [ ] [Diagnosing Failed Deployment](deployment-guide-for-python2.md#failed-deploys)
   * [ ] [Rolling Back](deployment-guide-for-python2.md#rolling-back)

## Walkthrough Video

This quickstart video is not a full replacement for the list of instructions above \(which including testing\). Also, this video omits the final step: [tagging a release](https://github.com/internetarchive/openlibrary/wiki/Deployment-Guide#git-tagging-a-release)

[![archive org\_details\_openlibrary-tour-2020\_internal-staff-deployment mp4](https://user-images.githubusercontent.com/978325/93828018-56e11a00-fc1f-11ea-9a6b-89ddc4b9feb3.png)](https://archive.org/embed/openlibrary-tour-2020/internal-staff-deployment.mp4)

## Introduction

Greetings deployer! You are one of the select few who has privileged access to Internet Archive infrastructure and is tasked with the important responsibility of deploying the Open Library website. Deploys typically occur on Thursday, after the candidate code has had a few days to be tested on staging \([https://dev.openlibrary.org](https://dev.openlibrary.org)\). The following are instructions which we hope will ease you in your journey towards a successful deploy of the Open Library system. For more information on the system architecture, please refer to the [Production Provisioning Guide](https://github.com/internetarchive/openlibrary/wiki/Production-Service-Architecture)

## Testing on Staging

Before code is pushed to production, it pass all tests -- see [Testing Guide](https://github.com/internetarchive/openlibrary/wiki/Testing) -- and should be QA tested on development, i.e. ol-dev0 \([https://dev.openlibrary.org](https://dev.openlibrary.org)\). If packages are missing, make sure you track them in your `requirements.txt` and `scripts/bootstrap.sh` as these will have to be [manually installed on production](deployment-guide-for-python2.md#Satisfying_Dependency_Changes).

```bash
ssh -A ol-dev0
cd /opt/openlibrary/openlibrary  # where OL code lives
git checkout <branch>  # checkout the correct branch name

# Rebuild CSS, JS dependencies
sudo make css js

# Restart the dev server
sudo kill -9 `pgrep -f openlibrary-dev-server`  # kill previous instance
sudo /olsystem/bin/upstart-service openlibrary-dev-server :7071 &  # restart
```

## Satisfying Dependency Changes

As unideal as it my be, the deployment process is not yet fully configured to understand and be responsive to changes in your Python requirements.txt / library or your `apt-get` ubuntu dependencies. As a result, you'll need to manually make sure `ol-home`, `ol-web3`, `ol-web4`, and `ol-www1` have the correct dependencies using `pip` and `apt-get`:

```bash
ssh ol-web3 # needed for openlibrary restart
cd /opt/openlibrary
sudo su openlibrary
source venv/bin/activate
pip install <requirement(s)>

ssh ol-web4 # needed for openlibrary restart
sudo su openlibrary
cd /opt/openlibrary
source venv/bin/activate
pip install <requirement(s)>

ssh ol-home # needed for solr-updater restart
sudo su openlibrary
cd /opt/openlibrary
source venv/bin/activate
pip install <requirement(s)>

ssh ol-www1  # required for serving static files
sudo su openlibrary
cd /opt/openlibrary
source venv/bin/activate
pip install <requirement(s)>
```

After dependencies are installed, restart solr-updater, openlibrary and make sure nagios shows no alerts.

## Deploying to Production

### Background

`ol-home` actually has 2 openlibrary repos on it. One is in `/opt/openlibrary/openlibrary` \(the same convention as the other Open Library servers\). `ol-home` also has a repo within `/1/var/lib/openlibrary/deploy/openlibrary`. When a new deploy of `openlibrary` is performed, it is executed on `/1/var/lib/openlibrary/deploy/openlibrary` and then rsync'ed to each server's `/opt/openlibrary/openlibrary` path. This is why deploys must be done from `ol-home`.

### Strategy

On production, Open Library practices [blue-green](http://blog.christianposta.com/deploy/blue-green-deployments-a-b-testing-and-canary-releases/) deployment. The purpose of a blue-green deploy is to gracefully deploy without downtime. Blue-green deployment is a 2-stage strategy whereby deployment is performed on a non-active secondary node \(blue\) without any changes being made to the primary \(green\) active node. Once it has been ensured that deployment of the blue node has completed successfully, a command is issued to switch the roles of the blue and green node \(the newly deployed blue node becomes the green node\). The previous green node may then be used as a secondary green node within a load-blanced pool until it is needed again as a blue node \(as is the case with our setup\). Open Library has two web nodes \(`ol-web3` and `ol-web4`\) which are load balanced using `haproxy` behind `nginx` \(`ol-www1`\).

For Open Library, we have a haproxy load-balancer \(`ol-www1`\) which coordinates several services, including 2 instances of the openlibrary website \(`ol-web3` and `ol-web4`\) which it distributes balanced workloads to. During our blue-green deploy, we deploy to both `ol-web3` and `ol-web4` but we only restart `ol-web3` \(our blue node\) while `ol-web4` \(green\) continues to serve clients using the stable code. This way, if there is a problem with deployment,

Caution: Because one of our two web servers \(namely `ol-web3`, our blue node\) is effectively being recommissioned as a staging server during deployment, additional stress will be applied to `ol-web4` in the event where `ol-web3` experiences a failure and goes offline. Therefore, it is not advised to deploy during times of high user traffic.

### Deploying olsystem

Note: If you are following these instructions while provisioning new servers for the Open Library service \(e.g. adding a memcached server to the pool\) please also refer to the [Provisioning Guide](https://github.com/internetarchive/openlibrary/wiki/Provisioning-Guide).

[Olsystem](https://github.com/internetarchive/olsystem) is the configuration repository for Open Library. Most deployments shouldn't require change to these configurations. If you're deploying `olsystem` changes which affect memcached servers, solr, or databases, it's best practice to stop Open Library services which use these configs before deploying and then restart them after the config is deployed:

* [ ] stop `ol-web3` and `ol-web4`: `ssh ol-web3 sudo supervisorctl stop openlibrary;ssh ol-web4 sudo supervisorctl stop openlibrary`
* [ ] stop `ol-dev` \(which uses the production config\): `sudo kill -9`pgrep -f openlibrary-server\`
* [ ] stop `ol-mem[3-5]`: e.g. `ssh ol-mem3 sudo /etc/init.d/memcached stop`
* [ ] stop `ol-home` services \(import-bot, solr-updater, infobase\): e.g. `ssh ol-home supervisorctl stop infobase`

Once your to your `olsystem` configuration changes are tested on `ol-dev` and merged to master, you may deploy `olsystem` from `ol-home` by running:

```bash
/olsystem/bin/deploy-code olsystem
```

At this point, if a deploy of `openlibrary` is also necessary, **now would be a good time to continue with the** [**Deploying OpenLibrary**](deployment-guide-for-python2.md#deploying-openlibrary) **instructions** prior to restarting these services.

Otherwise, \(if your change only affects `olsystem` configs and not `openlibrary`, then once the deploy succeeds, restart the above services in reverse order. Note we use [supervisorctl update](http://supervisord.org/running.html#supervisorctl-actions) here so that the config files are reloaded from disk.

* [ ] start `ol-home` services \(import-bot, solr-updater, infobase\): `ssh ol-home supervisorctl update infobase`
* [ ] for `ol-mem[3-5]` run `sudo /etc/init.d/memcached stop` 
* [ ] on ol-dev run `sudo /olsystem/bin/upstart-service openlibrary-dev-server :7071 &`
* [ ] start `ol-web3` and `ol-web4`: `ssh ol-web3 sudo supervisorctl update openlibrary;ssh ol-web4 sudo supervisorctl update openlibrary`

### Deploying OpenLibrary

On ol-home \(make sure you ssh with -A to forwards ssh keys, required if you don't want "permission denied for pubkey" errors or restart\) issue the following commands to initiate a deployment to all production nodes:

```bash
ssh -A ol-home
/olsystem/bin/deploy-code openlibrary
```

If `openlibrary` deployment exits with a failure, see the [failed deploys](deployment-guide-for-python2.md#failed-deploys) troubleshooting guide.

### Testing Deployment

`ssh` into the public facing web-head \(i.e. `ol-www1`\), activate the VM's virtualenv, and try importing the openlibrary package directly from python and start the service on a different port for testing:

```bash
source /opt/openlibrary/venv/bin/activate
python import openlibrary  # detect any obvious breaking problems

# Run OL (in stand-alone debug mode) on a different port
sudo /olsystem/bin/upstart-service openlibrary-server :1234
# Ctrl+c to kill this process or...
sudo kill -9 `pgrep -f openlibrary-server`  # kill previous instance
```

### Restarting Services

From `ol-home`, once the deployment\(s\) are complete and you've [tested deployment](deployment-guide-for-python2.md#Testing_Deployment), you are ready to restart the production web-nodes `ol-web3` and `ol-web4` in blue-green fashion. Consult [Monitoring Deployment](deployment-guide-for-python2.md#Monitoring_Deployment) after **each** web-node is restarted:

```bash
ssh ol-web3 sudo supervisorctl restart openlibrary  # blue node;
```

Before proceeding, [check haproxy loadbalancer](https://openlibrary.org/admin?stats) to ensure the blue node succeeds before attempting to restart the green node.

```bash
ssh ol-web4 sudo supervisorctl restart openlibrary  # green node;
```

## Monitoring Deployment

After deploying Open Library, verify all systems are green via the following procedures:

* Check HAProxy to ensure both `ol-web3` and `ol-web4` are up \(as is `ol-www1`\)
* Check for [red flags in nagios](https://monitor.archive.org/cgi-bin/nagios3/status.cgi?hostgroup=24.openlibrary&style=detail&limit=0&start=100&limit=100)

## Git Tagging a Release

After you have deployed to ol-home \(production Open Library\), tag the master branch accordingly:

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

#### Unstashed Changes on ol-home

The most common case of failed `openlibrary` deployment is unstashed changes within `/1/var/lib/openlibrary/deploy/openlibrary`. This is especially true for the `package-lock.json` files which sometimes gets modified while the deploy is building `nodejs` dependencies. This error will manifest as something like:

```text
[ol-home] out: Updating 811fbbf..058c6ce
[ol-home] out: error: Your local changes to the following files would be overwritten by merge:
[ol-home] out:     package-lock.json
[ol-home] out: Please, commit your changes or stash them before you can merge.
[ol-home] out: Aborting
```

To resolve, `cd` to `/1/var/lib/openlibrary/deploy/openlibrary` and either `git stash` or `git checkout` any files which are causing conflicts.

### Rolling Back

Sometimes deploys don't fail to succeed, they just succeed in deploying or buggy code. The zero-day fix is to roll-back. Once a deploy is completed, previous deploys will be preserved \(within directories named by their git commit hash\) within `/opt/openlibrary/deploys`. The currently deployed/live code directory is symlinked from `/opt/openlibrary/openlibrary`.

To roll back, go to the particular server\(s\), e.g. ol-web3 and ol-web4, reset the `/opt/openlibrary/openlibrary` symlink to point to the desired earlier directory in `/opt/openlibrary/deploys` and then restart openlibrary.

The same procedure applies to olsystem \(although olsystem doesn't need to be restart; however, openlibrary may need a restart after deploying olsystem\).

TODO: Make this more compact/elegant

For each server:

```bash
ssh -A ol-web3
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
| ol-covers0 | scripts/setup\_olserver.sh | SERVICE=covers /opt/openlibrary/scripts/run\_olserver.sh | 20.04.1 |
| ol-dev1 | scripts/setup\_olserver.sh | N/A | _**16.04.7**_ |
| ol-home0 | scripts/setup\_olserver.sh | SERVICE=home /opt/openlibrary/scripts/run\_olserver.sh | 20.04.1 |
| ol-mem3 | ??? | ??? | _**16.04.7**_ |
| ol-mem4 | ??? | ??? | _**16.04.7**_ |
| ol-mem5 | ??? | ??? | _**16.04.7**_ |
| ol-solr0 | ? scripts/setup\_olserver.sh ? | See scripts/solr\_builder readme | 20.04.1 |
| ol-solr1 | ? scripts/setup\_olserver.sh ? | See scripts/solr\_builder readme | 20.04.1 |
| ol-web1 | scripts/setup\_olserver.sh | SERVICE=web /opt/openlibrary/scripts/run\_olserver.sh | 20.04.1 |
| ol-web2 | scripts/setup\_olserver.sh | SERVICE=web /opt/openlibrary/scripts/run\_olserver.sh | 20.04.1 |
| ol-www1 | scripts/setup\_olserver.sh | SERVICE=web /opt/openlibrary/scripts/run\_olserver.sh | _**14.04.1**_ |

#### Initial setup:

1. export GITHUB\_USERNAME=cclauss
2. export GITHUB\_TOKEN=123abc  \# Create this token at [https://github.com/settings/tokens](https://github.com/settings/tokens)
3. Manually copy `scripts/setup_olserver.sh` to your home directory and run it.
4. Extras for some servers
   * `ol-covers` requires `cd /opt && sudo git clone https://git.archive.org/jake/booklending_utils`
   * ??? required volume mount `/olsystem/etc/ia.ini:/openlibrary/.config/ia.ini`
5. export SERVICE = xxx  \# Options for xxx are web, covers, home, infobase
6. Run the script at the bottom of this page to launch the Docker containers on the server.

#### Transitioning to Python 3:

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

### staging.openlibrary.org is dev1 running Python 3

```bash
# Make sure you're in the docker group, to avoid sudo-ing all the docker commands
sudo usermod -aG docker USER_NAME
# Exit/re-enter to take effect

# Add/remove branches you want to test
sudo vim _dev-merged.txt
sudo ./scripts/make-integration-branch.sh _dev-merged.txt dev-merged ; git rev-parse --short HEAD

# Starts a py3 web node
export COMPOSE_FILE="docker-compose.yml:docker-compose.infogami-local.yml:docker-compose.staging.yml"
docker-compose down
HOSTNAME=${HOSTNAME:-$HOST} docker-compose up --no-deps -d web memcached
```

Visit [http://staging.openlibrary.org/status](http://staging.openlibrary.org/status) to test the server.

### /opt/openlibrary/scripts/run\_olserver.sh

```bash
cd /opt/openlibrary
sudo git pull origin master
cd /opt/openlibrary/vendor/infogami
sudo git pull origin master
cd /opt/olsystem
sudo git pull origin master
cd /opt/openlibrary

# $SERVICE options: web, covers, infobase, home
export SERVICE=${SERVICE:-web}
echo "Starting $SERVICE"
cd /opt/openlibrary
sudo docker-compose build --pull $SERVICE
sudo docker-compose down
sudo docker-compose up -d --no-deps memcached
HOSTNAME=${HOSTNAME:-$HOST} sudo docker-compose \
    -f docker-compose.yml \
    -f docker-compose.infogami-local.yml \
    -f docker-compose.production.yml \
    up -d --no-deps $SERVICE
# sudo docker-compose logs -f --tail=10 $SERVICE
```

