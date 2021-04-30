# Production-Service-Architecture\_old

## UPDATES

* 2019: `ol-mem[0-2]` trusty VMs have been reprovisioned as `ol-mem[3-5]` following the [Provisioning Guide](https://github.com/internetarchive/openlibrary/wiki/Provisioning-Guide)

## Current Production Architecture

Today, our production service architecture consists of ~11 VMs: ![Open Library Production Architecture](https://archive.org/download/openlibrary-documentation/openlibrary-production-architecture.png)

## Current Provisioning Setup

Our current production setup process \(as of 2019\) for provisioning these 11 VMs is ostensibly **manual** and relies on a lot of manually `scp`ing directories around, as well as a separate repository called `olsystem` which contains the production configs, cron jobs, and infrastructure required to run the official openlibrary.org service.

Each of our 11 VMs are more-or-less provisioned identically:

* Every VM has an `/opt` directory containing all the "business"
* Within `/opt` there is an `openlibrary/` and a `petabox/` directory. It's very likely `/opt/petabox` is not required by all VMs, though it's not currently well understood which services may rely on it \(e.g. the `ol-home` VM makes heavy use of `olsystem` which may reference petabox\)
* `/opt/openlibrary` contains all the business logic for the Open Library project:

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

At minimum, re-provisioning a VM requires:

* setting up firewall rules and installing core packages \(e.g. git, docker\) by running an ansible playbook
* `scp`'ing over the legacy VM's `/opt` directory \(preferably as an external mountable `/1` volume which can be moved in the future\)
* Setting up `olsystem` so that its files within `/opt/openlibrary/olsystem/etc` symlink to the right locations within `/etc` 

