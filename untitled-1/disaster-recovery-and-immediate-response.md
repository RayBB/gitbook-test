# Disaster-Recovery-&-Immediate-Response

## Responding to a Outage

* [ ] Report outage on \#ops and \#openlibrary on Slack
* [ ] Consult the [Troubleshooting](disaster-recovery-and-immediate-response.md#Troubleshooting) section to identify likely suspects
* [ ] Check the dashboards:
  * [NAGIOS](https://monitor.archive.org/cgi-bin/nagios3/status.cgi?hostgroup=24.openlibrary&style=detail)
  * [HAProxy](https://openlibrary.org/admin?stats)
  * [https://status.archivelab.org/?admin=true](https://status.archivelab.org/?admin=true) - links to all the dashboards
  * [stats dashboard](http://ol-home.us.archive.org:8088/dashboard/)
* [ ] If the baremetal machine is hanging, contact \#ops on slack or [manually restart baremetal](https://gnt-webmgr.us.archive.org/)
* [ ] If there's a fiber outage and openlibrary.org's servers don't resolve \(even to Sorry service\), ask in \#openlibrary or \#ops for openlibrary.org to be temporarily pointed to the active Sorry server
* [ ] Create a new [postmortem](https://github.com/internetarchive/openlibrary/issues/new?assignees=&labels=Type%3A+Post-Mortem%2C+Priority%3A+0%2C+GJ%3A+Triage+Exception&template=post_mortem.md&title=) issue

## Troubleshooting

Before continuing, you may want to check our [Port-mortems](https://github.com/internetarchive/openlibrary/issues?q=is%3Aissue+label%3A%22Type%3A+Post-Mortem%22+) to see if this is a known / already solved problem.

1. If solr-updater or import-bot or deploy issue, or infobase \(API\), check `ol-home`
2. If lending information e.g. books appear as available on OL when they are waitlisted on IA, this is a freak incident w/ memcached and we'll need to ssh into each memcached \(ol-mem\*\) and `sudo service memcached restart`
3. If there's an issue with ssl termination, static assets, connecting to the website, check `ol-www1` \(which is where all traffic enters and goes into haproxy -- which also lives on this machine\). Another case is abuse, which is documented in the troubleshooting guide \(usually haproxy limits or banning via nginx `/opt/openlibrary/olsystem/etc/nginx/deny.conf`
4. If there's a database problem, sorry \(`ol-db0` primary, `ol-db1` replication, `ol-backup1`\)
5. We don't generally have solr issues, but that would be `ol-solr2` \(to be replaced w/ `ol-solr0`\)
6. If we're seeing `ol-web3` and `ol-web4` offline, it may be network, upstream, DNS, or a breaking dependency, CHECK [NAGIOS](https://monitor.archive.org/cgi-bin/nagios3/status.cgi?hostgroup=24.openlibrary&style=detail) + alert \#ops + \#openlibrary. Check the logs in `/var/log/openlibrary/` \(esp. `upstart.log`\)
7. If you notice a disk filling up rapidly or almost out of space... CREATE A BASILISK FILE \(an empty 2GB placeholder `dd`'d file that we can delete and have the ability to `ls`, etc\)
8. Look at the troubleshooting guide history
9. [Is the server having trouble after rebooting?](disaster-recovery-and-immediate-response.md#Handling_Server_Reboot)
10. [Is OpenLibrary getting slammed with traffic, crawlers, or bad actors?](disaster-recovery-and-immediate-response.md#Handling_DDOS)
11. [Is Search Overloading archive.org elastic search upstream?](disaster-recovery-and-immediate-response.md#Overloaded_Search)

## Handling DDOS

First, ssh over to `ol-www1` \(which is the entry point for all traffic\) and determine who the bad actor\(s\) are. Because we anonymize IPs, you'll first have to populate a map of anonymous IPs to IPs we can actually block:

```text
ssh -A ol-www1
netstat -n | /home/samuel/work/reveal-abuse/mktable  # XXX this should probably be added to `olsystem`, see: https://github.com/internetarchive/olsystem/issues/45
```

Then run:

```text
sudo tail -n 5000 /var/log/nginx/access.log | cut -d ' ' -f 1 | sort | uniq -c  | sort -n | tail -n 10 | /home/samuel/work/reveal-abuse/reveal | /home/samuel/work/reveal-abuse/shownames
```

At this point, you can add the IPs to /olsystem/etc/nginx/deny.conf or add classes of IPs or user-agents to `/etc/nginx/sites-available/openlibrary.conf`, e.g.:

```text
    if ($http_user_agent ~* (Slurp|Yahoo|libwww-perl|Java)) {
        return 403;
    }
```

Or, you can block on a per-IP basis in `/opt/openlibrary/olsystem/etc/nginx/deny.conf`.

## Handling Server Reboot

When `ol-web3` or `ol-web4` are restarted, there is a chance one of them may have trouble restarting. You can usually learn why by looking in /var/log/openlibrary/upstart. Chances are it will complain about upstart.log being unwritable and or there being no directory /var/run/openlibrary owned by openlibrary. First, we should find out why this happens! To fix:

```text
ssh -A ol-web3 # or ol-web4
sudo chown openlibrary:openlibrary /var/log/openlibrary/upstart.log
sudo mkdir /var/run/openlibrary
sudo chown openlibrary:openlibrary /var/run/openlibrary
```

Make sure solr-updater is up and run ol-solr-indexer.py to sync any skipped OL db -&gt; OL solr records:

```text
/olsystem/bin/olenv python /opt/openlibrary/openlibrary/scripts/ol-solr-indexer.py --config /olsystem/etc/openlibrary.yml --bookmark ol-solr-indexer.bookmark --backward --days 180
```

Restart OLBot from ol-home and then confirm it's working:

```text
sudo -u openlibrary /olsystem/bin/olenv HOME=/home/openlibrary OPENLIBRARY_RCFILE=/olsystem/etc/olrc-importbot python scripts/manage-imports.py --config /olsystem/etc/openlibrary.yml import-all >> /tmp/importer.log
```

## Out of Space

### Cleanup Deploys

Sometimes too many deploys accumulate in `/opt/openlibrary/deploys/openlibrary` on our `ol-` servers. Usually, the solution to free up space is to remove files from previous years or months. For instance, the following removes all old deploys from the year 2017 in order to free up space:

```text
ssh -A ol-web3  # etc
cd /opt/openlibrary/deploys/openlibrary
ls -lathr | grep 2017 | awk 'NF>1{print $NF}' | sudo xargs rm -r`
```

### Docker

Docker logs can take up a ton of space. @cdrini mentions one solution is: \(Truncating docker logs for container with ID d12b...\)

```text
sudo df -h - See the sizes of a bunch of things on the VM
truncate -s 0 $(docker inspect --format='{{.LogPath}}' d12b518475e1)
```

### upstart.log

There is a possibility supervisor can get confused \(perhaps related to permissions/`chown`\), and instead of rotating logs, will start writing to `/var/log/openlibrary/upstart.log` until `/dev/vda1` \(or wherever root / is mounted\) runs out of space. The solution is to restart "supervisor" \(not openlibrary via supervistorctl but supervisor itself\) on the aflicted node \(e.g. `ol-web4` in this example\):

```text
sudo service supervisor restart
```

If successful, you should see a new openlibrary.log with an update time more recent than upstart.log. One you've confirmed this, you can truncate the erroneously inflated upstart.log to free up disk space:

```text
sudo truncate upstart.log  --size 0
```

After truncating, you'll want to restart `openlibrary`, e.g.

```text
ssh ol-web4 sudo supervisorctl restart openlibrary
```

