# Production-Service-Architecture

## UPDATES

* 2019: `ol-mem[0-2]` trusty VMs have been reprovisioned as `ol-mem[3-5]` following the [Provisioning Guide](https://github.com/internetarchive/openlibrary/wiki/Provisioning-Guide)
* Dec. 2020: Migration to Docker-based deployments based on docker-compose
* Jan. 2021: Upgrade to Infogami master and drop `docker-compose.infogami-local.yml`

## Three ways to deploy:

1. localhost for Developers -- [http://localhost:8080/status](http://localhost:8080/status)
   * `export COMPOSE_FILE="docker-compose.yml:docker-compose.override.yml"`
2. dev or staging servers -- ol-dev01 is [http://staging.openlibrary.org/status](http://staging.openlibrary.org/status)
   * `export COMPOSE_FILE="docker-compose.yml:docker-compose.staging.yml"`
3. production on multiple servers -- ol-web1 and ol-web2 are [http://openlibrary.org/status](http://openlibrary.org/status)
   * `export COMPOSE_FILE="docker-compose.yml:docker-compose.production.yml"`

Once you have set `$COMPOSE_FILE`, you can:

```text
docker-compose down && PYENV_VERSION=3.8.6 docker-compose up -d && docker-compose logs -f --tail=10
```

For more details see: [https://github.com/internetarchive/openlibrary/blob/master/docker/README.md](https://github.com/internetarchive/openlibrary/blob/master/docker/README.md)

The remainder of this document will focus on production deployments.

## Current Production Architecture

Today, our production service architecture consists of the following Docker containers: 1. affiliate-server 2. covers 3. covers\_nginx 4. cron \(in progress\) 5. db 6. home 7. importbot \(in progress\) 8. infobase 9. infobase\_nginx 10. memcached 11. solr 12. solr-updater 13. web

![Open Library Production Architecture](https://archive.org/download/openlibrary-documentation/openlibrary-production-architecture.png)

## Current Provisioning Setup

Our current production setup process \(as of 2021\) for provisioning these Docker containers is **manual** and relies on a lot of manually rsync-ing images around, as well as a separate repository called `olsystem` which contains the production configs, cron jobs, and infrastructure required to run the official openlibrary.org service.

Our Docker containers are more-or-less provisioned identically:

* The `docker-compose*.yml` files at openlibrary\`s root directory contains the Docker configuration data for each container
* These containers may mount external volumes such as `olsystem`, `petabox`, and `1` to access config and shared data.
* `/opt/openlibrary` contains the business logic for the Open Library project:

  ```text
  /opt/
  /opt/petabox
  /opt/openlibrary
  /opt/openlibrary/venv  -- python virtualenv
  /opt/openlibrary/maxmind-geoip/  -- .dat file for anonymizing IPs
  /opt/openlibrary/deploys  -- history of all deploys, hash-binned by service
  /opt/openlibrary/deploys/openlibrary  -- history of openlibrary deploys
  /opt/openlibrary/deploys/olsystem  -- history of openlibrary deploys
  /opt/openlibrary/deploys/base  -- deprecated??
  /opt/openlibrary/deploys/openlibrary/openlibrary  -- active openlibrary deploy
  /opt/openlibrary/deploys/openlibrary/openlibrary  -- active olsystem deploy
  /opt/openlibrary/olsystem/  -- symlink to active olsystem: /opt/deploys/openlibrary/olsystem
  /opt/openlibrary/openlibrary -- symlink to active openlibrary: /opt/deploys/openlibrary/olsystem
  ```

At a minimum, re-provisioning a container requires:

* setting up firewall rules and installing core packages \(e.g. git, docker\) by running an ansible playbook
* rsync-ing over the legacy VM's `/opt` directory \(preferably as an external mountable `/1` volume which can be moved in the future\)
* Setting up `olsystem` so that its files within `/opt/openlibrary/olsystem/etc` symlink to the right locations within `/etc` 

