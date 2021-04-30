# Endpoints

An overview \(audit\) of the Open Library internal "unofficially supported" APIs \(which should be replaced with public APIs\)

* [Lists](endpoints.md#lists)
  * [Creating Lists](endpoints.md#creating-lists)
  * [Adding to Lists](endpoints.md#adding-to-lists)
  * [Searching for Lists](endpoints.md#searching-for-lists)
* [Works](endpoints.md#works)
  * [Creating Works](endpoints.md#creating-works)
  * [Deleting Works](endpoints.md#deleting-works)
* [Editions](endpoints.md#editions)
  * [Viewing Editions](endpoints.md#viewing-editions)
  * [Editing Editions](endpoints.md#editing-editions)
  * [Deleting Editions](endpoints.md#deleting-editions)
* [Importing](endpoints.md#importing)
* [Subjects](endpoints.md#subjects)
* [List of All Routes](endpoints.md#list-of-all-routes)

## Lists

### Creating Lists

### Adding to Lists

### Searching for Lists

In openlibrary/plugins/openlibrary/lists.py

```text
GET /lists/search?q=
```

## Works

### Creating Works

```text
POST https://openlibrary.org/api/new.json
```

**Body \(minimum\):**

```text
{
    "type": {"key": "/type/work"},
    "title": "<Work Title>",
    "authors": [{"type": "/type/author_role", "author": {"key": "/authors/<olid>"}}]
}
```

Returns the created work's OLID.

### Deleting Works

In file openlibrary/plugins/upstream/addbook.py:

```text
GET https://openlibrary.org/books/(OL...M)/edit
```

**Parameters:**

```text
name="_delete"
```

**Permissions:**

User must be Administrator

**Notes:**

Deleting the last Edition of a Work via this endpoint will remove the Work also.

[**RESTful API**](https://openlibrary.org/dev/docs/restful_api) **Delete:**

```text
PUT https://openlibrary.org/works/(OL...W).json
```

**Body:**

```text
{ 
  "type": { "key": "/type/delete" },
  "_comment": "<Reason for deletion>"
}
```

**Permissions:**

User must be Administrator

**Notes:**

Deleting a Work that still has editions will succeed, leaving orphaned Editions, potentially without Authors if the Author was only present on the Work. Care must be taken when deleting Works that this does not occur. Deleting the last Work of an Author will not remove the Author record.

## Editions

### Viewing Editions

RESTful API

```text
GET https://openlibrary.org/books/(OL...M).json

GET https://openlibrary.org/books/(OL...M).rdf
```

### Creating Editions

### Editing Editions

[**RESTful API**](https://openlibrary.org/dev/docs/restful_api) **Edit:**

```text
PUT https://openlibrary.org/books/(OL...M).json
```

**Body:**

```text
{
  < complete **JSON** body of current record from GET request, modified as desired >
  "_comment": "<Description of changes>"
}
```

### Merging Editions

### Deleting Editions

In file openlibrary/plugins/upstream/addbook.py:

[https://openlibrary.org/books/\(OL...M\)/edit](https://openlibrary.org/books/%28OL...M%29/edit)

**Parameters:**

name="\_delete"

**Permissions:**

User must be Administrator

[**RESTful API**](https://openlibrary.org/dev/docs/restful_api) **Delete:**

```text
PUT https://openlibrary.org/books/(OL...M).json
```

**Body:**

```text
{ 
  "type": { "key": "/type/delete" },
  "_comment": "<Reason for deletion>"
}
```

**Permissions:**

User must be Administrator

**Notes:**

Deleting the last Edition of a Work will **NOT** remove the Work. It has to be cleared separately.

## Importing

### Data Import

```text
POST /api/import
```

The raw POST data is parsed by [`parse_body()`](https://github.com/internetarchive/openlibrary/blob/db19d10e7dfc03307e48a7afdd1ae734dd514045/openlibrary/plugins/importapi/code.py#L49) which intends to accept:

* XML \(rdf \| opds \| MARCMXL\)
* JSON edition format \(needs schema\)
* MARC binary

### Import by archive.org reference

```text
POST /api/import/ia
```

**POST body:**

```text
{
    "identifier": "<ocaid>",
    "require_marc": "false",
    "bulk_marc": "false",
    "local_id": "<optional local_id config key>"
}
```

Example data to import a single archive.org item:

```text
{ "identifier": "europe1890194500wink" }
```

Example data to import one record from a bulk MARC, identifier format `ocaid/filename:offset:length`:

```text
{ "bulk_marc": "true", "identifier": "talis-openlibrary-contribution/talis-openlibrary-contribution.mrc:1353778212:578" }
```

If importing from a partner's MARC records where we want to store their `local_id` \(in practice this is expected to be a scannable barcode\) the optional `local_id` parameter can be supplied which will read the configuration from an entry under [https://openlibrary.org/local\_ids](https://openlibrary.org/local_ids) to extract an id from any controlfield \(e.g. `100$`\) or any other MARC subfield \(e.g. `919$a`\) as specified in the `"id_location"` of that entry. Example: `"local_id": "trent"`.

## Subjects

### Searching for Subjects

See: openlibrary/plugins/worksearch/subjects.py

[https://openlibrary.org/search/subjects](https://openlibrary.org/search/subjects)

## Recent Changes \(edits\)

[https://openlibrary.org/dev/docs/api/recentchanges](https://openlibrary.org/dev/docs/api/recentchanges)

### Recent Changes \(edits\) by date

### Recent Changes by edit type

edit types: create, update, register \(old form of user create by AccountBot\), bulk\_update \(starting 2009-03\), add-book \(starting 2010-08\), add-cover \(starting 2010-08\), edit-book \(starting 2010-08\), lists \(starting 2010-11\), new-account \(replaced register\),

### Recent Changes by both

[https://openlibrary.org/recentchanges/2009/12/31/bulk\_update.json?bot=true&offset=0&limit=100](https://openlibrary.org/recentchanges/2009/12/31/bulk_update.json?bot=true&offset=0&limit=100)

offset: default 0, max 10000 limit: default 100, max 1000

## List of all Routes

An Explorer's Map to the Chasm Which is Open Library -- A mapping of url routes \(endpoints\) and the files in which they appear. Informed via: grep -ri "path \= \"\[/\]" \*

```text
`## Infogami`
   `Infogami default routes:`
   `infogami/core/code.py:    path = "/account/login"`
   `infogami/core/code.py:    path = "/account/register"`
   `infogami/core/code.py:    path = "/account/logout"`
   `infogami/core/code.py:    path = "/account/forgot_password"`
   `infogami/core/code.py:    path = "/account/reset_password"`
   `infogami/core/code.py:    path = "/account/preferences"`
   `infogami/core/code.py:    path = "/account/preferences/change_password"`
   `infogami/core/code.py:    path = "/favicon.ico"`
   `infogami/plugins/api/code.py:    path = "/api/(.*)"`
   `infogami/plugins/api/code.py:    path = "/account/login"`
   `infogami/plugins/wikitemplates/code.py:    path = "/account/preferences/template_preferences"`
   `vendor/infogami/infogami/core/code.py:    path = "/account/login"`
   `vendor/infogami/infogami/core/code.py:    path = "/account/register"`
   `vendor/infogami/infogami/core/code.py:    path = "/account/logout"`
   `vendor/infogami/infogami/core/code.py:    path = "/account/forgot_password"`
   `vendor/infogami/infogami/core/code.py:    path = "/account/reset_password"`
   `vendor/infogami/infogami/core/code.py:    path = "/account/preferences"`
   `vendor/infogami/infogami/core/code.py:    path = "/account/preferences/change_password"`
   `vendor/infogami/infogami/core/code.py:    path = "/favicon.ico"`
   `vendor/infogami/infogami/infobase/client.py:        path = "/" + sitename + path`
   `vendor/infogami/infogami/plugins/api/code.py:    path = "/api/(.*)"`
   `vendor/infogami/infogami/plugins/api/code.py:    path = "/account/login"`
   `vendor/infogami/infogami/plugins/wikitemplates/code.py:    path = "/account/preferences/template_preferences"`

`## Open Library`
`Home Page:`
    `openlibrary/plugins/openlibrary/home.py:    path = "/"`

`Admin & Devops Endpoints:`
   `openlibrary/admin/code.py:    path = "/admin/?"`
   `openlibrary/plugins/admin/code.py:    path = "/admin(?:/.*)?"`
   `openlibrary/plugins/admin/mem.py:    path = "/memory"`
   `openlibrary/plugins/admin/mem.py:    path = "/memory/type/(.*)"`
   `openlibrary/plugins/admin/mem.py:    path = "/memory/id/(.*)"`
   `openlibrary/plugins/openlibrary/dev_instance.py:    path = "/_dev/process_ebooks"`

`Entities:`
   `Books:`
   `openlibrary/core/models.py:    types.register_type('^/books/[^/]*$', '/type/edition')`
   `openlibrary/plugins/books/code.py:add_hook("books", books)`
   `openlibrary/plugins/upstream/addbook.py:    path = "(/books/.*)/daisy"`
   `openlibrary/plugins/upstream/addbook.py:    path = "(/books/OL\d+M)/edit"`
   `openlibrary/plugins/upstream/addbook.py:    path = "/books/add"`
   `openlibrary/plugins/openlibrary/code.py:    path = "/addbook"`
   `openlibrary/plugins/openlibrary/code.py:    path = r"/(isbn|oclc|lccn|ia|ISBN|OCLC|LCCN|IA)/([^/]*)(/.*)?"  [bookpage]`

   `Works:`
   `openlibrary/core/models.py:    types.register_type('^/works/[^/]*$', '/type/work')`
   `openlibrary/plugins/upstream/addbook.py:    path = "(/works/OL\d+W)/edit"`

   `Authors:`
   `openlibrary/core/models.py:    types.register_type('^/authors/[^/]*$', '/type/author')`
   `openlibrary/plugins/openlibrary/authors.py:    path = "/authors"`
   `openlibrary/plugins/upstream/merge_authors.py:    path = "/authors/merge"`
   `openlibrary/plugins/worksearch/code.py:    path = "/authors/(OL\d+A)/merge-works"`
   `openlibrary/plugins/upstream/addbook.py:    path = "(/authors/OL\d+A)/edit"`
   `openlibrary/plugins/upstream/addbook.py:    path = "/authors/_autocomplete"`

   `Lists:`
   `openlibrary/plugins/openlibrary/lists.py:    path = "/lists"`
   `plugins/openlibrary/lists.py:    path = "(/(?:people|books|works|authors|subjects)/[^/]+)/lists"`
   `plugins/openlibrary/lists.py:    path = "(/people/\w+/lists/OL\d+L)/delete"`
   `plugins/openlibrary/lists.py:    path = "(/(?:people|books|works|authors|subjects)/[^/]+)/lists"`
   `plugins/openlibrary/lists.py:    path = "(/people/[^/]+/lists/OL\d+L)"`
   `plugins/openlibrary/lists.py:    path = "(/people/\w+/lists/OL\d+L)/seeds"`
   `plugins/openlibrary/lists.py:    path = "(/people/\w+/lists/OL\d+L)/editions"`
   `plugins/openlibrary/lists.py:    path = "(/people/\w+/lists/OL\d+L)/editions"`
   `plugins/openlibrary/lists.py:    path = "(/people/\w+/lists/OL\d+L)/subjects"`
   `plugins/openlibrary/lists.py:    path = "(/people/\w+/lists/OL\d+L)/embed"`
   `plugins/openlibrary/lists.py:    path = "(/people/\w+/lists/OL\d+L)/export"`
   `plugins/openlibrary/lists.py:    path = "(/people/[^/]+/lists/OL\d+L)/feeds/(updates).(atom)"`

   `Languages:`
   `openlibrary/core/models.py:    types.register_type('^/languages/[^/]*$', '/type/language')`
   `openlibrary/plugins/upstream/addbook.py:    path = "/languages/_autocomplete"`

   `Cover Images:`
   `openlibrary/plugins/upstream/code.py:    path = "(/books/OL\d+M)/cover"`
   `openlibrary/plugins/upstream/code.py:    path = "(/authors/OL\d+A)/photo"`
   `openlibrary/plugins/upstream/covers.py:    path = "(/books/OL\d+M)/add-cover"`
   `openlibrary/plugins/upstream/covers.py:    path = "(/works/OL\d+W)/add-cover"`
   `openlibrary/plugins/upstream/covers.py:    path = "(/authors/OL\d+A)/add-photo"`
   `openlibrary/plugins/upstream/covers.py:    path = "(/books/OL\d+M)/manage-covers"`
   `openlibrary/plugins/upstream/covers.py:    path = "(/works/OL\d+W)/manage-covers"`
   `openlibrary/plugins/upstream/covers.py:    path = "(/authors/OL\d+A)/manage-photos"`
   `openlibrary/plugins/upstream/code.py:    path = "/images/.*"`

`Special Book Collections:`
   `openlibrary/plugins/openlibrary/borrow_home.py:    path = "/read"`
   `openlibrary/plugins/openlibrary/borrow_home.py:    path = "/borrow"`

`MARC Records:`
   `openlibrary/views/showmarc.py:    path = "/show-marc/(.*)"`
   `openlibrary/views/showmarc.py:    path = "/show-records/ia:(.*)"`
   `openlibrary/views/showmarc.py:    path = "/show-records/amazon:(.*)"`
   `openlibrary/views/showmarc.py:    path = "/show-records/(.*):(\d+):(\d+)"`

`Statistics:`
   `openlibrary/views/loanstats.py:    path = "/stats"`
   `openlibrary/views/loanstats.py:    path = "/stats/lending(?:/(libraries|regions|countries|collections|subjects|format)/(.+))?"`
   `openlibrary/views/loanstats.py:    path = "/stats/waitinglists"`

`Themes:`
   `openlibrary/plugins/theme/code.py:    path = "/theme"`
   `openlibrary/plugins/theme/code.py:    path = "/theme/files"`
   `openlibrary/plugins/theme/code.py:    path = "/theme/files/(.+)"`
   `openlibrary/plugins/theme/code.py:    path = "/theme/modifications"`
   `openlibrary/plugins/theme/code.py:    path = "/theme/manage"`
   `openlibrary/plugins/theme/code.py:    path = "/theme/git-merge"`

`Recent Changes:`
   `openlibrary/plugins/upstream/recentchanges.py:    path = "/recentchanges"`
   `openlibrary/plugins/upstream/recentchanges.py:    path = "/recentchanges(/[^/0-9][^/]*)"`
   `openlibrary/plugins/upstream/recentchanges.py:    path = "/recentchanges/(\d\d\d\d(?:/\d\d)?(?:/\d\d)?)(/[^/]*)?"`
   `openlibrary/plugins/upstream/recentchanges.py:    path = "/recentchanges/goto/(\d+)"`
   `openlibrary/plugins/upstream/recentchanges.py:    path = "/recentchanges/\d\d\d\d/\d\d/\d\d/[^/]*/(\d+)"`

`Lending, Borrowing, & Availability:`
   `openlibrary/plugins/upstream/borrow.py:    path = "/borrow/notify"`
   `openlibrary/plugins/upstream/borrow.py:    path = "(/books/.*)/borrow"`
   `openlibrary/plugins/upstream/borrow.py:    path = "(/books/.*)/_borrow_status"`
   `openlibrary/plugins/upstream/borrow.py:    path = "(/books/.*)/borrow_admin"`
   `openlibrary/plugins/upstream/borrow.py:    path = "(/books/.*)/borrow_admin_no_update"`
   `openlibrary/plugins/upstream/borrow.py:    path = r"/ia_loan_status/(.*)"`
   `openlibrary/plugins/upstream/borrow.py:    path = r"/ia_auth/(.*)"`
   `openlibrary/plugins/upstream/borrow.py:    path = r"/borrow/receive_notification"`
   `openlibrary/plugins/openlibrary/api.py:    path = "/availability"`

`User Accounts & Auth:`
   `openlibrary/plugins/upstream/account.py:    path = "(/account/.*)"`
   `openlibrary/plugins/upstream/account.py:    path = "/account/create"`
   `openlibrary/plugins/upstream/account.py:    path = "/account/login"`
   `openlibrary/plugins/upstream/account.py:    path = "/account/verify/([0-9a-f]*)"`
   `openlibrary/plugins/upstream/account.py:    path = "/account/verify"`
   `openlibrary/plugins/upstream/account.py:    path = "/account/email"`
   `openlibrary/plugins/upstream/account.py:    path = "/account/email/verify/([0-9a-f]*)"`
   `openlibrary/plugins/upstream/account.py:    path = "/account/email/verify"`
   `openlibrary/plugins/upstream/account.py:    path = "/account/password"`
   `openlibrary/plugins/upstream/account.py:    path = "/account/password/forgot"`
   `openlibrary/plugins/upstream/account.py:    path = "/account/password/reset/([0-9a-f]*)"`
   `openlibrary/plugins/upstream/account.py:    path = "/account/notifications"`
   `openlibrary/plugins/upstream/account.py:    path = "/account/loans"`

`Data Dumps:`
   `openlibrary/plugins/upstream/data.py:    path = "/data/ol_dump(|_authors|_editions|_works|_deworks)_latest.txt.gz"`
   `openlibrary/plugins/upstream/data.py:    path = "/data/ol_cdump_latest.txt.gz"`
   `openlibrary/plugins/upstream/data.py:    path = "/data/ol_dump(|_authors|_editions|_works)_(\d\d\d\d-\d\d-\d\d).txt.gz"`
   `openlibrary/plugins/upstream/data.py:    path = "/data/ol_cdump_(\d\d\d\d-\d\d-\d\d).txt.gz"`

`Search:`
   `openlibrary/plugins/search/code.py:    path = "/search"`
   `openlibrary/plugins/worksearch/languages.py:    path = "/languages"`
   `openlibrary/plugins/worksearch/code.py:    path = "/search/edition"`
   `openlibrary/plugins/worksearch/code.py:    path = "/search"`
   `openlibrary/plugins/worksearch/subjects.py:    path = "/subjects"`
   `openlibrary/plugins/worksearch/publishers.py:    path = "/publishers"`

 `Importing:`
   `openlibrary/plugins/importapi/code.py:add_hook("import", importapi)`
   `openlibrary/plugins/importapi/code.py:add_hook("ils_search", ils_search)`
   `openlibrary/plugins/importapi/code.py:add_hook("ils_cover_upload", ils_cover_upload)`
   `openlibrary/plugins/importapi/code.py:add_hook("import/ia", ia_importapi)`

`Infobase Connection:`
   `vendor/infogami/infogami/plugins/api/code.py:add_hook("get", infobase_request)`
   `vendor/infogami/infogami/plugins/api/code.py:add_hook("things", infobase_request)`
   `vendor/infogami/infogami/plugins/api/code.py:add_hook("versions", infobase_request)`
   `vendor/infogami/infogami/plugins/api/code.py:add_hook("get_many", infobase_request)`
   `vendor/infogami/infogami/plugins/api/code.py:add_hook("write", infobase_request)`
   `vendor/infogami/infogami/plugins/api/code.py:add_hook("save_many", infobase_request)`

`Suggest API?:`
   `openlibrary/plugins/openlibrary/code.py:    path = "/suggest/search"`
   `openlibrary/plugins/openlibrary/code.py:    path = "/suggest/blurb/(.*)"`
   `openlibrary/plugins/openlibrary/code.py:    path = "/suggest/thumbnail"`

`Site / Domain Administration:`
   `openlibrary/plugins/openlibrary/code.py:    path = "/robots.txt"`
   `openlibrary/plugins/openlibrary/code.py:    path = "/health"`
   `openlibrary/plugins/openlibrary/code.py:    path = "/system/invalidate"`
   `openlibrary/plugins/openlibrary/code.py:    path = "/debug/memory"`
   `openlibrary/plugins/openlibrary/code.py:    path = "/debug/backdoor"`
   `admin/code.py:    path = "(/(?:images|js|css)/.*)"`
   `types.register_type('^/usergroup/[^/]*$', '/type/usergroup')`
   `types.register_type('^/permission/[^/]*$', '/type/permission')`
   `types.register_type('^/(css|js)/[^/]*$', '/type/rawtext')`

`Informational:`
   `openlibrary/plugins/openlibrary/borrow_home.py:    path = "/borrow/about"`

`Library Partners:`
   `openlibrary/core/models.py:    types.register_type('^/libraries/[^/]*$', '/type/library')`
   `openlibrary/plugins/openlibrary/libraries.py:    path = "/libraries/dashboard"`
   `openlibrary/plugins/openlibrary/libraries.py:    path = "/(libraries/pending-\d+)"`
   `openlibrary/plugins/openlibrary/libraries.py:    path = "/libraries/register"`
   `openlibrary/plugins/openlibrary/libraries.py:    path = "/libraries/locations.txt"`
   `openlibrary/plugins/openlibrary/libraries.py:    path = "/libraries/stats"`
   `openlibrary/plugins/openlibrary/libraries.py:    path = "/libraries/stats/(.*).csv"`
   `openlibrary/plugins/openlibrary/libraries.py:    path = "(/libraries/[^/]+)/notes"`

`APIs & Services:`
   `openlibrary/plugins/openlibrary/api.py:    path = "(/works/OL\d+W)/editions"`
   `openlibrary/plugins/openlibrary/api.py:    path = "(/authors/OL\d+A)/works"`
   `openlibrary/plugins/openlibrary/code.py:    path = "/details/([a-zA-Z0-9_-]*)(?:/leaf(\d+))?"`
   `openlibrary/plugins/openlibrary/dev_instance.py:    path = "/is_loaned_out/.*"`
   `openlibrary/plugins/books/code.py:    path = r"/api/volumes/(brief|full)/(oclc|lccn|issn|isbn|htid|olid|recordnumber)/(.+)"`
   `openlibrary/plugins/books/code.py:    path = r"/api/volumes/(brief|full)/json/(.+)"`
```

