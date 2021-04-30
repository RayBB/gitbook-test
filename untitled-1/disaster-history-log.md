# Disaster-History-Log

## How-To

When someone goes wrong/unplanned on openlibrary.org, it’s recommended to open a new issue of Type: Post-Mortem \(using the issue template -- which should automatically set [Type: Post-Mortem](https://github.com/internetarchive/openlibrary/issues?utf8=%E2%9C%93&q=label%3A%22Type%3A+Post-Mortem%22+) and Priority: 0 and to make sure yourself or someone else is the assignee as the designated notetaker and/or person responsible leading effort. Please also specify an Affects: label to help others search and/or get up to speed.

## History

We used to track brief notes about outages and ops issues on [https://github.com/internetarchive/openlibrary/wiki/Disaster-History-Log](https://github.com/internetarchive/openlibrary/wiki/Disaster-History-Log). The goal was a searchable log of past events \(in case of a repeat emergency\). On 2019-12-27 @cdrini + @mekarpeles decided to switch to using github issues with [Type: Post-Mortem](https://github.com/internetarchive/openlibrary/issues?utf8=%E2%9C%93&q=label%3A%22Type%3A+Post-Mortem%22+) labels to document issues as they are happening \(and to post retro reflections\).

Outage scenarios in descending order of occurrence:

## Failing BWB Price Lookups

2019-12-27 15:15 PT: `ol-web3` and `ol-web4` failing fetch betterworldbooks prices

BWB switched their product API \(e.g. '[https://products.betterworldbooks.com/service.aspx?ItemId=9780142410752](https://products.betterworldbooks.com/service.aspx?ItemId=9780142410752)'\) to only use https \(for PCI compliance reasons\) and this was breaking Open Library and `urllib2`. The issue was documented in [https://github.com/internetarchive/openlibrary/pull/2779](https://github.com/internetarchive/openlibrary/pull/2779) and we solved by switching to `requests`. When deployed, this still didn't fix on production. The solution was to upgrade `pip` on production machines and installing the latest `pyopenssl` and `cryprography`.

## OpenLibrary Blacklisted by ElasticSearch

2019-03-28 \(and the week thereof\): **Overloaded ElasticSearch** [https://docs.google.com/document/d/1toBEk01cJ0uUyfOFIxrASlFzX9aPIML3vdj4WAXrxyY/edit\#](https://docs.google.com/document/d/1toBEk01cJ0uUyfOFIxrASlFzX9aPIML3vdj4WAXrxyY/edit#) Open Library was experiencing unresponsive web heads \(ol-web3, ol-web4\) via haproxy. The reason for the outage was that recent changes have been made with Archive.org Elastic Search so that clients who make many concurrent requests get demoted and queued, so that these requests do not take priority over users making individual requests. It's hypothesized that Open Library \(as a whole\) is somehow being treated as a single client \(instead of Open Library's clients being passed through and treated as individual end-users\) and presumably this is resulting in Open Library being penalized for its high volume of searches. The end result is ol-web3 and ol-web4 search requests queue are being queued to the point of the Open Library service becoming unresponsive. Sam was able to discover the issue by attaching `strace` to a single thread of `ol-home` and notice the process was hanging on search dispatch to Archive.org Elastic Search. The solution required unblocking Elastic Search on Archive.org's side. On the ol-web3, debug went like:

```text
# Update `/olsystem/bin/upstart-service` so “function openlibrary-server” has NUM_WORKERS=1 
$ sudo supervisorctl stop openlibrary  # stop gunicorn
$ sudo netstat -ntlp  # verify nothing listening on 7071 (otherwise kill listening pid)
$ sudo supervisorctl start openlibrary  # start gunicorn in single thread mode
$ sudo strace -tt -s 500 -p $pid_of_gunicorn
# Use curl to see what's happening and then audit the strace
# revert/checkout `/olsystem/bin/upstart-service` back to normal when finished
```

## Upstart Log Massively Large

2019-09-12 19:59 - 20:52 ET: `ol-web3:/var/log/openlibrary/upstart.log` exploding in size possibly due to archive.org serving 503s ; some user disruption due to archive.org unavailability

* **19:59**: abazella notified on slack about `ol-web3:/var/log/openlibrary` filling up
* **20:17**: Drini investigated and found `upstart.log` was 188GB, and was spitting out:

  ```text
  2019-09-13 00:18:43 [28300] [openlibrary.core.lending] [ERROR] POST failed
  ...
  HTTPError: HTTP Error 503: Service Temporarily Unavailable
  ```

* **20:26**: Linked to archive.org outage
* **20:44**: Drini \(with buy-in from Mek\) truncated the log file: `sudo truncate upstart.log  --size 0`
* **Conclusion**: It looks like `ol-web3` is not rolling the log file correctly; that should be investigated.

## Older History

* 2019-03-29 5:00pm PST OL requests for archive.org ES getting queued until stopped
  * [https://github.com/internetarchive/openlibrary/wiki/Disaster-Recovery\#overloaded-search](https://github.com/internetarchive/openlibrary/wiki/Disaster-Recovery#overloaded-search)
* 2017-11-09 10:00pm PST
  * Situation: `ol-www1` was getting clobbered shortly after syncing it in prep for migration from Funston to Richmond \(but this was a red herring\). Spikes of traffic were forcing haproxy to 503 w/ haproxy status sQ
  * Tried:
    * Restarting nginx
    * Restarting haproxy \(helped for a short period\)
    * Updated haproxy.conf to only log errors \(this was helpful; helped us identify slow API calls\)
    * Updated haproxy.conf to increase process count for `ol-web3`, `ol-web4` \(endpoints quickly became saturated again\)
  * Resolution:
    * Identify 503 sQ using tail -f /var/log/haproxy.log on ol-www1
    * Run: `sudo tail -n 5000 /var/log/nginx/access.log | cut -d ' ' -f 1 | sort | uniq -c | sort -n | tail -n 10 | /home/samuel/work/reveal-abuse/reveal | /home/samuel/work/reveal-abuse/shownames`
      * See also: [https://git.archive.org/mek/detect-abuse](https://git.archive.org/mek/detect-abuse)
    * Blocking bad actors on `ol-www1` `nginx` in `/olsystem/etc/nginx/deny.conf`
    * Moving forward @jonah will help us implement haproxy sticktables to ban IPs for periods as use becomes abusive/disruptive
* 2017-03-17 2:30pm PDT
  * Situation: Packet-flapping on the baremetal \(running nearly all of OL's services\) took down Open Library services -- see: [https://internetarchive.slack.com/archives/C06RP0F6E/p1492465473543927](https://internetarchive.slack.com/archives/C06RP0F6E/p1492465473543927)
  * Resolution: jonah reset ports switch side, host came back
* 2017-04-01 2:00pm PDT
  * Situation: ACS4 went down, Nagios alerts triggered, Open Library borrowing failed
  * Resolution:
    * Brenton restarted ACS4 via Ganeti \(mek didn't have access\)
      * It took ~10 minutes for nagios to catch up on alerts
    * Mek restarted `ol-web3` and `ol-web4` \(as otherwise loans were failing\)
    * Outage recovery was added to status.archivelab.org
  * Learnings:
    * We need a better guide for ACS outages:
      * [http://dev.blog.archive.org/2015/01/20/recovering-from-an-acs4-related-ol-outage/](http://dev.blog.archive.org/2015/01/20/recovering-from-an-acs4-related-ol-outage/)
      * [https://wiki.archive.org/twiki/bin/view/BookServer/WebHome](https://wiki.archive.org/twiki/bin/view/BookServer/WebHome)
    * More people need access to `ganeti` and `ol-acs4`
  * Resolution\(s\):
    * Mek now has access to ACS + ganeti
* 2017-02-22 5:00am PST, 9:45am - 11:00am PST
  * Situation: Cover service was down
  * Tried:
    * Restarting cover store
    * Restarting just `ol-web3` \(a la blue green deploy\) via supervisord
    * Looked at logs, noticed problem starting gunicorn on `ol-web3`
  * Resolution: [https://internetarchive.slack.com/archives/openlibrary/p1487790396002096](https://internetarchive.slack.com/archives/openlibrary/p1487790396002096)
  * Thanks: Sam
* 2017-02-13 6:00pm PST - 6:30pm PST
  * Situation: Scheduled Maintenance w/ Trevor
  * Problems: Servers which were restarted came back up w/ permission problems \(/var/run/openlibrary missing\)
  * Resolution:
    * [we added server restart guide to deployment](Deployment#handling-server-reboot)
    * [we added docs for checking other services \(e.g. solr-updater\) which may go down](Deployment#verify-deployment)
  * Questions:
    * Why is this happening? Is some startup process nuking /var/run/openlibrary? [https://internetarchive.slack.com/archives/openlibrary/p1487113203001313](https://internetarchive.slack.com/archives/openlibrary/p1487113203001313)
  * Thanks: Brenton
* 2017-02-10 ~5:00pm PST - 5:30pm PST
  * Situation: Deploying IA/OL XAuth bridge, deployment restart failed \(`ol-web3`, `ol-web4`\)
  * Thanks: Sam, Mark, Jim, Brenton
  * Problem: Deployment process was not robust against new pip dependencies \(in this case LEPL for email validation\)
  * Learnings:
    * Changing symlinks must be done on all services \(`ol-web3`, `ol-web4`, etc\)
    * Do not reset history in git as our deploy process looks only at the most recent hash it knows about!
  * Resolution: we added blue-green deployment to our docs + [guide on adding dependencies](Deployment#satisfying-dependency-changes)
* 2016-12-23 11:50pm PST - 2016-12-24 6:30am PST
  * Situation: DNS problems [reported here](https://internetarchive.slack.com/archives/openlibrary/p1482593478000364), [debugged here](https://internetarchive.slack.com/archives/ops/p1482564582000350) and
  * Resolution: [solved here](https://internetarchive.slack.com/archives/openlibrary/p1482593478000364) -- we started documenting our deployment process
* 2016-12-22 12:30pm PST - 2016-12-22 13:20pm PST
  * Situation: DNS problem, resolved itself, [recounted here](https://internetarchive.slack.com/archives/ops/p1482440491000312)
* 2016-12-02 6:35pm PST - 2016-04-02 7:20pm PST
  * Situation: DNS problems, resolved itself, [recounted here](https://internetarchive.slack.com/archives/ops/p1480732524002014)

