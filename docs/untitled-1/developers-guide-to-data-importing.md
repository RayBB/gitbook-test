# Developer's-Guide-to-Data-Importing

For programmatically bulk importing new books and authors into Open Library.

### Bulk Import Options

The following resources are for developers doing bulk book record creation via our APIs. If you are a librarian and you want to add a new book catalog entry, refer to the guide on [Importing a Book Record Manually](Developer%27s-Guide-to-Data-Importing.md#Import-Manually). 1. [Import by ISBN](Developer%27s-Guide-to-Data-Importing.md#Import-by-ISBN) 2. [Import by MARC](Developer%27s-Guide-to-Data-Importing.md#MARC-Records) 3. [Import by Archive.org Identifier](Developer%27s-Guide-to-Data-Importing.md#Import-by-OCAID) 4. [Import by ONIX Feeds](Processing-ONIX-Feeds) \(incomplete\)

### Procedure for Bulk Imports

1. Make sure your source data is archived and uploaded to [https://archive.org/details/ol\_data](https://archive.org/details/ol_data)
2. Add/register your source to the [https://openlibrary.org/dev/docs/data](https://openlibrary.org/dev/docs/data) document
3. [Apply for a `bot` account](https://github.com/internetarchive/openlibrary/wiki/Writing-Bots) with API access
4. Create a new bot script for [https://github.com/internetarchive/openlibrary-bots](https://github.com/internetarchive/openlibrary-bots) \(each bot gets its own directory and contains a README so others in the future can see exactly how your data was imported\)
5. Follow the [Bots Getting Started instructions](https://github.com/internetarchive/openlibrary/wiki/Writing-Bots#getting-started-rules) before running your bot.

### Historical Reference

Here's a list of sourced we've historically imported. Many of the data sets themselves are archived within the `https://archive.org/details/ol_data` collection.

* [https://openlibrary.org/dev/docs/data](https://openlibrary.org/dev/docs/data) - a list of major sources we've imported
* [https://openlibrary.org/about/help](https://openlibrary.org/about/help)

### Import Code-Paths

There are multiple paths by which data can be imported into Open Library.

1. Through the website UI and the Open Library Client which both use the endpoint: [https://openlibrary.org/books/add](https://openlibrary.org/books/add) 
   * code: [openlibrary/plugins/upstream/addbook.py](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/upstream/addbook.py) 
   * tests: [openlibrary/plugins/upstream/tests/test\_addbook.py](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/upstream/tests/test_addbook.py) although the current tests only cover the `TestSaveBookHelper` class, which is only used by the edit book pages, not addbook.
2. Through the [data import API](https://github.com/internetarchive/openlibrary/wiki/Endpoints#importing): [https://openlibrary.org/api/import](https://openlibrary.org/api/import)
   * code: [openlibrary/plugins/importapi/code.py](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/importapi/code.py)
3. By reference to archive.org items via the [IA import endpoint](https://github.com/internetarchive/openlibrary/wiki/Endpoints#importing): [https://openlibrary.org/api/import/ia](https://openlibrary.org/api/import/ia) 
   * code: [openlibrary/plugins/importapi/code.py](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/importapi/code.py) which calls `openlibrary.catalog.add_book.load()` in [openlibrary/catalog/add_book/\_\_init\_\_.py](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/catalog/add_book/__init__.py)

     Checking for existing works and editions is performed here in `openlibrary.catalog.add_book.exit_early()`

   * Add book tests: [openlibrary/catalog/add\_book/test\_add\_book.py](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/catalog/add_book/test_add_book.py)
4. Through our privileged ImportBot [scripts/manage\_imports.py](https://github.com/internetarchive/openlibrary/blob/master/scripts/manage-imports.py)  which POSTs to the IA import API via `Openlibrary.import_ocaid()` from [openlibrary/api.py](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/api.py) 
5. Through bulk import API [openlibrary/api.py](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/api.py) -- this should be considered deprecated

## Import APIs

### Import by ISBN

If you know the isbn of a book, you can import it using the `https://openlibrary.org/isbn/:isbn` API.

#### Submitting ISBNs in Bulk

Ultimately, we'd like to create an ongoing ISBN import pipeline which discovers book isbns as they are published and imports their metadata into openlibrary.org. This requires 3 steps:

1. **Discovery** - writing bots to find isbns in web content such as catalogs and APIs 
2. **Submission** - uploading these isbns to [https://archive.org/details/ol\_data](https://archive.org/details/ol_data) and processing them with @hornc's [`modern-import-bot`](https://github.com/internetarchive/openlibrary-bots/tree/master/modern-import-bot)
3. **Importing** - updating isbn import endpoints to prevent duplicating existing authors and importing undesirable content \(like CDs\)

improve a general purpose openlibrary.org endpoint which takes `isbn` \(10, 13\) and imports an item onto Open Library using our Amazon Affiliate API -- this includes logic to make sure only books \(i.e. non-book items on amazon, like CDs\) get imported and we avoid duplicating existing authors, works, and editions.

**Discovering ISBNS**

We'll want to write bots to discover isbns in web content, such as catalogs and APIs. These bots will live in a dedicated repo called `openlibrary-bots` \([https://github.com/internetarchive/openlibrary-bots](https://github.com/internetarchive/openlibrary-bots)\). Each bot will have its own directory in the project with a README.md explaining how to run the bot to harvest isbns. I imagine the community will work on several bots \(Amazon bots, Goodreads bots, Google Books bots, Better World Books bots, etc\) which continuously look at sources at some regular interval to discover new isbns.

If we plan on creating a new bot, it's probably best practice to first _open an issue_ on the `openlibrary-bots` repo explaining:

* what data source we plan on using
* what technology we’d like to use

For instance, let’s say we want to create a bot which discovers isbns from Amazon’s new arrivals: 1\) You’d create an issue on [https://github.com/internetarchive/openlibrary-bots/issues](https://github.com/internetarchive/openlibrary-bots/issues) describing how it will work, what tech we will use, what the directory will be called 2\) Once we get feedback, we’d create a directory in the project \(e.g. amazon-new-arrivals-bot\) 3\) We’d add a README.md and a code a bot which discovers isbns 4\) Create a new Archive.org item within the [https://archive.org/details/ol\_data](https://archive.org/details/ol_data) collection named after your bot or data source, where your isbns will live \(e.g. for our example `amazon-new-arrivals-bot` consider making a new item on Archive.org called `amazon-new-arrivals-isbns` within the `ol_data` collection\). Add the url of this bot's archive.org item \(where isbns will get uploaded/submitted\) to the bot README. 5\) Add a few basic unit tests to ensure the software is working correctly \(e.g. it works correctly against a static amazon page\)

**Submitting ISBNs in Bulk**

Please do not run this list of isbns directly against our [https://openlibrary.org/isbn/:isbn\`](https://openlibrary.org/isbn/:isbn`) API because it is highly rate-limited. Instead, @hornc is writing a [`modern-import-bot`](https://github.com/internetarchive/openlibrary-bots/tree/master/modern-import-bot) designed to take safely lists of isbns \(from a specified archive.org item\) and queue the isbns up to be safely imported into Open Library.

As a result, once your bot has collected a list of isbns for a certain time period, the first step is to upload the isbns to its Archive.org item \(which should have been created in step \#1 and named according to the bot which generated the isbns -- e.g. [https://archive.org/details/amazon-new-arrivals-isbns\`](https://archive.org/details/amazon-new-arrivals-isbns`) within the [https://archive.org/details/ol\_data](https://archive.org/details/ol_data) collection\) as `<date>_<source>_isbns.csv` -- e.g. `2019-06-25_amazon_isbns.csv`

**Updating ISBN Import**

In order for this entire process to work correctly and not produce bad data, we'll need to first make change to the isbn import endpoint to prevent duplication. For instance, to make sure only books \(i.e. non-book items on amazon, like CDs\) get imported and we avoid duplicating existing authors, works, and editions.

### Import by OCAID

`ocaid` stands for [Open Content Alliance Identifier](https://en.wikipedia.org/wiki/Open_Content_Alliance). The name was coined when Internet Archive and other groups institutions around the world worked together to create an ID system they could universally share for accessing books. Today, `ocaid` means the equivalent of Archive.org identifier. When you go to [https://archive.org/details/ol\_data](https://archive.org/details/ol_data), `ol_data` is the `ocaid` / Archive.org identifier. This section deals with importing Archive.org book items \(by their `ocaid` / Archive.org identifier\) into Open Library.

#### Ongoing Automatic Imports

Most qualifying Archive.org books are automatically imported into Open Library through a process calls **ImportBot**. [ImportBot](https://github.com/internetarchive/openlibrary/blob/master/scripts/manage-imports.py) is a script in the `scripts/` directory which is continuously run via a supervisorctl task on an internal machine called `ol-home`. The script makes a privileged call to [`core/ia.py`](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/core/ia.py#L358) which queries Archive.org for books which meet import criteria \(i.e. have a MARC record, have the right digitization `repub-state`, etc\). The call to `core/ia.py`'s `get_candidate_ocaids` returns a list of `ocaids` which are batch-submitted using a queue to [https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/importapi/code.py](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/importapi/code.py) -- See \#3 on [Import Code-Paths](Developer%27s-Guide-to-Data-Importing.md#Import-Code-Paths) above.

### Direct Import by OCAID

If you are sufficiently privileged, you can import an Archive.org id into Open Library using: [https://openlibrary.org/api/import/ia](https://openlibrary.org/api/import/ia)

### Bulk Import by OCAID

Assuming you have API write access, you can use ImportBot's [`import_ocaids`](https://github.com/internetarchive/openlibrary/blob/master/scripts/manage-imports.py#L67) method directly to import a list of Archive.org items into Open Library by ocaid.

**NB** As of 2019-07, the ocaid import process is being revised by @charles to relax some outdated ocaid import constraints which are currently [preventing Open Library from importing tens of thousands of valid book items and their records](https://github.com/internetarchive/openlibrary/wiki/archive.org-%E2%86%94-Open-Library-synchronisation). e.g. right now MARC records and `repub-state`s are required, which we may want to relax. Also, we run into problems when Archive.org has multiple books for the same `isbn` \(this causes a collision where a valid duplicate editions is rightfully caught by our duplication detector and prevented from being imported as a new edition\). Ideally, perhaps, editions on Open Library would have multiple `ocaids`, but we're not there yet in our data model.

### MARC Records

#### Bulk MARC Import

We are in the process of expanding the `api/import/ia` endpoint functionality.

**From a conversation with @mek + @hornc** We're going to write a `ImportBulkMarcIABot` in `openlibrary-bots`

1. For a given bulk archive.org item within ol\_data collection, iterate over all its .mrc files.
2. For each .mrc file, retrieve the length of the first binary marc using the length \(i.e. 1st\) component of the LEADER field so we know when the first MARC records stops

   e.g. [http://www.archive.org/download/trent\_test\_marc/180221\_ItemID\_Test\_001.mrc?range=0:5](http://www.archive.org/download/trent_test_marc/180221_ItemID_Test_001.mrc?range=0:5)

3. We'll use ^ with the correct item + file to get the first MARC and then use the LEADER to process the rest
4. We'll take the MARC, call the MarcBinary code in `openlibrary/catalog/marc/marc_binary.py`
5. We'll have to exposed a new endpoint within `openlibrary/plugins/importapi/code.py` for MARC import \(w/o IA book item\) 
6. Add `"local_id": ["urn:trent:12345"]`key to openlibrary book metadata in addition to correctly formatted `source_record` so archive.org can search for the records using the OL books API from

#### Importing Partner MARCs with Local ID Barcodes

**Cold Start:** If we’re creating an instance of Open Library service from scratch, before the following instructions, we’ll first need to create a new infogami `thing` called `local_ids` which is of type `page`. Next:

* A new partner named `foo` appears and their local\_id barcode is in field `999$a` within their MARCs
* As an admin, navigate to [https://openlibrary.org/local\_ids/foo.yml?m=edit](https://openlibrary.org/local_ids/foo.yml?m=edit) and set the following metadata. The following example presumes that `foo-archive-item` is the name of an archive.org item \(preferably within the `archive.org/details/ol_data` collection\) which contains one or more `.mrc` file\(s\). You can refer to [https://openlibrary.org/local\_ids/trent.yml](https://openlibrary.org/local_ids/trent.yml) for a live example.

  ```text
  id_location: 999$a
  key: /local_ids/foo
  name: Foo
  source_ocaid: foo-archive-item
  type:
    key: /type/local_id
  urn_prefix: foo
  ```

* save, and you should be ready to start importing MARCs using the following instructions: [https://github.com/internetarchive/openlibrary/wiki/Endpoints\#import-by-archiveorg-reference](https://github.com/internetarchive/openlibrary/wiki/Endpoints#import-by-archiveorg-reference)

### Goals

Here is a list of generally expressed goals for importing:

* More imported books
* Quality imports
* Newer, more recent books
* More useful items, what are our users searching for and not finding?
* Ongoing, automated pipeline so OL stays \(or becomes..\) current
* Flexible, extensible import process that can import a wide range of available and new data sources
* Synchronisation with current and potential data partners \(BWB, Wikidata, etc.\)

These are high level areas. To make concrete progress we need to identify some specific goals, collect some data, identify tradeoffs, and prioritise between these categories, then break down into specific tasks.

### Metrics

In order to measure progress and gaps, we need some metrics...

#### Basic starting point -- where are we?

* How many books are we currently importing now and over the recent past?
  * graph imports per month using `add_book` query?
  * Extra details:
    * Split by source, user added vs. bot added, MARC import, ISBN import
    * Distribution by published date for current imports and sources
* What is the current distribution of published dates in Open Library?
  * TASK: create a script to extract this data \(for graphing\) from a monthly ol\_edition dump.
* What is being searched for and not found by users?
* Overlap stats with partner data \(needs requirement details\)

### Published dates in Open Library -- books with ISBN \(14M out of 25M\) @ July 2019

![Editions by publication year graph for July 2019](https://docs.google.com/spreadsheets/d/e/2PACX-1vRXTYkmE6wRV82Z-zJaCsPAJ91SBIEWOyCaPCK1U89c379_Ing4RReHsnTX_0JiShyDnySLgjd5ejKN/pubchart?oid=723822477&format=image)

### Sources of import metadata

* Libraries, MARC data
  * PROs:
    * Better metadata
    * More detail
    * Author distinction with dates
  * CONs:
    * Tend to be older books... primarily because we are not actively seeking new dumps? SFPL was the most recently extracted catalog imported \([https://archive.org/details/marc\_openlibraries\_sanfranciscopubliclibrary](https://archive.org/details/marc_openlibraries_sanfranciscopubliclibrary)\).
* Booksellers, ISBN lookup
  * PROs
    * Newer books, incentive to keep up, market driven, popular modern books emphasised.
  * CONs
    * Lighter metadata
    * Frequently include notebooks and other products with ISBNs for sale that we might want to exclude
    * Frequently include non-book products with other identifiers \(or incorrectly converted data in ISBN field\) in their catalogs
    * Bookseller catalogs are not concerned with overall catalog coherence or quality -- maximising sales goal not impacted by junk metadata entries. Offer it for sale even if it does not make sense. We OTOH want _ALL_ items to be coherent.

IDEA: Define a bookseller metadata quality threshold. Heuristics to decide whether we want to import a record?

### Quality issues

* Duplicate records \(Author, Work, Edition\)
* Conflated records
* Junk records
  * non-book items
  * other non-record items the are generated due to errors
* Corrupt data
  * Unicode conversion
  * Dates, names, ISBN10 -&gt; 13 conversion etc.
  * Misaligned metadata \(good values in incorrect fields, multiple values in one field etc.\)
  * Mismatched identifiers contribute to conflation issues

### Current state

_.. notes written, pending .._

### Other issues related to importing

* Add book UI
  * Consolidate add\_book endpoints
  * UI add book should use and leverage the import API checks and available fields for consistency and code maintainability
  * [https://github.com/internetarchive/openlibrary/issues/1163](https://github.com/internetarchive/openlibrary/issues/1163)

    **Related resources**
* Current [project board for import pipeline efforts](https://github.com/internetarchive/openlibrary/projects/16)
* [https://github.com/internetarchive/openlibrary/wiki/Developer's-Guide-to-Data-Importing](https://github.com/internetarchive/openlibrary/wiki/Developer's-Guide-to-Data-Importing)
* [https://github.com/internetarchive/openlibrary/wiki/Endpoints\#importing](https://github.com/internetarchive/openlibrary/wiki/Endpoints#importing)
* [https://github.com/internetarchive/openlibrary/wiki/Data-Source-Overview](https://github.com/internetarchive/openlibrary/wiki/Data-Source-Overview)
* [https://github.com/internetarchive/openlibrary-bots/tree/master/modern-import-bot](https://github.com/internetarchive/openlibrary-bots/tree/master/modern-import-bot)

  **Historical -- we likely want to resume the intent of these efforts:**

* [https://openlibrary.org/dev/docs/data](https://openlibrary.org/dev/docs/data)
* [https://openlibrary.org/about/help](https://openlibrary.org/about/help)
* [https://openlibrary.org/about/requirements](https://openlibrary.org/about/requirements)

