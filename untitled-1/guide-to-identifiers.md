# Guide-to-Identifiers

Best practices were discussed at [http://webservices.itcs.umich.edu/mediawiki/oaibp/index.php/IdentifyingTheResource](http://webservices.itcs.umich.edu/mediawiki/oaibp/index.php/IdentifyingTheResource) although that site has been frozen since, in mid-2007, the DLF was reabsorbed into the CLIR to form [https://www.diglib.org](https://www.diglib.org). Not sure where current best practices abide.

## Name identifiers

* olid - Open Library author record \(e.g. OL1234A, one \(or more\) records intended to refer to an author\)
* lcauth - Library of Congress authority ID \(e.g. [https://id.loc.gov/authorities/no2013090983](https://id.loc.gov/authorities/no2013090983) \)
* viaf id - Virtual International Authority File identifier, records federated from many national name authority files by the OCLC, with one \(or more\) viaf id per author \(c.f. [https://viaf.org/](https://viaf.org/)\)
* isni - International Standard Name Identifier per ISO27729, one isni per author \(c.f. [http://www.isni.org/](http://www.isni.org/) \)
* orcid - Open Researcher and Contributor ID, one orcid per author identity \(occasionally per pseudonym, c.f. [https://orcid.org](https://orcid.org) \) 
* wdt id - Wikidata item as used for a person or identity \(e.g. [https://www.wikidata.org/wiki/Q6290611](https://www.wikidata.org/wiki/Q6290611) \)

## Edition identifiers

* oclcid - OCLC control number \(many oclcids may refer to records for one edition: use the smallest number; c.f. [https://help.oclc.org/Discovery\_and\_Reference/WorldCat-org/WorldCat-org\_Item\_Details\_screen/About\_the\_Item\_Details\_screen](https://help.oclc.org/Discovery_and_Reference/WorldCat-org/WorldCat-org_Item_Details_screen/About_the_Item_Details_screen)\) 
* lccn - Library of Congress Control Number \(c.f. [https://www.loc.gov/marc/lccn-namespace.html](https://www.loc.gov/marc/lccn-namespace.html) \)
* olid - Open Library ID \(e.g. OL123W, OL234M, etc\)
* ocaid - Open Content Alliance ID \(i.e. Archive.org item identifier\)
* htid - Hathi Trust ID \(c.f. [https://www.hathitrust.org/data](https://www.hathitrust.org/data) \)
* googleid - Google Books ID \(c.f. [https://support.google.com/books/partner/answer/3474239](https://support.google.com/books/partner/answer/3474239) \)

## Open Library identifiers on Archive.org

Back in the day, we used to connect Archive.org &lt;=&gt; OpenLibrary.org items using a metadata field called `openlibrary` on Archive.org and a field called `ocaid` on OpenLibrary.org \(which stands for `Open Content Alliance ID` — see: [https://en.wikipedia.org/wiki/Open\_Content\_Alliance](https://en.wikipedia.org/wiki/Open_Content_Alliance)\).

@mek noticed these `openlibrary` IDs were very stale and had not been used for a while. So 2 new fields were added \(in attempt to deprecate `openlibrary` field on Archive.org\). These are `openlibrary_edition` and `openlibrary_work`.

If I recall @hank and @judec informed me the `openlibrary` metadata key is actually being used in certain places within our derive pipelines — this code has never been updated to use `openlibrary_edition` and `openlibrary_work`.

### Retrieving Archive.org data for an Open Library identifier

For any given `openlibrary_edition` one should be able to use this ID pull the MARC from Open Library to confirm if the metadata matches. Or even more easily — let’s say you have a book on archive.org called… `jungleauthoritat00sinc`

And its metadata lists its `openlibrary_edition` as `OL3561303M`.

If you use [https://openlibrary.org/books/OL3561303M.json](https://openlibrary.org/books/OL3561303M.json) it will return the json API data for that book and you can check the values without necessarily even needing the MARC

## OpenLibrary

```text
covers: [253146],
ocaid: "jungleauthoritat00sinc",
key: "/books/OL3561303M",
identifiers: {
  goodreads: ["54855"],
  librarything: ["3414"]
},
lccn: ["2002026536"],
isbn_10: ["039397779X"],
```

## Archive.org identifiers

```text
<isbn>9780393977790</isbn>
<isbn>039397779X</isbn>
<openlibrary>OL3561303M</openlibrary>
<external-identifier>urn:asin:039397779X</external-identifier>
<external-identifier>urn:acs6:jungleauthoritat00sinc:pdf:0c7cfd74-4178-4606-aad6-82c7dd226004</external-identifier>
<external-identifier>urn:acs6:jungleauthoritat00sinc:epub:e2b0f076-2d77-44b4-bbbd-75e14c654c2c</external-identifier>
<external-identifier>urn:oclc:record:1035882283</external-identifier>
<boxid>IA1138320</boxid>
<identifier>jungleauthoritat00sinc</identifier>
<containerid>S0022</containerid>
<identifier-access>http://archive.org/details/jungleauthoritat00sinc</identifier-access>
<identifier-ark>ark:/13960/t9n33ns5x</identifier-ark>
<oclc-id>473022932</oclc-id>
<oclc-id>492015570</oclc-id>
<oclc-id>50143929</oclc-id>
<oclc-id>611841594</oclc-id>
<oclc-id>750905675</oclc-id>
<oclc-id>845516453</oclc-id>
<oclc-id>849008372</oclc-id>
<lccn>2002026536</lccn>
```

