# Provisioning-Guide

See [\#2036 Migrate all production VMs to xenial](https://github.com/internetarchive/openlibrary/issues/2036)

Updating configuration and services has happened a lot less frequently than basic code deployments. The following instructions apply to the less frequent updates that may coincide with code releases. Extra caution is required when updating production configuration and servers since there is not yet a reliable, repeatable process for performing these tasks. They are currently manual steps. We will work on automating these initial deploys with tools like Ansible and Docker where appropriate.

Required OS upgrades \(upgrading all our server from Ubuntu Trusty to Ubuntu Xenial, which are required by the end of 2019\), will be performed at some point in 2019 for all OL servers and services. We have decided to start with Memcached since it is theoretically the simplest to upgrade, and there is a pool of multiple servers that can have one swapped out with an upgrade with minimal effect \(or so the theory goes\).

## Steps to perform when making the upgrade

* ssh to the new server \(from e.g. ol-home\) to verify that the host is accessible, and is the expected host with the expected operating system for Ubuntu: `lsb_release -a`
* Test that the new memcached server is up and responsive \(memcached telnet command to verify the memcache server is accepting connections and is at the expected version, check memory and utilisation stats\)

```text
telnet ol-mem4 11211

version
stats
stats items
quit
```

* ensure the memcache pool configuration is updated and _loaded_ for both Open Library _and_ Infobase. Update and restart the appropriate services.

## Post upgrade testing

* Make a test edit to an Open Library item and confirm the record is updated as expected, and the edit history is current and shows the correct difference, with an appropriately incremented version number.
* Monitor community edits post upgrade over a period of a few days to looks for any unexpected behaviour. Since there is a pool \(some edits may be cached correctly while some have problem\), and caching issues may take some time to manifest, proving a single edit looks good is not sufficient to indicate an all clear. Multiple edits should be checked daily to rule out the possibility of less obvious issues. Check a range of data types. Editions, Works, Authors, Lists, Newly added items etc.
* Place a notification in the `#openlibrary-g` channel to notify the community that a change has been made, and ask for anyone who notices anything unusual with regards to edit changes being saved to speak out in the channel and contact @mek or @charles.

