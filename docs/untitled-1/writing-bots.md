# Writing-Bots

## Introduction

Are you frustrated with the number of metadata mistakes on OpenLibrary editions, works, authors, etc? Do you notice the same problem happening over and over? Are you seeing a lot of duplicate Authors which should be merged or fake books which should be deleted? Are there books missing from OpenLibrary that you'd like to import from other sources? These are jobs for [`bot accounts`](https://openlibrary.org/dev/docs/bots).

OpenLibrary `bot accounts` are used to make http POSTs to work, author, edition, subject, etc., pages in order to correct metadata or create/import new records \(e.g. books, authors, bookcovers\) in bulk.

## Applying for a `bot account`

Follow these 2 steps to apply for a privileged `bot account`: 1\) First, [register a new OpenLibrary account](https://openlibrary.org/account/create) which meets the following guidelines: \(a\) Your `bot account` should be **different** from your personal OpenLibrary account and \(b\) the username of your new account should end with the word "Bot" \(e.g. `WorkBot`, `ImportBot`\). These conventions allow us to [monitor / filter through `Recent Changes` by `bot`s](https://openlibrary.org/recentchanges#bots) to identify only those changes made by `bot account`s. \(Since bots tend to repeat the same small operation with high frequency, they would overwhelm the list if shown alongside edits by humans.\) 2\) Open a github issue and ask a site `admin` \(e.g. @mekarpeles or @hornc as of 2019\) to set your account to have `bot` privileges and to [add your account to the `"API" usergroup`](http://openlibrary.org/usergroup/api?m=edit)

## Getting Started: Rules

**Question** Great, I now have a bot account, I can start fixing thousands of problems I've noticed, right?

**Answer** Hold on! After you have been granted bot access, _please do not_ run a bot script to change metadata in bulk \(for more than 100 records\) until it has first been reviewed by charles \(@hornc on github\). The right process is to create a new directory for your bot within the [https://github.com/internetarchive/openlibrary-bots](https://github.com/internetarchive/openlibrary-bots) repository, to open a PR, and to add charles \(@hornc\) or myself \(@mekarpeles\) as the reviewer.

**Question** I have a metadata question about how my bot should work, a question about using the openlibrary-client, or I need a code review for my bot. Who do I ask?

**Answer** @hornc \(@charles on slack\) is our lead on metadata and is a great person to answer questions about the `openlibrary-client` and writing/registering `bots` to fix metadata or add new books to our catalog

**Question** I have written a bot, am I ready to run it?

**Answer** If you have written a bot script and think it's ready to be run, the correct procedure is to fork the [https://github.com/internetarchive/openlibrary-bots](https://github.com/internetarchive/openlibrary-bots) repository, create a new branch for your bot, add a directory in the project named according to the action your bot will perform, and then submit a PR \(pull request\) for review. Please don't run bulk modifications until @horn or @mekarpeles have reviewed and approved your script or your bot privileges may be revoked.

**Question** My bot reads metadata from a source file and then modifies records on Open Library -- should these source data files be saved somewhere?

**Answer** Yes please -- If you are writing a bot to add new or updated metadata to Open Library from a file\(s\), those files should also be committed with your script to [https://github.com/internetarchive/openlibrary-bots](https://github.com/internetarchive/openlibrary-bots)

## `openlibrary-client` Library

Instead of making POSTs to API endpoints directly using a `bot account`, the OpenLibrary community has created an official python client library called [`openlibrary-client`](https://github.com/internetarchive/openlibrary-client) which streamlines the process of making metadata updates and writing `bot`s. `Bot`s used to be written using the [http://github.com/internetarchive/openlibrary/blob/master/openlibrary/api.py](http://github.com/internetarchive/openlibrary/blob/master/openlibrary/api.py) library, but this has been deprecated in favor of the much easier \(and safer\) `openlibrary-client`.

* [Click here for instructions on installing and using the openlibrary-client](https://github.com/internetarchive/openlibrary-client).

## `Bot account` Etiquette

If possible, `bot accounts` **should not** be used directly to make 1-off changes, e.g. from the command line. Changes instead should be coded into `bot scripts` \(i.e. `bot`s\) and checked in to the [openlibrary-bots](Developer-Resources/github.com/internetarchive/openlibrary-bots) repository. This way we can look back in time and see/figure out what exact changes were made \(so we can debug if something goes wrong or if we have to modify or update and re-apply the logic\).

## Example `Bot`s

[Here's an example by @hornc of a `bot` which uses `openlibrary-client` for merging works/editions and updating records in bulk](https://github.com/hornc/catharbot).

The following are **legacy examples** which used `api.py` \(which is deprecated\) instead of `openlibrary-client`, but they give an idea of what OpenLibrary `bot`s are, what they do, how they are written, and how they are used. Please refer to the [openlibrary-client documentation](https://github.com/internetarchive/openlibrary-client) for more modern examples:

* [Daniel's IdentifierBot](http://github.com/dmontalvo/IdentifierBot/blob/master/fastadder.py)
* [Ben's Goodreads bot](https://github.com/internetarchive/openlibrary/blob/master/scripts/2010/04/import_goodreads_ids.py)
* [AMillarBot](http://openlibrary.org/people/AMillarBot)
* [oclcBot, by Bruce Washburn](http://openlibrary.org/people/oclcBot)

## `Bot` Opportunities

Migrated to [https://github.com/internetarchive/openlibrary/issues/2863](https://github.com/internetarchive/openlibrary/issues/2863)

Also see [`Ideas for new bots`](https://github.com/internetarchive/openlibrary-bots/labels/Ideas%20for%20new%20bots)

