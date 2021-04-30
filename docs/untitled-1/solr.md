# Solr

[Apache Solr](http://lucene.apache.org/solr/features.html) is used to power the search box, but is also used internally by the system and is intrinsic to the correct operation of the system. Some examples of places that it's used include:

* Enumerating the list of works for an author
* Enumerating the list of editions for a work
* Autocomplete by author name when editing a work and selecting authors
* Various "top" things like the top items to list in a carousel, an author's top work or subjects, etc.

Proper operation of the Open Library Solr instance requires that it be updated when authors, works, and editions are edited or the search index will not correctly reflect the underlying database.

#### Solr updater script

[Solr updater script](https://github.com/internetarchive/openlibrary/blob/master/scripts/new-solr-updater.py)

Which is configured to run as a service in production and a background process in the development [docker-compose environment](https://github.com/internetarchive/openlibrary/blob/cb167b78fac4199e57096009f1bbbdeb23ed4c72/docker/ol-docker-start.sh#L35).

To get the production solr-updater service status: `sudo status ol-solr-updater`

#### Expected behavior and timings

Changes to the Solr index are _supposed_ to be visible on the live Open Library site in no more than [about 15 minutes](https://github.com/internetarchive/openlibrary/blob/c4d877ee6410df6f70ab45718baebe52fdf366ba/openlibrary/templates/admin/solr.html#L21), but the latency depends on how far the Solr updater is behind in processing the database update queue.

The updater script looks for document changes in the Infobase logs at `http://<internal-infobase-url>/openlibrary.org/log` and has _\(possibly incomplete\)_ logic to update Solr accordingly.

The updater script checks for updates every [5 seconds](https://github.com/internetarchive/openlibrary/blob/0748d2ab0db7966fd82b0a84572edac293e08d24/scripts/new-solr-updater.py#L268) and sends an update + commit to Solr every [100 updates, or 60 seconds](https://github.com/internetarchive/openlibrary/blob/0748d2ab0db7966fd82b0a84572edac293e08d24/scripts/new-solr-updater.py#L198) whichever occurs first.

Here's a useful solr command for checking how far behind solr-updater is:

```bash
curl "ol-home:7000/openlibrary.org/log/$(cat /var/run/openlibrary/solr-update.offset)?limit=1"
```

#### Admin force Solr update endpoint

[https://openlibrary.org/admin/solr](https://openlibrary.org/admin/solr)

Check the current value of the key populated by this endpoint that the updater script checks: [https://openlibrary.org/admin/inspect/store?key=solr-force-update](https://openlibrary.org/admin/inspect/store?key=solr-force-update)

**NOTE:** this key is currently overwritten each time the form is submitted, so it is possible the updater script won't get the values if there are sequential POSTs.

#### Log locations:

* Solr updater script log: `/var/log/upstart/ol-solr-updater.log`
* Solr itself: `/var/log/tomcat6/catalina.out`

Here's a useful command for counting log output:

```bash
ssh -A ol-solr2
sudo bash
cat /var/log/tomcat6/catalina.2020-05-13.log | grep '^May' | cut -c1-24 | sed 's/:[0-9][0-9] //' | sort | uniq -c > tmp.txt
```

#### Query Solr directly on dev instance

On host: [http://0.0.0.0:18983/solr/select?wt=json&json.nl=arrarr&q=key:/authors/OL18319A](http://0.0.0.0:18983/solr/select?wt=json&json.nl=arrarr&q=key:/authors/OL18319A) Equivalent for Docker: [http://192.168.100.100:8983/solr/select?wt=json&json.nl=arrarr&q=key:/authors/OL18319A](http://192.168.100.100:8983/solr/select?wt=json&json.nl=arrarr&q=key:/authors/OL18319A) where 192.168.100.100 is the IP address returned by `docker-machine ip` on OS X or Windows.

### Creating a Solr Backup

Creates a `.tar.gz` file of the Solr index.

```bash
ssh -A ol-home

# 1. Stop solr-updater so that the index doesn't change while we make the backup
sudo supervisorctl stop solr-updater

ssh -A ol-solr2
tmux

# 2. Commit any pending docs
# check how many docs are pending
curl http://localhost:8983/solr/admin/stats.jsp | grep -A 2 Pending
# ** This didn't work earlier! It took 7+ minutes, so we just abandoned it **
curl http://localhost:8983/solr/update?commit=true

# 3. Create the backup (~35 min on 8 May 2019; ~13 GB)
ssh -A ol-home
cd /1/solr-backups
time ssh -A mek@ol-solr2 "sudo -uroot tar zcf - /var/lib/solr/data" > backup-$(date +%F).tar.gz

# 4. Restart Solr updater
ssh -A ol-home
sudo supervisorctl start solr-updater
```

## Docs from GIO - Updating the Search Engine \(notes by: Anand Chitipothu\)

Open Library uses Apache Solr for providing search functionality to the website. The Solr instance maintains its index of all the items to be search in its data directory. This index is informally called a “search index”. All book, work and author records are stored in the same search engine.

### Updating the Search Engine

Whenever a record is updated on Open Library, the corresponding entry in the search engine must be updated to get up-to-date results. Open Library has two different ways to update the search index:

#### 1. The Manual Way

The openlibrary/solr module provides a script called `update_work.py` for updating solr. Even though the script name is indicating only work, it can be used for updating even edition and author documents in solr.

To update a one or more entries manually: WARNING: be sure to use the right openlibrary.yml file…

```bash
python openlibrary/update_work.py --config openlibrary.yml /books/OL123M /works/OL234W /authors/OL45A
```

By default, the script performs an commit to the Solr. Doing a commit ensures that the changes are flushed to the disk and available to search requests from now on. However, it is very expensive operation and takes more than 5 minutes \(at the time of writing this\).

To update the documents without out committing them, add `–nocommit` flag:

```bash
python openlibrary/update_work.py --config openlibrary.yml --nocommit /books/OL123M /works/OL234W /authors/OL45A
```

#### 2. The Solr Updater

There is a script `scripts/new_solr_updater.py`, which is run as a daemon process, listens to the edits happening to the database and updates the corresponding documents in Solr.

Infobase, the system that handles the all the modifications to the system, maintains a log of all changes. It writes a JSON entry to a log file whenever something is modified in the database. It also provides an API to request these log entries.

The solr updater script uses this to get new modifications made after what it has last seen. It uses those entries to find which documents should be updated in the search engine.

While this looks like a fair approach, the solr updater script can fail at a bad record or fail at an unexpected data. When this happen the solr updater dies and starts from the same point when it comes up and thus gets into an infinite loop.

The current position of the log file consumed by the solr updated is maintained in a state file. The state file will be at /var/run/openlibrary/solr-update.offset, or any other path specified as argument to the solr updater script.

### The Updating Process

To understand what is involved in updating a record in solr, lets restrict to work search and try to visualize a work record with bunch of editions.

The work record should appear in search results, when any one the following terms are used in the search query:

* title of the work
* title of any of its editions \(could be in other languages\)
* ISBN or any other ID of the editions
* name of the the authors
* \(and some more\)

To get all this information, the solr document needs information from the following sources:

* The work record from OL database
* All the edition records belonged to that work
* All the author records belonged to that work
* Data about all the records which have been marked as redirect to any of the above records
* IA metadata for each edition having a scan

When updating multiple records at once, getting these individually might be too inefficient. So, some efforts have gone into it to make the process faster by making requests in batches whenever possible and directly take to the database to avoid middle layer overheads.

The flow will be similar for author records as well.

