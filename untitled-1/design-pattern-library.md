# Design-Pattern-Library

The Design Pattern Library audits all the different types of components that exist in the Open Library codebase. Eventually, we'd love to move these components to [open library's developer design page](https://openlibrary.org/developers/design).

## Inspiration

Inspired by [Wikimedia's style guide](https://design.wikimedia.org/style-guide/visual-style.html%20).

## How to Contribute

### Notes

1. This list is unstable and likely to change at any given time, while we refactor the front-end UI.

   If you want to help us document components, please jump in with your image uploads and documenting where these components are in use!

2. We are not looking for design changes at the current moment of time and rather would like to focus on making components easier to create. For consistency, try not to design components on a per page basis!

   Please refer to issue [1951](https://github.com/internetarchive/openlibrary/issues/1951#issuecomment-471648372) and issue [1969](https://github.com/internetarchive/openlibrary/issues/1969#issuecomment-471647053).

You can also help by contributing to the associated mega-issue [\#1092](https://github.com/internetarchive/openlibrary/issues/1092) and making sure the codebase reflects this document.

### Adding a Component

The documentation for a UI component includes:

* a `name` — in this case it’s called `Decorated-Carousel` \(required\)
* a `location` \(required\) which is an example URL or page where the component may be found on the website
* a `short description` \(optional\) of what the component is
* a `screenshot` of the UI component \(required\)

### Example

This is an example of a UI component on openlibrary.org \(one of our other projects\). ![Screen Shot 2019-03-25 at 10 27 28AMPDT](https://user-images.githubusercontent.com/978325/54941943-4e553980-4eeb-11e9-8d04-6d1c5ad9a64f.png)

## Frequently Asked Questions

### _How do I choose a name?_

If you look through the website’s css/html, you may see a class name which describes the component. This is probably the right name to include in the docs for the component, even if it’s not the best name — if you have a suggestion for a better name, _please include both_! It’s important to include the CSS name\(s\) \(even if it’s not the best name\) because this is what will help us find the component in our code.

### _How do I know what’s a component?_

Generally, a component will be a visual element which occurs repeatedly on the website \(like a special button, a dropdown menu, a box containing download options, etc\)

### _How do I add a screenshot!?_

This is actually kind of a hack, sorry! You can't drag screenshots directly into this document. Here's what I do: 1. Open a new window to [http://github.com/internetarchive/iaux/issues/new](http://github.com/internetarchive/iaux/issues/new) 2. The Issues UI **does** allow you to drag a image into the comment section of the issue! 3. When you drag and drop your screenshot into the issue, it should appear as something like `![Screen Shot 2019-03-25 at 10 50 00AMPDT](https://user-images.githubusercontent.com/978325/54942862-3aaad280-4eed-11e9-8f53-04e734db45ce.png)` 4. Copy this snippet and paste it into this Design Pattern Library in the appropriate section for the component you are documenting.

## Components

These are components on Open Library to be made modular and documented:

### Admin table

![screen shot 2019-02-04 at 4 54 07 pm](https://user-images.githubusercontent.com/148752/52247175-ad101680-289d-11e9-9ddb-821680f6818c.png) ![screen shot 2019-02-04 at 4 55 01 pm](https://user-images.githubusercontent.com/148752/52247176-ad101680-289d-11e9-9efe-919f128266fd.png)

Seen on

### Book

![screen shot 2018-09-21 at 4 25 39 pm](https://user-images.githubusercontent.com/148752/45910063-1134b100-bdbb-11e8-9b1e-aaf2982c483f.png)

Is used in the [Carousel component](design-pattern-library.md#Carousel) to display books. Note, the button inside this component is bespoke \(a book-cta\) - see \#1158.

### ContentHead

Can appear once at the top of any page.

![screen shot 2018-10-07 at 4 06 10 pm](https://user-images.githubusercontent.com/148752/46588080-f6b13780-ca4a-11e8-9217-e47b9c4252cc.png)

Consists of the page title, an inline "count" and a set of tools.

Other elements can be rendered under the tools but are not part of the component.

### CtaBtn

```text
<div class="cta-btn">CTABtn</div>
```

![screen shot 2019-01-15 at 10 25 48 pm](https://user-images.githubusercontent.com/148752/51230361-8b61e600-1914-11e9-864c-319f056e0d78.png)

![openlibrary org\_search\_q=cracking+code mode=everything](https://user-images.githubusercontent.com/978325/71931917-bd86dc00-3153-11ea-9540-dabe0f33288f.png)

#### cta-btn--unavailable

![screen shot 2019-01-15 at 10 14 54 pm](https://user-images.githubusercontent.com/148752/51229922-075b2e80-1913-11e9-9d89-f695cc1071e5.png)

#### cta-btn--available

![screen shot 2019-01-15 at 10 15 53 pm](https://user-images.githubusercontent.com/148752/51229966-37a2cd00-1913-11e9-962f-ad67a187de50.png) ![openlibrary org\_books\_OL1395236M\_Influence \(1\)](https://user-images.githubusercontent.com/978325/71932013-ea3af380-3153-11ea-8077-43a5e1ce0494.png)

```text
<div class="cta-btn cta-btn--available">CTABtn</div>
```

#### cta-btn--preview

![openlibrary org\_books\_OL18628948M\_Cracking\_the\_communication\_code](https://user-images.githubusercontent.com/978325/71932174-338b4300-3154-11ea-8c78-c7f1561316c4.png)

#### with badge

```text
<div class="cta-btn">
  Join waiting list
  <div class="cta-btn__badge">4</div>
</div>
```

![screen shot 2019-01-16 at 8 11 39 am](https://user-images.githubusercontent.com/148752/51262084-7ada5b80-1966-11e9-8345-b9c93ce4a92a.png)

[Example](https://openlibrary.org/search?q=Amy+Tan) \[Example 2\]\([https://openlibrary.org/works/OL1843755W/The\_bonesetter's\_daughter](https://openlibrary.org/works/OL1843755W/The_bonesetter's_daughter)\)

### Cover

![screen shot 2018-09-28 at 2 48 08 pm](https://user-images.githubusercontent.com/148752/46235083-8826f800-c32d-11e8-90e8-3bba12ed484d.png)

Extremely similar to the [Book](design-pattern-library.md#book) compoonent. Seen on [https://openlibrary.org/subjects/science](https://openlibrary.org/subjects/science)

Makes use of [buttonCta](design-pattern-library.md#buttonCta)

### ButtonBtn

![screen shot 2018-09-21 at 5 12 42 pm](https://user-images.githubusercontent.com/148752/45910857-9753f600-bdc1-11e8-9dd5-365237185e62.png)

As seen in component [PageBanner](design-pattern-library.md#pagebanner)

#### modifiers

If this button is not large enough for add the large modifier \('btn--large'\)

e.g. the read ebook button on \[a book page\]\([https://openlibrary.org/books/OL7885696M/Charlotte's\_Web](https://openlibrary.org/books/OL7885696M/Charlotte's_Web)\)

![screen shot 2018-11-02 at 4 20 35 pm](https://user-images.githubusercontent.com/148752/47944809-46084d80-debb-11e8-8605-3701e29136f8.png)

#### notes

See \#1158

### ButtonGeneric

![screen shot 2018-09-21 at 5 14 15 pm](https://user-images.githubusercontent.com/148752/45910888-c9655800-bdc1-11e8-997c-3bb87da9616b.png)

As seen in the sign up button! See \#1158

### ButtonGhost

Appears in the [iaBar](design-pattern-library.md#iaBar)

![screen shot 2018-09-24 at 5 44 40 pm](https://user-images.githubusercontent.com/148752/45986619-8bff0580-c021-11e8-8e77-5b3932a2e067.png)

### ButtonBrowserDefault \(was ButtonSearch\) -

It is used to submit the search query. Uses slightly modified default browser styles.

Seen - [https://openlibrary.org/search?q=harry+potter](https://openlibrary.org/search?q=harry+potter) , [https://openlibrary.org/search](https://openlibrary.org/search)

![search-button](https://user-images.githubusercontent.com/31198893/46106369-1bf5a880-c1f6-11e8-8662-d3cf18fe6e1b.png)

### ButtonCta

Styled with .cta-btn, this button is a little Dr Jekyll and Hyde in how it is defined, but it always looks similar, only varying by color.

### ButtonLink

Currently used in the [EditToolbar](design-pattern-library.md#edit-toolbar) component.

![screen shot 2018-10-11 at 9 40 32 am](https://user-images.githubusercontent.com/148752/46819895-ba772300-cd39-11e8-87bc-2eb9bff4b841.png)

### BorrowBookForm

\(please help document me!\)

### BorrowTable

A table listing ![BorrowTable](https://user-images.githubusercontent.com/148752/45578856-d4a40b00-b838-11e8-826f-9e54f2051257.png)

the table appears differently if loans are activated: ![Screen Shot 2020-03-28 at 10 56 04 AM](https://user-images.githubusercontent.com/148752/77830018-cea41100-70e2-11ea-94c1-4f9da1a2d35f.png)

Used on the following pages:

* [https://openlibrary.org/account/loans](https://openlibrary.org/account/loans)

### Carousel

Arrows are fonts rendered via the slick library. This carousel contains several [CategoryItems](design-pattern-library.md#CategoryItem)

![Carousel](https://user-images.githubusercontent.com/148752/45588422-678c8600-b8c9-11e8-9aaf-daeb1fa1774a.png)

#### Decorated-Carousel

A decorated carousel has background:

![Carousel-decorated](https://user-images.githubusercontent.com/978325/45579855-f81e8400-b83f-11e8-967c-1dd8c07ef575.png)

This is used on \[the works page\]:

\([https://openlibrary.org/works/OL69395W/Historie\_of\_the\_raigne\_of\_King\_Henry\_the\_Seventh](https://openlibrary.org/works/OL69395W/Historie_of_the_raigne_of_King_Henry_the_Seventh)\).

#### Carousel-section

A carousel section wraps a carousel and a heading.

![Carousel-section](https://user-images.githubusercontent.com/148752/45588442-9efb3280-b8c9-11e8-8b37-7dacb990cdce.png)

### CategoryItem

![screen shot 2018-09-24 at 5 40 54 pm](https://user-images.githubusercontent.com/148752/45986547-0e3afa00-c021-11e8-8883-dfc5f9cd1e3d.png)

\(appears on home page\)

### Chart

As on [subject page](https://openlibrary.org/subjects/fantasy)

![screen shot 2018-09-28 at 9 38 25 am](https://user-images.githubusercontent.com/148752/46221555-7f6cfc80-c302-11e8-880a-7163c194acac.png)

### ChartStats

As seen on [stats](https://openlibrary.org/stats)

![screen shot 2018-11-20 at 5 38 47 pm](https://user-images.githubusercontent.com/148752/48813323-31192000-eceb-11e8-9c3f-9a5525a189bf.png)

### ColorBoxOverlay

An overlay that expands images. Visible when clicking the image of the author on their page. [Author page](https://dev.openlibrary.org/authors/OL21718A/Bill_Bryson)

![Screen Shot 2019-06-22 at 14 25 17PMPDT](https://user-images.githubusercontent.com/978325/59980045-17072f00-95a5-11e9-994b-e3c4c6941a7b.png)

Also used when clicking the image of a book cover on a [book page](https://dev.openlibrary.org/books/OL3684559M/A_short_history_of_nearly_everything).

![Screen Shot 2019-06-23 at 10 51 30AMPDT](https://user-images.githubusercontent.com/978325/59980043-140c3e80-95a5-11e9-939b-16e3ff992ab8.png)

![Screen Shot 2019-06-22 at 14 25 43PMPDT](https://user-images.githubusercontent.com/978325/59980047-20909700-95a5-11e9-9708-f94f74338a85.png)

Also used when adding an item to a reading list via a Dropper component:

![screen shot 2018-09-19 at 5 32 12 pm](https://user-images.githubusercontent.com/148752/45788898-04d11c80-bc32-11e8-9ca5-864bb08704f6.png)

\(Uses rules \#colorbox, \#cboxOverlay, \#cboxWrapper\)

### dataTables

\(please help document me!\)

### Diff

Renders [a diff](https://openlibrary.org/books/OL7500626M/The_Time_Machine?b=9&a=1&_compare=Compare&m=diff)

![screen shot 2018-09-15 at 10 05 48 am](https://user-images.githubusercontent.com/148752/45588767-023b9380-b8cf-11e8-99df-2e0142fd1ef7.png)

### Edit-Toolbar

Any wiki page that can be edited will have an edit toolbar

![screen shot 2018-10-11 at 9 41 46 am](https://user-images.githubusercontent.com/148752/46819962-e5617700-cd39-11e8-8745-513c075e6b96.png)

### EditionsTable

![EditionsTable](https://user-images.githubusercontent.com/148752/45579776-6c0c5c80-b83f-11e8-968e-ab6e9681caec.png)

#### .lists variant

When inside a .lists element this renders differently. \(please help document me!\)

### HeadedLinkList

A list of links with a heading as seen on [https://openlibrary.org/subjects](https://openlibrary.org/subjects)

![screen shot 2018-09-28 at 2 14 56 pm](https://user-images.githubusercontent.com/148752/46233807-fb7a3b00-c328-11e8-85b6-64b604ecc708.png)

We currently make use of .contentQuarter elements to arrange these side by side but it would be useful to have them modeled as their own component and deal with layout separately.

See also: [LinkBox](design-pattern-library.md#linkbox)

### Manage-Covers

![screen shot 2018-11-01 at 10 11 41 pm](https://user-images.githubusercontent.com/148752/47894990-245b8780-de23-11e8-8364-ed1447477b71.png)

Shows when logged in and with JS enabled when you hover over a cover.

### Page-History

![screen shot 2018-09-15 at 9 47 20 am](https://user-images.githubusercontent.com/148752/45588626-732d7c00-b8cc-11e8-911d-358d86ce04f3.png)

shows at the bottom of any wiki page, showing the revision history of that page.

\([https://openlibrary.org/works/OL69395W/Historie\_of\_the\_raigne\_of\_King\_Henry\_the\_Seventh](https://openlibrary.org/works/OL69395W/Historie_of_the_raigne_of_King_Henry_the_Seventh)\).

#### RevisionHistoryTable

Confusingly, a similar table renders on [the revision history page](https://openlibrary.org/books/OL7500626M/The_Time_Machine?m=history)

![RevisionHistoryTable](https://user-images.githubusercontent.com/148752/45588739-82152e00-b8ce-11e8-8509-451692cfd0e9.png)

### iaBar

Internet Archive Bar: A bar that appears at the top of all pages. Makes use of a [ButtonGhost](design-pattern-library.md#ButtonGhost).

![screen shot 2018-09-24 at 5 43 24 pm](https://user-images.githubusercontent.com/148752/45986591-5e19c100-c021-11e8-9993-49d8a71b2621.png)

### Merge Form

Url: [http://localhost:8080/authors/merge?key=OL6848355A&key=OL18319A&key=OL24522A](http://localhost:8080/authors/merge?key=OL6848355A&key=OL18319A&key=OL24522A) Requires admin access

![screen shot 2019-02-24 at 9 54 06 am](https://user-images.githubusercontent.com/148752/53303008-489c0380-381a-11e9-87af-fb5e3994b9b0.png)

### MyBooks

Combines [MyBooksList](design-pattern-library.md#MyBooksList) and [MyBooksMenu](design-pattern-library.md#MyBooksMenu) In use on [https://openlibrary.org/account/books/want-to-read](https://openlibrary.org/account/books/want-to-read)

### MyBooksList

![MyBooksList](https://user-images.githubusercontent.com/148752/45579534-3cf4eb80-b83d-11e8-9891-167aeca012c7.png)

uses [SearchResultItem](design-pattern-library.md#searchresultitem)

### MyBooksMenu

![screen shot 2018-12-26 at 2 19 50 pm](https://user-images.githubusercontent.com/148752/50458626-58ca5c00-0919-11e9-9128-9f6749df13a6.png)

In use on [https://openlibrary.org/account/books/want-to-read](https://openlibrary.org/account/books/want-to-read)

### OLform

Used to render forms in OpenLibrary \(please help document me!\) Some examples:

* Search at top of [subjects page](https://openlibrary.org/subjects/science)
* the [edit a work page](https://openlibrary.org/works/OL69395W/Historie_of_the_raigne_of_King_Henry_the_Seventh/edit)

![olform](https://user-images.githubusercontent.com/148752/45588481-5132fa00-b8ca-11e8-8028-66a61c79c1db.png)

![edit olform](https://user-images.githubusercontent.com/148752/45588637-9ce6a300-b8cc-11e8-88ce-bdf91f288d40.png)

### ReadingLog-stats

Undocumented component.

Used in openlibrary/templates/stats/readinglog.html

### ReaderStats

![screen shot 2018-12-15 at 1 20 01 pm](https://user-images.githubusercontent.com/148752/50047549-4b38ea80-006c-11e9-9536-147aecdf37eb.png)

Example url: [https://openlibrary.org/books/OL16180205M/Harry\_Potter\_and\_the\_Sorcerer's\_Stone](https://openlibrary.org/books/OL16180205M/Harry_Potter_and_the_Sorcerer's_Stone)

### LinkBox

Similar to HeadedLinkList but used to show related links, which are listed inline with a smaller heading. This element does not use a ul element, but does function in a very similar way.

Seen on [https://openlibrary.org/subjects/science](https://openlibrary.org/subjects/science) and on book pages

![screen shot 2018-09-28 at 2 25 40 pm](https://user-images.githubusercontent.com/148752/46234230-81e34c80-c32a-11e8-9da6-90bde50df8d6.png)

![screen shot 2018-10-06 at 7 59 56 am](https://user-images.githubusercontent.com/148752/46572617-fb3bfa00-c93d-11e8-9344-8426b0910f00.png)

See also [HeadedLinkList](design-pattern-library.md#HeadedLinkList)

### All Pages

#### HeaderBar

The header bar appears at the top of every page.

![HeaderBar](https://user-images.githubusercontent.com/148752/45579420-50538700-b83c-11e8-81c8-02fe4f4bbd09.png)

### FlashMessage

Can be displayed on **any page** via JS to report feedback to a user after some form has been posted.

![screen shot 2018-12-13 at 6 13 32 pm](https://user-images.githubusercontent.com/148752/49979199-dac28a00-ff02-11e8-897a-bdca47ec8fe5.png)

#### FlashMessageBookAdded

![screen shot 2018-12-13 at 6 13 07 pm](https://user-images.githubusercontent.com/148752/49979201-dc8c4d80-ff02-11e8-8d3b-8a8c3741f107.png)

#### FlashMessageError

![screen shot 2018-12-13 at 6 12 42 pm](https://user-images.githubusercontent.com/148752/49979204-ddbd7a80-ff02-11e8-914b-ee9bc540d27d.png)

#### Footer

The footer appears at the top of every page.

![Footer](https://user-images.githubusercontent.com/978325/45580060-bc84b980-b841-11e8-98e1-8041c4d5be60.png)

### RatingsForm

![screen shot 2018-10-12 at 1 00 18 pm](https://user-images.githubusercontent.com/148752/46891462-d0aadf00-ce1e-11e8-93be-a4c0dafcf69c.png)

### SearchResultContainer

![screen shot 2018-11-08 at 8 30 13 pm](https://user-images.githubusercontent.com/148752/48243418-2ed2cf80-e395-11e8-9c0b-660b17dd959b.png)

[example](https://openlibrary.org/search?q=as&mode=ebooks&m=edit&m=edit&has_fulltext=true)

Contains: [\#SearchResultItem](design-pattern-library.md#SearchResultItem), [\#widgetbox](design-pattern-library.md#widgetbox) and [\#LinkBox](design-pattern-library.md#LinkBox)

### SearchResultItem

![screen shot 2018-09-15 at 9 44 34 am](https://user-images.githubusercontent.com/148752/45588596-fd291500-b8cb-11e8-9d0a-1508380f00b1.png)

[example](https://openlibrary.org/search?q=as&mode=ebooks&m=edit&m=edit&has_fulltext=true)

### SearchResultItemCTA

![screen shot 2018-12-26 at 2 32 53 pm](https://user-images.githubusercontent.com/148752/50458854-2588cc80-091b-11e9-96d4-d22631be9423.png)

e.g. [https://openlibrary.org/account/books/want-to-read](https://openlibrary.org/account/books/want-to-read)

### SearchResultsInstantResult

Shows up when you search from the header

![screen shot 2018-09-15 at 9 45 05 am](https://user-images.githubusercontent.com/148752/45588607-25b10f00-b8cc-11e8-9745-4341af79bb94.png)

### UI-Tabs / TabsPanel

A tab component. See [author page](https://openlibrary.org/authors/OL27173A/Avi/edit) for an example.

![screen shot 2018-11-21 at 8 28 22 am](https://user-images.githubusercontent.com/148752/48854732-73804280-ed67-11e8-8548-a11257fc1327.png)

#### StatsChart

Can be found on the home page. Graph rendered via an HTML canvas.

![StatsChart](https://user-images.githubusercontent.com/148752/45588471-29439680-b8ca-11e8-83ae-00ea4f9aa360.png)

### PageBanner

![1](https://user-images.githubusercontent.com/148752/45588543-3745e700-b8cb-11e8-8619-6228788dc7a5.png) ![2](https://user-images.githubusercontent.com/148752/45588544-3745e700-b8cb-11e8-8b83-c8e9026f31f9.png)

### Edition \(book\)

A mega component which puts together various other components. Appears on [book page](https://openlibrary.org/books/OL1447641M/Women_of_words)

![screen shot 2018-10-12 at 1 07 49 pm](https://user-images.githubusercontent.com/148752/46891776-ea98f180-ce1f-11e8-9f80-b2e973bdece8.png)

### ListEntry

![screen shot 2018-11-25 at 11 26 59 am](https://user-images.githubusercontent.com/148752/48983560-0a6d3780-f0a5-11e8-98cf-439aaecb1194.png)

Seen on [lists](https://openlibrary.org/lists) page.

### ListsPageCta

![screen shot 2018-11-25 at 10 43 12 am](https://user-images.githubusercontent.com/148752/48983066-1d7d0900-f09f-11e8-918c-70db70321f7e.png)

Seen on [lists](https://openlibrary.org/lists) page.

### ListsLists

On [works page](https://openlibrary.org/works/OL100238W/The_Titan) and books pages:

![screen shot 2018-09-18 at 11 01 54 am](https://user-images.githubusercontent.com/148752/45706914-52666000-bb32-11e8-8d7e-89aeaf43a5c5.png)

\[pictured: dropit tool\]

### ShareLinks

![screen shot 2018-09-19 at 5 01 42 pm](https://user-images.githubusercontent.com/148752/45788170-c8032680-bc2d-11e8-8821-09d5f614dc28.png)

Appears on [book page](https://openlibrary.org/books/OL23053293M/Ranma_1_2.). I don't believe it appears anywhere else \(??\)

### Dropper

The dropper is an enhanced substitution `select` element. At time of writing it does not work without JavaScript \(see \#1147\).

![screen shot 2018-09-19 at 5 10 04 pm](https://user-images.githubusercontent.com/148752/45788340-e3bafc80-bc2e-11e8-9ab7-cf1fdef06d19.png)

#### Dropper-list

![screen shot 2018-11-27 at 8 09 30 am](https://user-images.githubusercontent.com/148752/49094756-d49e8f00-f21b-11e8-9322-1473de1ce6fb.png)

A variant of the dropper exists for things that are not books. Can be seen on subjects and authors pages.

Dropper with dropdown

\(See also [ReadStatuses](design-pattern-library.md#Readstatuses) component, shown in image\)

![screen shot 2018-09-19 at 5 11 16 pm](https://user-images.githubusercontent.com/148752/45788362-0fd67d80-bc2f-11e8-8981-833512692122.png)

with activated-check

![screen shot 2018-09-19 at 5 11 57 pm](https://user-images.githubusercontent.com/148752/45788381-2f6da600-bc2f-11e8-9ff0-eef3ba1f96fa.png)

When referring to "Drop click" we are referring to this part of the element:

![screen shot 2018-09-19 at 5 16 48 pm](https://user-images.githubusercontent.com/148752/45788524-f1bd4d00-bc2f-11e8-9045-96320b08964d.png)

### Mode-options

Radio buttons to toggle between different 3 different options

Seen - [https://openlibrary.org/search](https://openlibrary.org/search) , [https://openlibrary.org/search?q=harry+potter](https://openlibrary.org/search?q=harry+potter)

![mode-options](https://user-images.githubusercontent.com/31198893/46106538-99211d80-c1f6-11e8-818a-0ffed5e76aa8.png)

### Books Page

### Read Panel

Appears on books page.

![screen shot 2018-10-11 at 5 20 57 pm](https://user-images.githubusercontent.com/148752/46840895-11045180-cd7a-11e8-9d29-4f2a0b857ced.png)

### Definition List

Definition markup can be used to generate a definition list See [https://www.w3.org/MarkUp/html3/deflists.html](https://www.w3.org/MarkUp/html3/deflists.html)

![screen shot 2018-10-11 at 5 23 36 pm](https://user-images.githubusercontent.com/148752/46841001-70626180-cd7a-11e8-886c-713ff38b6bfb.png)

### ReadStatuses

Shown when you click the [Dropper](design-pattern-library.md#dropper)component.

![screen shot 2018-10-11 at 5 33 55 pm](https://user-images.githubusercontent.com/148752/46841305-e0251c00-cd7b-11e8-85f7-1f71a7344585.png)

### WidgetBox

Widget boxes are used for layout. They consist of a header, a hint and a component \(which might be a list or another component on this page\)

The screenshot shows a WidgetBox with an unordered list and a WidgetBox containing a ListsLists. ![screen shot 2018-10-19 at 3 32 17 pm](https://user-images.githubusercontent.com/148752/47246697-3dd8ea00-d3b4-11e8-8c87-920b35fcf05e.png)

### NavEdition

Appears at the top of book pages. Will probably be merged in future with another component once identified.

![screen shot 2018-10-23 at 6 03 09 pm](https://user-images.githubusercontent.com/148752/47399581-fcb34380-d6ed-11e8-9c2b-7babc86fac17.png)

### PageAlert

![screen shot 2019-02-08 at 5 41 26 pm](https://user-images.githubusercontent.com/148752/52514686-de078880-2bc8-11e9-9287-0758ae459889.png)

Visit [http://openlibrary.org/books/add](http://openlibrary.org/books/add) incognito. Styled using \#noLogin Similar styles exist \(with different iconography\) for \#preMerge, \#postMerge, \#errorMerge

See also [FlashMessage](design-pattern-library.md#FlashMessage)

### WmdButtonBar

![screen shot 2018-11-09 at 3 40 53 pm](https://user-images.githubusercontent.com/148752/48293867-e4edf600-e435-11e8-8d56-11b3e0164722.png)

Seen on [http://localhost:8080/people/openlibrary7774?m=edit](http://localhost:8080/people/openlibrary7774?m=edit)

### WmdPromptDialog

![screen shot 2018-11-09 at 4 30 43 pm](https://user-images.githubusercontent.com/148752/48294976-d6570d00-e43c-11e8-9653-18575fd04aab.png)

When editing a profile page, and clicking the link button in the [wmd button bar](WmdButtonBar) this overlay shows. JavaScript is required.

### PageHeadingSearchBox

A form within the page that allows searching the site.

![screen shot 2018-11-16 at 3 58 36 pm](https://user-images.githubusercontent.com/148752/48653227-90f48b80-e9b8-11e8-983d-4ad68fc44404.png)

Seen on [http://openlibrary.org/subjects/romance](http://openlibrary.org/subjects/romance)

Must appear at the top of the page.

#### searchInsideForm

A PageHeadingSearchBox must contain a searchInsideForm

![screen shot 2019-02-08 at 5 56 07 pm](https://user-images.githubusercontent.com/148752/52514918-d47f2000-2bca-11e9-9273-35fe92d9901b.png)

Other examples:

* [http://openlibrary.org/lists](http://openlibrary.org/lists)

### UI-Dialog

Deprecated, but in use when removing lists on books pages. See also [colorbox](design-pattern-library.md#colorboxoverlay)

![screen shot 2019-01-11 at 1 41 48 pm](https://user-images.githubusercontent.com/148752/51061094-db544c00-15a6-11e9-8bed-12b2aabbd679.png)

### Throbber

Used when loading the carousel for changing covers for a book. Possibly reusable for other things?

![Screenshot 2019-12-06 at 3 24 50 PM](https://user-images.githubusercontent.com/148752/70363262-97aca580-183c-11ea-8a66-9c2015bda78a.png)

### SubNav

A list of navigation links \(tabs\) that appear under the title e.g. [Recent changes](https://openlibrary.org/recentchanges). When one is selected it should be bolded.

![Screen Shot 2020-03-28 at 10 50 05 AM](https://user-images.githubusercontent.com/148752/77829855-eaf37e00-70e1-11ea-94b0-f0773830b9b3.png) \(Note the title "Recent Changes" should not be considered part of the component.

![Screen Shot 2020-03-28 at 10 51 16 AM](https://user-images.githubusercontent.com/148752/77829876-0f4f5a80-70e2-11ea-8dc3-734c423f8a39.png)

