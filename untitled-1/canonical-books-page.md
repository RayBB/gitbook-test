# Canonical-Books-Page

Community, please make suggestions to represent your values so we can design as Canonical Books Page everyone is proud of:

## Principles

1. Our sitemap will index editions
   * Most recently added first _\[tfm -_ [_order doesn't matter_](https://www.sitemaps.org/faq.html#faq_url_position) _\]_
   * 1 edition per language per work, prioritizing books we have digitized? _\[tfm - all editions should be in the sitemap\]_
2. `/works` pages will become a redirect page to an edition \(i.e. “canonical book” page\). A user will not be able to get to a work page. An edition will always be selected / the destination for viewing book information. \[lsd - The work metadata must be editable: if work page is inaccessible to users then where?\] 
   * redirect to the “best” edition, by default
     * E.g. Borrowable + available, readable, newest, highest rated
     * Can be done dynamically, at time of hitting url
   * Allow default setting to be overridden to accommodate language, or a request for a specific edition
     * A lang= parameter could be used to tell the search results /work which edition to redirect to, i.e. as a filter/facet for desired language
3. All editions will also incorporate work data into their display \(i.e. an edition page is basically a work page with an edition selected\)
4. To see other editions of a work, we may opt to shown the “best” subset of them \(in one more many carousel\(s\)?\) on the selected edition page. Where applicable \(i.e. the edition has a work\), a user may see a comprehensive listing of all editions \(including those without ebooks\) by going to: `/works/OL...W/<title>/editions`. In this case, we can guarantee there is a work by virtue of there being multiple editions.
   * “best” may mean the same thing as defined above \(e.g. newest, available, languages\)
5. For maintainability and clarity, there will be exactly 1 template \(disregarding A/B testing\) for the “canonical book page”
6. Not all editions need a work under this approach \[tfm - I don't see how this follows. Work metadata is still needed\] 
7. The Canonical Book Page will be mobile responsive and have a mobile + desktop view
8. As a separate \(integrally related\) issue, solr search should be updated where possible to insert edition match info results to accommodate facet by language, year, etc
9. Separate but related issue: How should search results handle works/editions?

## Changes Required

1. Redirecting `/works/OL...W/<title>` to the "best" _edition_ page \(definition of "best" to be discussed\)
2. Adding `work` data \(e.g. `description`, `subject-tags`, other `edition`s\) to the _edition_ page
3. Overhauling the `Canonical Books Page` \(i.e. _edition_ page\) to have a consistent, mobile friendly design.
4. Creating a new _work's editions_ endpoint `/works/OL...W/<title>/editions` as the comprehensive listing of a work's editions.
5. Updating the _sitemap_ to include "select" editions per works \(spanning languages, availability, favoring recent date-added, etc -- again, to be discussed\)
6. Users must be able to easily choose _which_ edition they see of a work by identifiers, search result proximity, facets, etc. Getting five hundred editions of Ovid's Metamorphoses when searching for a specific ISBN is not helpful.

   **Research**

7. [A superficial record of how other sites \(GoodReads, LibraryThing, and Shelfari\) handle their Work/edition interactions](https://rawgit.com/wiki/internetarchive/openlibrary/images/works-editions%20research.svg) \(interactive SVG; links/hovering work\)

## Related

[https://internetarchive.slack.com/archives/C0ETZV72L/p1521771852000004](https://internetarchive.slack.com/archives/C0ETZV72L/p1521771852000004) [https://github.com/internetarchive/openlibrary/issues/844](https://github.com/internetarchive/openlibrary/issues/844) [https://github.com/internetarchive/openlibrary/issues/684](https://github.com/internetarchive/openlibrary/issues/684)

