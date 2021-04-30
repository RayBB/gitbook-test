# Deployment-Scratchpad

### [Deployment Guide](https://github.com/internetarchive/openlibrary/wiki/Deployment-Guide#deploying-openlibrary)

#### 2021-04-21 -- Deployment

* [ ] Open a terminal tab and log into ol-home0
* [ ] Open [https://openlibrary.org/admin?stats](https://openlibrary.org/admin?stats) so that you can monitor server status
* [ ] **Warn Slack channels `openlibrary` and `openlibrary-g` of imminent downtime!**
* [ ] On `ol-home0` run `cd /opt/openlibrary && sudo git pull origin master`
* [ ] On `ol-home0` run `/opt/openlibrary/scripts/deployment/deploy.sh`
* [ ] Run `./scripts/deployment/are_servers_in_sync.sh` to ensure the three servers are in sync.
  * They were not in sync :/ Had to git pull on ol-web2. Shouldn't be an issue after PR, I believe.
* [ ] Copy static files out Docker image and put them on `ol-www1` **GO LINE BY LINE**

  ```text
  # Make a backup of static assets
  ssh -A ol-www1 'sudo cp -r /opt/openlibrary/openlibrary/static /opt/openlibrary/openlibrary/static_backup'
  STATIC_DIR=/tmp/ol-static-$(date '+%Y-%m-%d')
  docker cp $(docker create --rm oldev:latest):/openlibrary/static $STATIC_DIR
  rsync -rvz $STATIC_DIR/ ol-www1:$STATIC_DIR
  # TODO: There's another static dir!
  ssh -A ol-www1 "sudo mkdir -p /opt/openlibrary/openlibrary/static-new && sudo cp -r $STATIC_DIR/* /opt/openlibrary/openlibrary/static-new"
  ssh -A ol-www1 'sudo chown -R openlibrary:openlibrary /opt/openlibrary/openlibrary/static-new'
  ssh -A ol-www1 'sudo rm -r /opt/openlibrary/openlibrary/static && sudo mv /opt/openlibrary/openlibrary/static-new /opt/openlibrary/openlibrary/static'
  ```

* [ ] Run `/opt/openlibrary/scripts/deployment/restart_servers.sh ol-web1 ol-covers0`
* [ ] Run `/opt/openlibrary/scripts/deployment/restart_servers.sh ol-home0 && docker restart openlibrary_infobase_nginx_1`
* [ ] Run `/opt/openlibrary/scripts/deployment/restart_servers.sh ol-web2`

#### 2021-04-15 -- Deployment

* [ ] Open a terminal tab and log into ol-home0
* [ ] Open a terminal tab and log into ol-covers0
* [ ] Open a terminal tab and log into ol-web1
* [ ] Open [https://openlibrary.org/admin?stats](https://openlibrary.org/admin?stats) so that you can monitor server status
* [ ] **Warn Slack channels `openlibrary` and `openlibrary-g` of imminent downtime!**
* [ ] On `ol-home0`
  * [ ] cd /opt/openlibrary
  * [ ] sudo git checkout master && sudo git pull origin master
  * [ ] sudo make git
  * [ ] cd /opt/olsystem
  * [ ] sudo git checkout master && sudo git pull origin master  \# -- Enter GitHub userid & token
  * [ ] cd /opt/booklending\_utils
  * [ ] sudo git checkout master && sudo git pull origin master  \# -- Enter git.archive.org userid & password
  * [ ] cd /opt/openlibrary
* [ ] Repeat the same steps on `ol-covers0` and `ol-web1`  \# `covers` does not need booklending\_utils
* [ ] Run `./scripts/deployment/are_servers_in_sync.sh` to ensure the three servers are in sync.
* [ ] On `ol-home0` run `/opt/openlibrary/scripts/deployment/deploy.sh`
  * NOTE: Run sudo \(note note: it superflously rsyncs the repos over to the other clients; we might want to rm the step? Or maybe not git pull?\)
* [ ] Copy static files out Docker image and put them on `ol-www1`

  ```text
  # Make a backup of static assets
  ssh -A ol-www1 'sudo cp -r /opt/openlibrary/openlibrary/static /opt/openlibrary/openlibrary/static_backup'
  STATIC_DIR=/tmp/ol-static-$(date '+%Y-%m-%d')
  docker cp $(docker create --rm oldev:latest):/openlibrary/static $STATIC_DIR
  rsync -rvz $STATIC_DIR/ ol-www1:$STATIC_DIR
  # TODO: There's another static dir!
  ssh -A ol-www1 "sudo mkdir -p /opt/openlibrary/openlibrary/static-new && sudo cp -r $STATIC_DIR/* /opt/openlibrary/openlibrary/static-new"
  ssh -A ol-www1 'sudo chown -R openlibrary:openlibrary /opt/openlibrary/openlibrary/static-new'
  ssh -A ol-www1 'sudo rm -r /opt/openlibrary/openlibrary/static && sudo mv /opt/openlibrary/openlibrary/static-new /opt/openlibrary/openlibrary/static'
  ```

* [ ] Run `~/are_servers_in_sync.sh` to ensure the three servers have the same Docker latest.
* [ ] Deploy to ol-web2

#### 2020-03-X?

**ADD A PIECE ON ROLLBACK WITH AND WITHOUT VOLUME MOUNTS**

* [https://docs.docker.com/engine/reference/commandline/tag](https://docs.docker.com/engine/reference/commandline/tag)

  To do a rollback on `ol-home0`:

* `docker image ls`
* Write down the Docker `IMAGE ID` that you want to roll back to.
* `docker tag oldev:<IMAGE ID> oldev:latest`
* Run `/opt/openlibrary/scripts/deployment/restart_servers.sh`
* Repeat the above steps as required on `ol-covers0`, `ol-web1`, `ol-web2`

**EnvVariable $OLDEV\_DOCKER\_SHA for which Docker image to use oldev:latest vs oldev:SomeSHA**

After deploy.sh finishes successfully,

* [ ] run `/opt/openlibrary/scripts/deployment/restart_servers.sh` on:
  * [ ] ol-home0
  * [ ] ol-covers0
  * [ ] ol-web1
* [ ] [https://openlibrary.org/admin?stats](https://openlibrary.org/admin?stats) ol-web1 goes green --&gt; red --&gt; green

Browse [http://ol-web1.us.archive.org:8080/status](http://ol-web1.us.archive.org:8080/status) :

* [ ] Software version     \[???\]
* [ ] Python version     3.8.6
* [ ] Host             ol-web1.us.archive.org
* [ ] Browse other pages of the site looking for issues

Once things look stable and correct...

* [ ] Log out of ol-web1 and into ol-web2
* [ ] Sync the repos
* [ ] Run `~/are_servers_in_sync.sh` to ensure the repos and Docker latest match all other servers
* [ ] Run `/opt/openlibrary/scripts/deployment/restart_servers.sh` on ol-web2
* [ ] [https://openlibrary.org/admin?stats](https://openlibrary.org/admin?stats) ol-web2 goes green --&gt; red --&gt; green

Broswe [http://ol-web2.us.archive.org:8080/status](http://ol-web2.us.archive.org:8080/status) :

* [ ] Software version     \[???\]
* [ ] Python version     3.8.6
* [ ] Host             ol-web2.us.archive.org
* [ ] Browse other pages of the site looking for issues

### 2021-04-08 -- FAILED

HALT DEPLOYMENT: docker-compose incompatibility discussed in [\#5007](https://github.com/internetarchive/openlibrary/issues/5007)

### 2021-03-24 -- Deployment -- IMAGE ID b634610574fc -- Software ver f4f9233

* Lot's of dependency updates after turning on dependabot
* [ ] Follow 2021-03-15 gameplan below

Open an ssh tunnel; i.e. ssh -L 8080:ol-web1:8080 -qAy cclauss@sshgw-sf.us.archive.org Then you'll be able to test at localhost:8080

```text
# Make a backup of static assets
ssh -A ol-www1 'sudo cp -r /opt/openlibrary/openlibrary/static /opt/openlibrary/openlibrary/static_backup'
STATIC_DIR=/tmp/ol-static-$(date '+%Y-%m-%d')
docker cp $(docker create --rm oldev:latest):/openlibrary/static $STATIC_DIR
rsync -rvz $STATIC_DIR/ ol-www1:$STATIC_DIR
# TODO: There's another static dir!
ssh -A ol-www1 "sudo mkdir -p /opt/openlibrary/openlibrary/static-new && sudo cp -r $STATIC_DIR/* /opt/openlibrary/openlibrary/static-new"
ssh -A ol-www1 'sudo chown -R openlibrary:openlibrary /opt/openlibrary/openlibrary/static-new'
ssh -A ol-www1 'sudo rm -r /opt/openlibrary/openlibrary/static && sudo mv /opt/openlibrary/openlibrary/static-new /opt/openlibrary/openlibrary/static'
```

### 2021-03-15 -- Deployment

* [ ] Open a terminal tab and log into ol-home0
* [ ] Open a terminal tab and log into ol-covers0
* [ ] Open a terminal tab and log into ol-web1
* [ ] Open [https://openlibrary.org/admin?stats](https://openlibrary.org/admin?stats) so that you can monitor server status
* [ ] **Warn Slack channels `openlibrary` and `openlibrary-g` of imminent downtime!**
* [ ] On `ol-home0`
  * [ ] cd /opt/openlibrary
  * [ ] sudo git checkout master && sudo git pull
  * [ ] sudo make git
  * [ ] cd /opt/olsystem
  * [ ] sudo git checkout master && sudo git pull  \# -- Enter GitHub userid & token
  * [ ] cd /opt/booklending\_utils
  * [ ] sudo git checkout master && sudo git pull  \# -- Enter git.archive.org userid & password
  * [ ] cd /opt/openlibrary
* [ ] Repeat the same steps on `ol-covers0` and `ol-web1`  \# `covers` does not need booklending\_utils
* [ ] Run `~/are_servers_in_sync.sh` to ensure the three servers are in sync.
* [ ] On `ol-home0` run `/opt/openlibrary/scripts/deployment/deploy.sh`
* [ ] ~Start an old-style deploy: `ssh -A ol-home /olsystem/bin/deploy-code openlibrary`~
* [ ] Copy files out Docker image and put them on `ol-www1`
* [ ] Run `~/are_servers_in_sync.sh` to ensure the three servers have the same Docker latest.

**ADD A PIECE ON ROLLBACK WITH AND WITHOUT VOLUME MOUNTS**

* [https://docs.docker.com/engine/reference/commandline/tag](https://docs.docker.com/engine/reference/commandline/tag)

  To do a rollback on `ol-home0`:

* `docker image ls`
* Write down the Docker `IMAGE ID` that you want to roll back to.
* `docker tag oldev:<IMAGE ID> oldev:latest`
* Run `/opt/openlibrary/scripts/deployment/restart_servers.sh`
* Repeat the above steps as required on `ol-covers0`, `ol-web1`, `ol-web2`

**EnvVariable $OLDEV\_DOCKER\_SHA for which Docker image to use oldev:latest vs oldev:SomeSHA**

After deploy.sh finishes successfully,

* [ ] run `/opt/openlibrary/scripts/deployment/restart_servers.sh` on:
  * [ ] ol-home0
  * [ ] ol-covers0
  * [ ] ol-web1
* [ ] [https://openlibrary.org/admin?stats](https://openlibrary.org/admin?stats) ol-web1 goes green --&gt; red --&gt; green

Browse [http://ol-web1.us.archive.org:8080/status](http://ol-web1.us.archive.org:8080/status) :

* [ ] Software version     \[???\]
* [ ] Python version     3.8.6
* [ ] Host             ol-web1.us.archive.org
* [ ] Browse other pages of the site looking for issues

Once things look stable and correct...

* [ ] Log out of ol-web1 and into ol-web2
* [ ] Sync the repos
* [ ] Run `~/are_servers_in_sync.sh` to ensure the repos and Docker latest match all other servers
* [ ] Run `/opt/openlibrary/scripts/deployment/restart_servers.sh` on ol-web2
* [ ] [https://openlibrary.org/admin?stats](https://openlibrary.org/admin?stats) ol-web2 goes green --&gt; red --&gt; green

Broswe [http://ol-web2.us.archive.org:8080/status](http://ol-web2.us.archive.org:8080/status) :

* [ ] Software version     \[???\]
* [ ] Python version     3.8.6
* [ ] Host             ol-web2.us.archive.org
* [ ] Browse other pages of the site looking for issues

### 2021-03-04 -- Deployment...

Do old-style deploy EARLY

One cover job failed on an error -- Restarting the server cleaned it up.

Troubles on ol-home because of 1. a lack of diskspace

* Remove older files from ol-home:/opt/openlibrary/deploys/openlibrary
  1. Problems with one or more PRs in the deployment

### 2021-02-12 -- Deployment...

> Check for red flags in nagios

How do I log in???

Not confident that all repos: 1. are set to the master branch 2. have no uncommitted changes 3. `git pull` works properly

Created a script in `~cclauss/are_repos_in_sync.sh` to ensure repos are all using the same SHA on all hosts

When running deploy you must enter: 1. GitLab id and password 2. GitHub id and password

One Docker image download failed so `deploy.sh` needed to be halted and rerun.

After deploy.sh finishes successfully, run `/opt/openlibrary/scripts/deployment/restart_servers.sh` on:

* [ ] ol-home0
* [ ] ol-covers0
* [ ] ol-web1

```text
Creating openlibrary_covers_2    ... error
Creating openlibrary_memcached_1 ... done

ERROR: for openlibrary_covers_2  Cannot create container for service covers: failed to mkdir /var/lib/docker/volumes/openlibrary_ol-vendor/_data/infogami/infogami: mkdir /var/lib/docker/volumes/openlibrary_ol-vendor/_data/infogami/infogami: file exists

ERROR: for covers  Cannot create container for service covers: failed to mkdir /var/lib/docker/volumes/openlibrary_ol-vendor/_data/infogami/infogami: mkdir /var/lib/docker/volumes/openlibrary_ol-vendor/_data/infogami/infogami: file exists
Encountered errors while bringing up the project.
```

* [ ] [https://openlibrary.org/admin?stats](https://openlibrary.org/admin?stats) ol-web1 goes green --&gt; red --&gt; green

Broswe `ol-web1.us.archive.org:8080/status`:

* [x] Software version     \[c446875\]
* [x] Python version     3.8.6
* [x] Host             ol-web1.us.archive.org
* [ ] Browse -- No covers
  * Rerun `/opt/openlibrary/scripts/deployment/restart_servers.sh` on ol-covers0
  * Do Old-style deploy

Old-style deploy: `ssh -A ol-home /olsystem/bin/deploy-code openlibrary`

Once things look stable and correct...

* [ ] Run `/opt/openlibrary/scripts/deployment/restart_servers.sh` on ol-web2
* [ ] [https://openlibrary.org/admin?stats](https://openlibrary.org/admin?stats) ol-web2 goes green --&gt; red --&gt; green

Broswe [http://ol-web2.us.archive.org:8080/status](http://ol-web2.us.archive.org:8080/status) :

* [x] Software version     \[c446875\]
* [x] Python version     3.8.6
* [x] Host             ol-web2.us.archive.org
* [x] Browse -- Perfect!

DONE!

### 2021-02-04 -- Deployment...

DO NOT sudo bash \(or rsync will be unhappy\)

OL-HOME0 cd /opt/openlibrary scripts/deployment/deploy.sh Provide userid/password for git.archive.org Provide userid/token for github.com Q: Why do we get dumped into emacs? \(This was sudo bash\) 4 minutes to docker-compose build --pull web 32 seconds to docker-compose run -uroot --rm home make i18n REPOSITORY TAG IMAGE ID CREATED SIZE oldev 7dbe17f2a809697acf53766bd89d60443653260d 9c8a9bb06afc 37 seconds ago 2.87GB 4 minutes to gzip 1 minute per server to rsync image plus repos Need to time docker load

### 2021-02-02 -- Process to update the infogami submodule...

pwd \# openlibrary/vendor/infogami git pull upstream master \# Ensure we have the latest infogami git status cd ../.. git status git branch git checkout -b update-infogami git add vendor/infogami git status git commit -m"update infogami" git push --set-upstream origin update-infogami \# Create a PR and merge it

### 2021-01-25

* Using scripts/deployment
  * 4-minute docker build
  * need to time make i18n
  * 4-minute docker save
  * rsyncs fail :-\(

### 2021-01-21

* `solr-updater`
  * Needed `/opt/.petabox/dbserver`, with perms for openlibrary user +x.
  * Something weird had happened on ol-home! the .offset files had disappeared :/ Seemed like solr-updater was stuck in a restart loop. Also infobase had been running on it for ~10h. Likely at some point the entire ol-home VM was restarted, causing supervisor to restart all the processes.
  * Remove the old processes from /etc/supervisor/conf.d, and ran `sudo supervisorctl update`
  * solr-updater is running ol-home0 from 2021-01-21 ; likely missed ~10h of edits or something. No clue how long the break was.
  * To check last edit + offset processed by solr-updater on ol-home0: `docker-compose exec solr-updater cat '/solr-updater-data/solr-update.offset'`
  * To get the last record/datetime: `curl -s "ol-home0:7000/openlibrary.org/log/$(docker-compose exec solr-updater cat '/solr-updater-data/solr-update.offset')?limit=1"`
* `import-bot`
  * It launched ok, but we noticed that on both ol-home and ol-home0 we were getting loads of errors:

```text
l: https://openlibrary.org/api/import/ia. Response:
importbot_1         | 2021-01-21 18:43:06 [178] [openlibrary.importer] [INFO] sleeping for 5 seconds before next attempt.
importbot_1         | 2021-01-21 18:43:11 [178] [openlibrary.api] [INFO] POST /api/import/ia
importbot_1         | 2021-01-21 18:43:11 [178] [openlibrary.importer] [WARNING] Failed to contact OL server. error=403 Client Error: Forbidden for url: https://openlibrary.org/api/import/ia. Response:
importbot_1         | 2021-01-21 18:43:11 [178] [openlibrary.importer] [ERROR] failed with internal error
importbot_1         | 2021-01-21 18:43:11 [178] [openlibrary.imports] [INFO] set-status professioncallgi0000luca - failed internal-error None
importbot_1         | 2021-01-21 18:43:11 [178] [openlibrary.importer] [INFO] importing istanbultocairoo0000hump
importbot_1         | 2021-01-21 18:43:11 [178] [openlibrary.api] [INFO] POST /api/import/ia
importbot_1         | 2021-01-21 18:43:11 [178] [openlibrary.importer] [WARNING] Failed to contact OL server. error=403 Client Error: Forbidden for url: https://openlibrary.org/api/import/ia. Response:
importbot_1         | 2021-01-21 18:43:11 [178] [openlibrary.importer] [INFO] sleeping for 5 seconds before next attempt.
importbot_1         | 2021-01-21 18:43:16 [178] [openlibrary.api] [INFO] POST /api/import/ia
importbot_1         | 2021-01-21 18:43:17 [178] [openlibrary.importer] [WARNING] Failed to contact OL server. error=403 Client Error: Forbidden for url: https://openlibrary.org/api/import/ia. Response:
importbot_1         | 2021-01-21 18:43:17 [178] [openlibrary.importer] [INFO] sleeping for 5 seconds before next attempt.
importbot_1         | 2021-01-21 18:43:22 [178] [openlibrary.api] [INFO] POST /api/import/ia
importbot_1         | 2021-01-21 18:43:22 [178] [openlibrary.importer] [WARNING] Failed to contact OL server. error=403 Client Error: Forbidden for url: https://openlibrary.org/api/import/ia. Response:
```

```text
Response: None
2021-01-21 18:30:31 [2374] [openlibrary.importer] [ERROR] failed with internal error
2021-01-21 18:30:31 [2374] [openlibrary.imports] [INFO] set-status practicalapproac0000sime_g7a3 - failed internal-error None
0.0 (1): UPDATE import_item SET status = 'failed', error = 'internal-error', import_time = '2021-01-21T18:30:31.230473', ol_key = NULL WHERE id=2424861
2021-01-21 18:30:31 [2374] [openlibrary.importer] [INFO] importing catalogueofmisce00slei_5
2021-01-21 18:30:31 [2374] [openlibrary.api] [INFO] POST /api/import/ia
/opt/openlibrary/venv/local/lib/python2.7/site-packages/urllib3/util/ssl_.py:139: InsecurePlatformWarning: A true SSLContext object is not available. This prevents urllib3 from configuring SSL appropriately and may cause certain SSL connections to fail. You can upgrade to a newer version of Python to solve this. For more information, see https://urllib3.readthedocs.io/en/latest/advanced-usage.html#ssl-warnings
  InsecurePlatformWarning,
2021-01-21 18:30:31 [2374] [openlibrary.importer] [WARNING] Failed to contact OL server. error=403 Client Error: Forbidden for url: https://openlibrary.org/api/import/ia. Response:
2021-01-21 18:30:31 [2374] [openlibrary.importer] [INFO] sleeping for 5 seconds before next attempt.
2021-01-21 18:30:36 [2374] [openlibrary.api] [INFO] POST /api/import/ia
/opt/openlibrary/venv/local/lib/python2.7/site-packages/urllib3/util/ssl_.py:139: InsecurePlatformWarning: A true SSLContext object is not available. This prevents urllib3 from configuring SSL appropriately and may cause certain SSL connections to fail. You can upgrade to a newer version of Python to solve this. For more information, see https://urllib3.readthedocs.io/en/latest/advanced-usage.html#ssl-warnings
  InsecurePlatformWarning,
2021-01-21 18:30:36 [2374] [openlibrary.importer] [WARNING] Failed to contact OL server. error=403 Client Error: Forbidden for url: https://openlibrary.org/api/import/ia. Response:
```

### 2021-01-15 Deployment steps

Use the scripts from [https://github.com/internetarchive/openlibrary/pull/4395](https://github.com/internetarchive/openlibrary/pull/4395) 1. **ol-home0**: Run `scripts/deployment/start_production_deploy.sh`

* **NOTE:** Login problem with `booklending_utils` \(redo manually\)
  1. **ol-home0, ol-web1, ol-covers**: Run `scripts/deployment/continue_production_deploy.sh`
* Run the script only until just before the `docker-compose down` step.
* Open `https://openlibrary.org/admin?stats`
* Once all three hosts are ready, run commands from `docker-compose down` to the bottom of the script.
* Check `https://openlibrary.org/admin?stats` and `http://ol-web1.us.archive.org:8080/status`
  1. Repeat step 2 only on the host **ol-web2**

NOTE: On both `ol-web{1, 2}`, the command `docker-compose run -uroot --rm home make i18n` failed with:

```text
ERROR: An HTTP request took too long to complete. Retry with --verbose to obtain debug information.
If you encounter this issue regularly because of slow network conditions, consider setting
    COMPOSE_HTTP_TIMEOUT to a higher value (current value: 60).
```

Manually re-running the command finished in a few seconds with no warnings or errors.

THINGS WE FORGOT: Old-style install on `ol-home` -- `ssh -A ol-home /olsystem/bin/deploy-code openlibrary`

### 2021-01-09 Deployment steps

1. **ol-home0**: `sudo git pull origin master` the four repos
2. **ol-home0**: build new Docker image
3. **ol-home0**: docker save the image with both tags `:SHA` and `:latest`
   * Should we used date-time instead of `:SHA`?
4. **ol-home0**: rsync the :SHA-tagged image to ol-web{1,2}, ol-covers
5. **ol-web{1,2}, ol-covers**: docker image prune && docker load the image with both tags `:SHA` and `:latest`
6. **ol-home0**: rsync the four repos to ol-web{1,2}, ol-covers
7. **all hosts**: docker-compose up the appropriate docker services for that host

When `start_production_deploy.sh` is run on `ol-home0` it should achieve steps 1. thru 4. above.

```bash
#!/bin/bash

# https://github.com/internetarchive/openlibrary/wiki/Deployment-Scratchpad

# This script must be run on ol-home0 to start a new deployment.

if [[ ${HOSTNAME:-$HOST} != "ol-home0" ]]; then
    echo "FATAL: Must only be run on ol-home0" ;
    exit 1 ;
fi

# `sudo git pull origin master` the core Open Library repos:
# 1. https://github.com/internetarchive/olsystem
# 2. https://git.archive.org/jake/booklending_utils
# 3. https://github.com/internetarchive/openlibrary
# 4. https://github.com/internetarchive/infogami
REPO_DIRS="/opt/olsystem /opt/booklending_utils /opt/openlibrary /opt/openlibrary/vendor/infogami"
for REPO_DIR in $REPO_DIRS
do
    cd $REPO_DIR
    sudo git pull origin master
done

# These commands were run once and probably do not need to be repeated
sudo mkdir -p /opt/olimages || true
sudo chown root:staff /opt/olimages
sudo chmod g+w /opt/olimages
sudo chmod g+s /opt/olimages
docker image prune

# Build the oldev Docker production image
cd /opt/openlibrary
export COMPOSE_FILE="docker-compose.yml:docker-compose.production.yml"
docker-compose build --pull web
docker-compose run -uroot --rm home make i18n

# Add a timestamp tag to the Docker image to facilitate rapid rollback
echo "FROM oldev:latest" | docker build -t "oldev:$(date +%Y-%m-%d_%H-%M)" -
docker image ls

# Compress the image in a .tar.gz file for transfer to other hosts
cd /opt/olimages
docker save oldev:latest | gzip > oldev_latest.tar.gz \*2

# Transfer the .tar.gz image to other hosts
REMOTE_HOSTS="ol-covers0 ol-web1 ol-web2"
for REMOTE_HOST in $REMOTE_HOSTS
do
    echo "Starting rsync of oldev_latest.tar.gz to $REMOTE_HOST..."
    rsync -a --no-owner --group --verbose oldev_latest.tar.gz "$REMOTE_HOST:/opt/olimages/"
    echo "Finished rsync of oldev_latest.tar.gz to $REMOTE_HOST..."
done
```

### 2021-01-07 Deploy

THINGS WE FORGOT:

1. To down/up infobase:

```bash
export COMPOSE_FILE="docker-compose.yml:docker-compose.production.yml"
docker-compose down
docker volume rm openlibrary_ol-vendor openlibrary_ol-build openlibrary_ol-nodemodules
HOSTNAME="$HOSTNAME" docker-compose up -d --no-deps infobase infobase_nginx affiliate-server
```

1. To check for new errors on Sentry 😅
2. [ ] @mek Modify fabfile to rsync ol code repos to the new servers in their expected directories

   ```bash
   cp ol-home:/deploy/latest/opt/openlibrary ol-web1:/opt/openlibrary
   ```

3. [ ] @mek Run old-style deploy from ol-home

   ```bash
   ssh -A ol-home
   /olsystem/bin/deploy-code openlibrary
   ```

4. [ ] @mek Checkout infogami 7595ae7 on ol-home
5. [x] @cclauss ol-home0 pull latest code of all repo
6. [x] @cclauss ol-home0 build the image \*1

   ```bash
   export COMPOSE_FILE="docker-compose.yml:docker-compose.production.yml"
   docker-compose build --pull web
   docker-compose run -uroot --rm home make i18n
   ```

7. [x] @cclauss Create /opt/olimages for everything
   * [x] ol-web1
   * [x] ol-web2
   * [x] ol-covers0
   * [x] @cdrini ol-home0

```bash
sudo mkdir -p /opt/olimages
sudo chown root:staff /opt/olimages
sudo chmod g+w /opt/olimages
sudo chmod g+s /opt/olimages
```

1. [x] @cdrini Docker save image

   \`\`\`sh

   cd /opt/openlibrary

   docker image prune \# Not necessary; but cleanup

   echo "FROM oldev:latest" \| docker build -t "oldev:$\(git rev-parse HEAD\)" -

cd /opt/olimages

time docker save oldev:latest \| gzip &gt; oldev\_latest.tar.gz \*2

## ~3 min, final file 820 MB ; image in docker container ls was 2.6 GB

```text
6. [x] ~Modify fabfile to~ rsync docker image to all the hosts 

```sh
cd /opt/olimages
# each takes ~18s
time rsync -a --no-owner --group --verbose oldev_latest.tar.gz "ol-web1:/opt/olimages/"
time rsync -a --no-owner --group --verbose oldev_latest.tar.gz "ol-web2:/opt/olimages/"
time rsync -a --no-owner --group --verbose oldev_latest.tar.gz "ol-covers0:/opt/olimages/"
```

1. [x] Do old style deploy using mek's code so that it doesn't try to rsync to ol-web{1,2} ol-covers0 ol-home0 \*4
   * Put mek's fabfile.py changes into `fab_file_changes.diff`

```bash
ssh -A ol-home
/olsystem/bin/deploy-code openlibrary
```

1. \[x web1, x web2, x covers0\] @cclauss git pull latest code

```bash
cd /opt/olsystem                    && sudo git pull origin master
cd /opt/booklending_utils           && sudo git pull origin master
cd /opt/openlibrary                 && sudo git pull origin master
cd /opt/openlibrary/vendor/infogami && sudo git pull origin master
cd /opt/openlibrary
```

1. \[x web1, x web2, x covers0\] docker load on all hosts \*3
   * The new docker image should have label "SHA" as well as "latest"

     \`\`\`sh

     **~2min**

     time docker image prune

## ~4min

time docker load &lt; /opt/olimages/oldev\_latest.tar.gz

cd /opt/openlibrary echo "FROM oldev:latest" \| docker build -t "oldev:$\(git rev-parse HEAD\)" -

```text
### for node in ol-web{1,2} ol-covers0
7. [x] Down / up
```sh
export COMPOSE_FILE="docker-compose.yml:docker-compose.production.yml"
# WARNING! Moment of downtime 😬 
docker-compose down
docker volume rm openlibrary_ol-vendor openlibrary_ol-build openlibrary_ol-nodemodules
HOSTNAME="$HOSTNAME" docker-compose up -d --scale covers=2 covers_nginx memcached
```

```bash
export COMPOSE_FILE="docker-compose.yml:docker-compose.production.yml"
docker-compose down
# Remove these because they contain stale copies of the build/vendor/nodemodules files.
# Want to use the files inside the docker image since it's freshly built.
# NOTE: longer term, we likely don't want these volume mounted for production
docker volume rm openlibrary_ol-vendor openlibrary_ol-build openlibrary_ol-nodemodules
docker-compose run -uroot --rm home make i18n
HOSTNAME="$HOSTNAME" docker-compose up --no-deps -d web
```

#### Notes

1. Hit a docker error: `ERROR: Couldn't connect to docker daemon` . Was not in docker group

```text
# Make sure you're in the docker group, to avoid sudo-ing all the docker commands
sudo usermod -aG docker USER_NAME
# Exit/re-enter to take effect
```

1. We might want a date-based instead of SHA-based image label, so that this is more human-readable \(maybe `date +%Y-%m-%dT%H%M`\)
2. It looks like `docker load` reloads all the layers!

Here is `docker image ls` before:

```text
drini@ol-web1:/opt/olimages$ docker image ls
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
oldev                latest              24e7730de44e        3 weeks ago         2.55GB
openlibrary/olbase   latest              7107cde0123e        4 weeks ago         2.43GB
openlibrary/olbase   <none>              e7ad65c36ade        5 weeks ago         2.45GB
openlibrary/olbase   <none>              5bc304b7af6a        6 weeks ago         2.46GB
openlibrary/olbase   <none>              8b9ba7350023        8 weeks ago         2.46GB
openlibrary/olbase   <none>              089f94fd1626        8 weeks ago         2.45GB
```

It notes:

```text
The image oldev:latest already exists, renaming the old one with ID sha256:24e7730de44e45356224fd9ec0f58eb39ef104f5e38b28227ff100ed67a1d0f6 to empty string
Loaded image: oldev:latest
```

After:

```text
drini@ol-web1:/opt/olimages$ docker image ls
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
oldev                latest              8b1a1ec051bf        About an hour ago   2.58GB
<none>               <none>              24e7730de44e        3 weeks ago         2.55GB
openlibrary/olbase   latest              7107cde0123e        4 weeks ago         2.43GB
openlibrary/olbase   <none>              e7ad65c36ade        5 weeks ago         2.45GB
openlibrary/olbase   <none>              5bc304b7af6a        6 weeks ago         2.46GB
openlibrary/olbase   <none>              8b9ba7350023        8 weeks ago         2.46GB
openlibrary/olbase   <none>              089f94fd1626        8 weeks ago         2.45GB
```

1. TODO for @cdrini: Remove all refs to ol-solr2 in olsystem

### 2020-12-10 Deploy

THINGS I FORGOT TO DO:

* `docker-compose run -uroot --rm home make i18n` \(Added after-the-fact\)
* [x] On web{1,2} chown /booklending\_utils

  ```bash
  sudo chown 999:staff -R /opt/booklending_utils
  sudo chmod g+w -R /opt/booklending_utils
  sudo find /opt/booklending_utils -type d -exec chmod g+s {} \;
  ```

* [x] Do a classic deploy so that ol-www1 has up-to-date static files
  * [x] Confirm web{3,4} not restarted

#### for node in web{1,2}:

1. \[x, x\] Down \(down first so code in memory that's currently running is not out-of-sync with what's on the filesystem\)

   ```bash
   export COMPOSE_FILE="docker-compose.yml:docker-compose.production.yml"
   docker-compose down
   ```

2. \[x, x\] Checkout

   ```bash
   cd /opt/olsystem
   git pull origin master
   cd /opt/openlibrary
   git pull origin master
   cd /opt/openlibrary/vendor/infogami
   git pull origin master
   cd /opt/booklending_utils
   git pull origin master
   ```

3. \[x, x\] Build

   ```bash
   export COMPOSE_FILE="docker-compose.yml:docker-compose.production.yml"
   docker-compose build --pull web
   docker-compose run -uroot --rm home make i18n
   ```

4. \[x, x\] Restart

   ```bash
   export COMPOSE_FILE="docker-compose.yml:docker-compose.production.yml"
   HOSTNAME=$HOSTNAME PYENV_VERSION=3.8.6 docker-compose up -d --no-deps web
   ```

Sort of issue? On web2, got a weird issue; `git pull origin master` was not pulling that latest infogami. We think this is because of some odd funny business with .git and /openlibrary \(which contains .git\) being mounted.

* **Seems like I forgot the COMPOSE\_FILE**

Note: need to build to JS and everything else as well!

What is the status of [https://github.com/internetarchive/openlibrary/issues/2032](https://github.com/internetarchive/openlibrary/issues/2032) ?

Option: 1\) Merge your olsystem PR, and test running the old deploy code \(should do checkout on all the nodes\)

> > 2\) Manually do it

Option: 1\) Try to docker save/load, and only build on ol-home

* If we didn't mount /openlibrary...

  > > 2\) Do it manually

ol-www1 is what serves static files, so we need a way to build the js/css/components \(inside docker\), and then copy the files over to ol-www1

Creates a tar: docker save oldev:latest

??? Get to the other servers

docker load oldev.tar oldev:14f docker tag oldev:14f latest

