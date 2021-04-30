# Orphaned-Editions-Planning

## 5 Million Orphans

"Orphaned Editions" are Open Library book records that only have an edition item \(OL..M\) and no corresponding work \(OL..W\). Search results list works, and the main landing page for a book is a work page. One work can have many editions. Editions without works break the data model and cause various problems with search, links, and syncing data. There is a hack to make search mostly work with these orphans where the edition is indexed as if it is a work. No current process is creating new orphans, but there exist 5 million from the early days of Open Library. An ongoing task has been to find a way to reduce and fix these completely.

![orphaned editions graph](https://docs.google.com/spreadsheets/d/e/2PACX-1vTEIOrqppehDt3GFfGwVsqefoUWkB3syOJHZLakNsf2alWkW28s8G6DaqXf58ZXiOgZZV6oIn5f78Fz/pubchart?oid=1153405837&format=image)

## Feb 2019 Update

Of the remaining 5M orphaned editions:

* IA sourced orphans: 70381

  These are not getting fixed by re-importing due to various issues where the importer is preventing the import to avoid non-book items. We may want to enable an override to relax the restrictions to fix the missing work?

These remaining items appear to be blocked from re-importing due to:

Microfiche example: 'cihm\_01524'

```text
b'{"error_code": "bad-repub-state", "success": false, "error": "Prohibited Item"}'
```

These are microfiche scans, there are 3746 cihm\_ items in this category

Other examples:

```text
  veteransadminist0383unit: {"error_code": "bad-repub-state", "success": false, "error": "Prohibited Item"}
  colonnade12andiuoft: {"error_code": "item-is-serial", "success": false, "error": "Invalid item"}
  deecclesiasticb00schogoog: {"error_code": "noindex-true", "success": false, "error": "Prohibited Item"}
  americanforests04natiuoft: {"error_code": "invalid-ia-identifier", "success": false, "error": "Invalid item"}
```

6277 of these ocaid items have source\_records from MARC files, so these can be re-imported that way. **\[DONE\]**

* Editions with source records in metadata: 695K

  These are books that were imported from library MARC records in the early days of Open Library, before Works were added. Re-importing these by passing their source to the `/api/import/ia` endpoint will generate the Work. This is currently in progress by a bot task. **\[DONE FEB 2019\]**

```text
18 marc:unc_catalog_marc
48 marc:bpl_marc
75 marc:wfm_bk_marc
140 marc:hollis_marc
422 marc:marc_binghamton_univ
884 marc:talis_openlibrary_contribution
1761 marc:bcl_marc
1819 marc:marc_laurentian
2152 marc:CollingswoodLibraryMarcDump10-27-2008
5179 marc:marc_miami_univ_ohio
8037 marc:marc_cca
9066 marc:SanFranPL
12091 marc:marc_university_of_toronto
25167 marc:marc_ithaca_college
33450 marc:marc_western_washington_univ
46041 marc:marc_oregon_summit_records
87970 marc:marc_upei
141227 marc:marc_records_scriblio_net
634103 marc:marc_loc_updates
```

* 1.7M items do not have ISBNs -- these seem to be very early imports from MARC records that are not captured in metadata, but are available in the history / comments. These can be fixed by checking the source from the history, and then re-importing. I am currently running a script to discover the source MARC records for these 1.7M items.
* 2.5M do not have a MARC source or an ocaid, but have ISBNs.

Of those, most seem to be imported very early on from Amazon. Some of these will also be from library MARC records where the source is not captured in metadata, and these can be distinguished by running a similar check on the comments for the initial import comment. Unfortunately, the existing Amazon lookup will NOT trigger the creation of a Work if only an edition is found. This needs to be fixed to allow these ISBN / Amazon source editions to generate works.

## 2018 Update

The information in the sections below is still accurate, but has been difficult to act upon at scale.

Recently the Import process has been updated to allow reliable re-imports of existing books from individual, or bulk, MARC record sources. Part of this revised process adds works to matched editions if they don't already exist. This means orphaned editions can be fixed by reimporting from their MARC source, or matching archive.org items by using an [import endpoint](https://github.com/internetarchive/openlibrary/wiki/Endpoints#importing).

This is a huge win, and will allow the editions-without-works problem to be solved finally, once we re-process all the data.

### New process drawing on the plans below

For each category identified below, in priority order, filter the orphan edition records from the OL db dumps, determine the original source from `source_records` and perform the appropriate [re-import request](https://github.com/internetarchive/openlibrary/wiki/Endpoints#importing).

## Planning

Query to extract all orphans from OL edition dump \(25M records\). \[**DONE!**\] Extract 5M orphans.

### Gather statistics, divide into groups.

### Focus on orphans with OCAIDs because:

these are the records that are borrowable / readable and their discoverability is adversely affected by not being associated to works. Has implications for search engine indexing, and the v2 availability API.

**First priority:** Duplicate OCAID orphans. These have two issues -- not associated with any work, and are duplicate records which should be merged down. These can be matched with existing records that have works and turned into redirects, or grouped together and have a single work created for the best record. **COMPLETED**

**Second stage** Merge down remaining duplicate OCAID works. There are as of August 2017 60,786 identified groups of duplicate works, so at least twice that many records. These groupings need further checking to determine the safest way to merge the records down.

| Groupings | Number of OCAIDs | Total number of works/editions |
| :--- | ---: | ---: |
| Doubled | 47,467 | 94,934 |
| Tripled | 4,761 | 14,283 |
| Quadrupled | 666 | 2,664 |
| 5+ dupes | 58 | _\(at least\)_ 290 |
| **TOTAL** | 52,952 | 112,171 |

**Third stage** Clean out any bad records, spam / bad data / non-book items in the list of orphans to reduce identification and merge workload.

**Fourth stage** The 59% of 192K orphaned OCAIDs with unknown authors need to be checked to see if author information is available from the original records and author / editor data added where possible. Then existing ids can be used to find matches using the methods outlined below. Once all ids and author/title matching attempts are completed, any remaining editions can have works created.

In the April 2017 data dump there were 4067 duplicate OCAID orphans. In the August 2017 dump there were 7 after the processing and merging by the CleanupBot. These 7 have since been manually merged, so there are no more editions in this class.

Regenerate statistics for the classes shown in my original graphs for the April data dump. Highlight improvements.

From the April 2017 graph:

|  | April | August | Improvement |
| :--- | ---: | ---: | ---: |
| duplicate OCAIDs | 130K | 128K | 2K |
| orphaned dupe OCAIDs | 4067 | 0 | 4K |
| other orphaned OCAIDs | 219K | 192K | 27K |

**Output** Method to automatically generate the data splits and statistics from any edition dump.

### Percentage of orphans with Author information \(August 2017 dump\)

| Group | Authors |
| :--- | :--- |
| All 25M Editions | 20M editions have author information. |
| All 5M Orphans | 1.2M orphans have authors. |
| Orphans with OCAIDs \(192,800\) | 79K orphans with OCAIDs have authors. |

### What were the sources of orphan records?

**Theory:** these were the early imports and user edits before OL automatically created works. Suspect creation dates to be clustered around 2009. Need to confirm this by collected created\_date statistics.

Sources: from Amazon records? \(especially from 2008\), from IA MARC records?

* Count and graph sources
* Count and graph created\_dates

... insert created dates graph ...

### Operations to resolve orphans

* Delete, if the record is purely bad data
* Redirect, if the edition is an exact duplicate of another edition correctly associated with a work \(this happens frequently -- there were often double imports in the early days of OL\)
* Associate with existing work
* Create new work and associate, if there are really no existing works or editions

The Duplicate OCAID orphans had some records that required two orphans to be merged \(\(2\) Redirect above\), and then a new work had to be created \(\(4\) Create new work\) to resolve the issues. Now that those records have been cleared, only one of the above solutions should be sufficient to resolve any remaining orphaned edition.

### Basic process

For each valid orphan we want to determine whether we need to create a new work, or associate it with an existing one.

### Merge Technique

Use unique ids on orphaned editions to find a matching edition already associated with a work,

* if a clear duplicate, convert orphan to a redirect.
* if not a clear duplicate, associate orphan with the work.

This works well if the original data is good. If the data is bad, incorrect associations can be made. Some bad ids can allow many unrelated editions to become collected under the wrong work. The mitigating factor to this is that the data was already incorrect in the database and needed to be fixed, at least this process groups like with like so the data problem should be more visible and can be eventually fixed in one step.

Work IDs to use:

* OCAID
* ISBN \(with xisbn lookups as an extension\) - xisbn is being decomissioned: [\(problem\)](https://github.com/xlcnd/isbnlib/issues/51) [\(possible solution\)](https://github.com/xlcnd/isbntools/issues/96)
* other ids: OCLCN, HTID

Author IDs to use:

* ISNI
* VIAF \(careful, these quite often conflate homonymic authors\)
* Wikidata Q number
* LCauth

#### Another approach

For orphans with author information, look for near title matches also by that author to suggest an existing work. Many near misses may vary only by punctuation marks, accents, or by articles "A ", "The ", "Le ", "L'", "Eine " etc. in variant cataloging of the same edition. Take care not conflate "Part II" vs. "Part III" or similar, which should be treated as different editions of one work. Consider also the possibility of duplicate or conflated author identifiers: "this is not the J. Smith you are looking for". A fuzzy title match and a fuzzy author match should strongly suggest the same work absent contrary information.

#### Further reduction techniques:

Identify any orphans that are simply bad data and candidates for deletion. To explore.

* Non-book items
* Blank title, low character title counts
* Garbage strings / spam
* not enough data to be worthwile. What is the threshold?

### Synchronising with the IA v2 Availability API

For every previously orphaned OCAID edition that is associated with a work, pass its id to the admin IA sync endpoint.

