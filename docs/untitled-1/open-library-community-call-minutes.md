# Open-Library-Community-Call-Minutes

Open Library hosts a Weekly Community Call every Tuesday @ 10:00am PT [https://zoom.us/j/369477551](https://zoom.us/j/369477551)

**Update \(2019-02-26\):** Starting on 2019-02, our notes have moved to [this Google Doc](https://docs.google.com/document/d/16eAABOSLrmCbOkRbwVXzis2x4dz2gEQWJsXK-crDCqg/edit#)

**Update \(2020-11-20\):** Notes for 2020 are at [this Google Doc](https://docs.google.com/document/d/1FEG-bdOTsi8Q3IHgFboL_LZImN18Q4wsWWWtlV8RgKk/edit#heading=h.5e4zdvk7337x)

**Update \(2021-01-09\):** Notes for 2021 are at [this Google Doc](https://docs.google.com/document/d/1YXh4SvaY9kyI-IIYnVQPDQIeDHoDcvvqvyHfkkTfPX8)

## Table of Contents

### Minutes for 2019

* [2019-01-22 Community Call](open-library-community-call-minutes.md#2019-01-22-community-call)

### Minutes for 2018

* [2018-05-15 Community Call](open-library-community-call-minutes.md#2018-05-15-community-call)
* [2018-05-01 Community Call](open-library-community-call-minutes.md#2018-05-01-community-call)
* [2018-04-17 Community Call](open-library-community-call-minutes.md#2018-04-17-community-call)
* [2018-04-10 Community Call](open-library-community-call-minutes.md#2018-04-10-community-call)
* [2018-04-03 Community Call](open-library-community-call-minutes.md#2018-04-03-community-call)
* [2018-03-20 Community Call](open-library-community-call-minutes.md#2018-03-20-community-call)
* [2018-03-13 Community Call](open-library-community-call-minutes.md#2018-03-13-community-call)

### Minutes for 2017

* [2017-10-03 Community Call](open-library-community-call-minutes.md#2017-10-03-community-call)
* [2017-09-26 Community Call](open-library-community-call-minutes.md#2017-09-26-community-call)
* [2017-09-12 Community Call](open-library-community-call-minutes.md#2017-09-12-community-call)
* [2017-08-29 Community Call](open-library-community-call-minutes.md#2017-08-29-community-call)
* [2017-05-16 Community Call](open-library-community-call-minutes.md#2017-05-16-community-call)
* [2017-05-02 Community Call](open-library-community-call-minutes.md#2017-05-02-community-call)
* [2017-04-18 Community Call](open-library-community-call-minutes.md#2017-04-18-community-call)
* [2017-04-11 Community Call](open-library-community-call-minutes.md#2017-04-11-community-call)
* [2017-04-04 Community Call](open-library-community-call-minutes.md#2017-04-04-community-call)
* [2017-03-28 Community Call](open-library-community-call-minutes.md#2017-03-28-community-call)
* [2017-03-21 Community Call](open-library-community-call-minutes.md#2017-03-21-community-call)
* [2017-03-15 Community Call](open-library-community-call-minutes.md#2017-03-15-community-call)
* [2017-03-08 Community Call](open-library-community-call-minutes.md#2017-03-08-community-call)

## Minutes for 2019

### 2019-01-22 community call

* search re-indexing
* amazon isbn api
* IA/OL sync and orphaned works
* design status \(v2, next steps\)
* python3 next steps? infogami?
* fixing search results page + subjects page

## Minutes for 2018

### 2018-08-21

#### Docker

**Fixes**

@hornc fixed psql shut down issue \(was causing lot of time\)

**Existing Issues**

symlinks on windows causing a problem

**Next Steps**

@cdrini docker-compose for solr

#### UPC / Bookcover Lookup API

nealk

### 2018-05-15 Community Call

#### Attendees

* Salman
* Drini
* Tabish
* Mek
* Charles
* num170r

### This Week

* Salman: OL client familiarity , import records, starting with wishlist import Bots repo; adding bots \(NYT bot\). Readme. etc. Pending PR for python3 to Catharbot.
* Drini: Goodreads, Shelfari books, Goodreads Work Edition modelling and diagram -- pros and cons of approaches.
* Tabish, planning to work on social share of reading, looking for small issues to pick up Salman metions Recaptca issue as a possibility to work on.
* Mek: Creating steps required to \(a\) create canonical books pages \(b\) breaking down `wishlist` import
* Charles: PR update for merge authors undo fix, Modern Greek translation branch. FRBR modelling \(Drini confirmed with example of one ISBN with many covers\)

### Todo:

* Charles - Intro to FRBR for OL on the wiki

### Next week:

* Review roadmap and tasks in Project wiki.

### 2018-05-01 Community Call

#### Attendees

* Galen
* Tom
* Tabish
* Mek

### Wins

* @Tabish got his first PR accepted to fix vagrant \(npm ssl\)! \#942
* @Galen Mancino + @mek launched `embed` feature; \#698 \#937 \#932
* @salman-bhai will be this summer's GSoC champion!

### Todo

* @brenton to review `embed` feature blog post
* @tfmorris is investigating multi-cores for solr \(so we can test + merge @cdrini's `solr`
* @abezella, any recommendations on how \#openlibrary-g may setup a 2ndary `solr` for testing migrations?
* @mek + @Tabish investigating [https://github.com/internetarchive/openlibrary/issues/925](https://github.com/internetarchive/openlibrary/issues/925)
* @salman-bhai can we share your roadmap with the community? @Tabish would love to team w/ you on Ansible, Vagrant, Xenial upgrade!
* @mek finishing `openlibrary.php` bridge + corresponding `openlibrary` changes -- this will allow developer vagrant to connect to fake/mocked lending endpoint
* @Tabish working on fb + twitter graph metadata for the Reading Log page\(s\)
* @Galen Mancino + @mek to discuss next steps:

  -- `add a book` \(choose identifier first, fetch data accordingly? include upload -&gt; IA?\)

* @mek to draft ol wiki page to organize notes around `canonical book page`
* @mek to fix OL -&gt; IA activation to redirect to Open Library login \(using @jnelson's updates to xauth\) \#accessible-epub-g \#834
* @mek to fix `works` page to rm `loan_status` and use `availability v2` \#844

### To Be Discussed

* How can we get cover service running in developer vagrant?
* Next steps for canonical books page
* Prereqs for @cdrini's code \(i.e. re-indexing solr against updated schema\) so we can push it live!
* Upcoming blog-posts?

### 2018-04-17 Community Call

#### Attendees

* Drini
* Tabish
* Neal
* Tom
* Galen
* Mek

### Agenda

* Updates & wins in the past one week
* Open Pull Requests? \(none this week\)
* What are we working on this week
* Special topic\(s\): 
  * This week: 2020 openlibraries Wishlist
  * Next week: Merging endpoint & Writing Bots
  * Next next week: Canonical Books Page

#### Updates

* openlibrary.php updated within petabox into production + developer modes to allow mocking of lending from within OpenLibrary
* Updated [https://github.com/internetarchive/openlibrary/wiki/Writing-Bots](https://github.com/internetarchive/openlibrary/wiki/Writing-Bots) page

### 2018-04-10 Community Call

#### Attendees

* Mek
* Charles
* Galen
* Salman

#### Agenda

* Updates & wins in the past one week
* Open Pull Requests? \(none this week\)
* What are we working on this week
* Discussion about Galen's work

#### This Week

* Salman 
  * Working on updating the README and deprecating code.openlibrary.org
  * Starting the Open Library Bots and start with having a simple bot which taken in ONIX or MARC feeds and imports it into the Open Library System.

### 2018-04-03 Community Call

#### Attendees

* Mek
* Charles

#### Agenda

* Updates & wins this week
* I like, I wish
* Open Pull Requests? \(none this week\)
* What are we working on this week

### 2018-03-20 Community Call

#### Attendees

* Mek
* Salman
* Charles

#### Agenda

* Updates & wins this week \(what did we work on + discuss last week\)
  * Internal stakeholder's Q2 meeting
* Open Pull Requests
  * None this week
* **Triage** Blockers & Major bugs
  * @charles will need some solr logs soon
  * [https://github.com/internetarchive/openlibrary/issues/844](https://github.com/internetarchive/openlibrary/issues/844) -- Slow loading works \(loanstatus\) @mek
* What we're working on **this week**
  * [https://github.com/internetarchive/openlibrary/issues/869](https://github.com/internetarchive/openlibrary/issues/869) -- Importing OL Wishlist w/ jude coelho @mek

#### Triage

* went through remaining Q1 board and closed issues

#### This Week

* Salman
  * Hindi internationalization
  * Updating the internationalization \(i18n\) wiki as he does this
  * Investigating writing an ONIX parser and using the openlibrary-client: github.com/internetarchive/openlibrary/issues/865
* Charles
  * [https://github.com/internetarchive/openlibrary/issues/628](https://github.com/internetarchive/openlibrary/issues/628)
  * [https://github.com/internetarchive/openlibrary/issues/482](https://github.com/internetarchive/openlibrary/issues/482)
  * [https://github.com/internetarchive/openlibrary/issues/893](https://github.com/internetarchive/openlibrary/issues/893)
  * Answering Salman's ONIX + openlibrary-client questions 
* Mek
  * Create issue to add docstrings for openlibrary-client
  * [https://github.com/internetarchive/openlibrary/issues/844](https://github.com/internetarchive/openlibrary/issues/844) -- Slow loading works \(loanstatus\) @mek

#### Wishes

* Salman
  * Wants a wiki page for internationlization \(how to contribute\)

### 2018-03-13 Community Call

#### Attendees

* Mek\(@mekarpeles\) 
* Charles\(@hornc\)
* Salman Shah\(@salman-bhai\)
* Tom\(@tfmorris\)
* Cdrini\(@cdrini\)

#### Agenda

* Discussion regarding Issues to be prioritised and nominating Issues before the Stakeholders' meet on Thursday \(2018-03-15\). 

## Minutes for 2017

### 2017-10-03 Community Call

#### Attendees

Charles, Drini, Mek

### 2017-09-26 Community Call

#### Agenda

* homepage carousels roll-out: [https://dev.openlibrary.org](https://dev.openlibrary.org)
* olc \(`openlibrary-client`\) improvements
* hackathon postmortem + progress w/ Long Now
* Next steps for work/edition/author merges
* Wikidata next steps
* October 11 deadlines: enhanced book pages, likes v. ratings, lists
* other?

### 2017-09-12 Community Call

#### Attendees:

* Mek
* Charles

#### Ideas

* Have a banner w/ button for volunteers + show jobs \(e.g. in blue bar\)
* Have an endpoint for flagging/proposing merge of Work IDs and admin page to review \(or for now, create spreadsheet\)

#### Todos

* @mek: get @charles access to OJF server
* @mek: create mailing list for OL and add link to Openlibrary website
* @mek: create a spreadsheet for Alexis + drani's work merges + 
* \[DONE\] @mek: solr updater logs for @charles
* @charles - organize metadata tasks

1\) Trigger update of archive.org items from IA

### 2017-08-29 Community Call

#### Attendees:

* Justine De Caires @jdecked

  -- reactifying the open library experience on the refactor/beta branch

  -- exploring carousel designs

  -- confused widget \(the call to action widget on editions and works pages\)

  -- You can find design resources here! [https://github.com/internetarchive/openlibrary/wiki/Design](https://github.com/internetarchive/openlibrary/wiki/Design)

* Charles Horn @charles

  -- Unorphaned 10,000 OL editions \(attaching them to existing works\). Work + Edition IDs still need to be imported into Archive.org. Maybe we can create an approved admin endpoint w/ baked-in s3 keys to update an edition's work/edition olid on Archive.org?

* Mek @mek

  -- `feature/editions-works-availability-v2` - Working on availability v2 integration into work and editions

  -- `refactor/move-work-editions-standalone` - Proposal for new routing, moving editions table from works to its own page

### 2017-05-16 Community Call

#### Charles

* ~15k Authors being merged from VIAF
* Addressing unicode search problems, e.g. when search &gt; 500 characters
* Unicode characters that require 2-characters linked is not rendering correctly \(half characters\)

#### Neal

* Searching wb for books

#### Mek

* debugging subject carousels performance issues
* unifying work/edition

### 2017-05-02 Community Call

#### mek

* subject carousels
* mobile header redesign

### 2017-04-18 Community Call

#### Agenda

* _Done_ this week
* _Goals_ this week

#### Done this week

#### @mekarpeles

Done:

* ImportBot
* Subject classification

#### @hornc

Done:

* merging 15,000 duplicate author records from VIAF import 
* re-enable author tests and account integration tests
* removed a bunch of CDs

Todo:

* Remove recaptcha for devs, admins, bots
* Lists \(any Work or Author on list shows no editions\)
* Delete function to openlibrary-client

#### David

Todo:

* Separating api.openlibrary.org from openlibrary.org

### 2017-04-11 Community Call

#### Agenda

* David working on metatools to parse MARCs on researcher2
* Nicholas, Irene, Bernat, and Jean working on OL Redesign
* Mek working on fixing Archive.org -&gt; Open Library import, speaking w/ Good Reads
* Charles, updating Openlibrary-client and working w/ @mekarpeles on work merging

#### Updates

Open Syllabus Project - \(subject\) collection created of ~500 works which we have on OL!

#### Ideas

Adding "likes" to user lists

### 2017-04-04 Community Call

#### Agenda

1. Review Roadmap
2. Updates & Progress
3. VIAF, Wikidata, OLID \(Done\)
4. Fixing book importing \(IA -&gt; OL\)
5. Adding subject tags to works & indexing in solr
6. Redesign content ideas
7. Merging works & editions
8. Todo
9. Fixing List Works

#### Charles

Is there an authority for subjects \(Library of Congress\), LibraryThing. cc: @dvd

#### Nicholas

Going to contribute to design doc

#### Mek

Share design doc w/ \#openlibrary-g and Nicholas How do we get subjects added to works and into solr?

### 2017-03-28 Community Call

#### Agenda

* Updates: What did we get done this week? What is currently in progress?
* Blockers: What do we need the community's feedback on?
* Meetings: What meetings need to occur?

#### Attendees

Mek, Charles, Neal, Brenton, Bernat, David

#### Updates

#### Charles

Aggregating list of \(unofficial\) endpoints. Documenting merging strategy Question about bulk upload API? Does it exist?

#### Bernat

Welcome! Bernat is working on a mentorship mobile app. Suggests react + less, would like overview of feature/beta branch

#### David

Open Book genome project. Improving data we have: Using openlibrary-client tool, metatools, librarything

* Metadata format

#### Neal

Was originally downloading warc files -- give me all pdfs researcher vm w/ batch of pdfs on them

#### Brenton

#### Mek

* Met with Jean Saung about service design \(creating an ecosystem diagram for Open Library\) [https://docs.google.com/a/archive.org/document/d/1RUsUnIJM78gTr5ycewUJNwYHERBQdg\_Tv-X-OZpwtRY/edit?usp=sharing](https://docs.google.com/a/archive.org/document/d/1RUsUnIJM78gTr5ycewUJNwYHERBQdg_Tv-X-OZpwtRY/edit?usp=sharing)
* \(PR in progress\) Added remote\_ids: wikidata, VIAF, and librarything IDs to author edit page
* Mek will write script to ingest 90k Author records and link them to wikidata + VIAF
* Worked on experimental mobile design for OL + started an experimental front-end which uses existing APIs to create combined works & editions page concept
* Openlibrary-client -- created librarything API \(xisbn\) \(tool for @dvd and @charles\)

#### Meetings

* Nicholas, Irene, Mek, Bernat, Jean, Brenton -- some subset of this group should meet or contribute to the ecosystem diagram \(it would be great if Jean can describe the purpose of this doc; I'll do my best\)

### 2017-03-21 Community Call

#### Time:

Tuesday, 10:30am PDT

#### Attendance:

* brenton
* skylerbunny
* charles
* nicholas
* mek

#### Updates

**Charles**

* Has been using APIs to create bots to address data quality
* Found a way to delete works \(e.g. audio works which shouldn't exist on OL\)
* Question: Will deleting a work via API \(w/ editions\) will fail?
* Detected bad Author merges
* Recaptcha2; wants to allow people to more easily edit

1\) [Endpoints](https://github.com/internetarchive/openlibrary/wiki/Endpoints) - contributing to document obscure endpoints \(seeing which new APIs we need to build\) 2\) Administrative Document - documenting processes & practices for merging works, etc. 3\) Updating openlibrary-client 4\) Cross-Linking Authors & Works \(VIAF, Wikidata, OL\) w/ David

**Skyler**

* Cares about the mission 1-web-page-per-book
* Partner w/ Charles on discovering user issues and metadata opportunities

**Mek**

* Completing rollout of repaired OPDS
* Looking for designers to help w/ beta branch \(separating FE and BE\)

### 2017-03-15 Community Call

#### Updates:

1\) Vagrant Fixes \(auth\) 2\) Removed link to kindle download \(broken\) 3\) Bots & admin features 4\) Metadata prep for @dvd 5\) OPDS this week 6\) TODO: fixing Lists \(works shows 0 editions\)

#### Folks:

* Nicholas \(ios, product; wants redesign\)
* Irene \(product, business\)
* David \(book metadata, search, systems\)
* Brenton, Mek \(OL\)

#### Todos, opportunities, and blockers:

1\) Getting David access to MARCs, the metatools repo, and a very short overview of the tools / process 2\) Getting @nicholaskinloch and @irenem access to data \(happy to do an in person google analytics session, or see what we need to do in order to get the data y'all need\) 3\) Following up with @charles about administrative metadata features \(possibly updating openlibrary-client to have special functions for `bot` users, like work merge\) 5\) Following up with David re: List bug \(Works show 0 editions\) 6\) Getting a design group together + making Open Library responsive 7\) Entertaining the idea of letting users answer questions / customize their accounts \(e.g. location, age, etc\) to help with recommendations 8\) Catching up w/ @skylerbunny to identify other problems/bugs of interest

### 2017-03-08 Community Call

Attendees:

* @mek, @charles

#### Blocking problems

@mek needs to fix vagrant researcher VM to work w/ updated auth; login w/ email

#### Discussing Q2 and Q3 Roadmap

See: [https://github.com/internetarchive/openlibrary/wiki/Roadmap](https://github.com/internetarchive/openlibrary/wiki/Roadmap)

Note:

* Charles needs a bot with admin access
* Current git has to our openlibrary page \(dev\) -- what version are we running? \(use git tag\)
* @skylerbunny's PR for prioritizing oclc &gt; isbn
* David and charles access to VM for MARC record parsing cares about pre-1600 books

#### What are we missing from being a good authoritative source of data?

* general librarianship \(no standard\) -- standard geographical location \[early books, foreign language books\] -- English is the language of metadata
* supporting multiple languages \(accurate records\)
* allow upload on Open Library?... \(to archive.org\)
* ask partners for title page scans \(from partners\) + book covers
* Wing numbers as an identifier 
* Grouping by publisher -- use subject "publisher"

openlibrary-g works better than open library mailing list

#### Opportunities:

* Author merges

Let's create a button for: \[books I own \| books I've read \| books I want\] private lists which are private

Stack-overflow for Open Library Wikipedia - every page \[Work\] has a discussion section

#### Ideas

Subscribe to changes on pages A user chat page or messaging Badges and karma for librarians \(discogs.com has this\) \[gameify\]

* number of edits
* works added
* librarian badge
* genre bags
* how many books you've read

How do we surface the style guides for Open Library?

#### Standards document:

* auto validation on a title \(warning of data is invalid\)

Like wikipedia, have a banner which shows which pages need flag desired data as missing -- request a change How can users request they want to know a field? !!!

