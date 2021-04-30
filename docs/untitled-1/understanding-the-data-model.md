# Understanding-The-Data-Model

## Infogami

Each Infogami page \(i.e. something with a URL\) has an associated type. Each type contains a schema that states what fields can be used with it and what format those fields are in. Those are used to generate view and edit templates which can then be further customized as a particular type requires.

Infogami provides a generic way through it's wiki to create new types as needed.

## Database Schema

Open Library in essence only really has **only two database tables**. By default they will have the same pretty basic functionality through Infogami

### Thing table

The thing table defines [types](https://openlibrary.org/dev/docs/infogami#anchor5) like editions, works authors, users, languages. The thing table also keeps track of instances of things by their identifiers it basically registers their IDs in the table as an instance.

Entries in a sample thing table

| id | key | type | latest\_revision | created | last\_modified |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 2 | /type/key | 1 | 1 | 2013-03-20 10:27:01.322813 | 2013-03-20 10:27:01.322813 |
| 3 | /type/string | 1 | 1 | 2013-03-20 10:27:01.322813 | 2013-03-20 10:27:01.322813 |
| 4 | /type/text | 1 | 1 | 2013-03-20 10:27:01.322813 | 2013-03-20 10:27:01.322813 |
| 5 | /type/int | 1 | 1 | 2013-03-20 10:27:01.322813 | 2013-03-20 10:27:01.322813 |

### Data table

The data table on the other hand maps one of these types to all of the data associated with it Infogami provides a generic way through it's wiki to create new types as are needed

Entry in a sample data table

| thing\_id | revision | data |
| :--- | :--- | :--- |
| 1 | 1 | {"created": {"type": "/type/datetime", "value": "2013-03-20T10:27:01.223351"}, "last\_modified": {"type": "/type/datetime", " value": "2013-03-20T10:27:01.223351"}, "latest\_revision": 1, "key": "/type/type", "type": {"key": "/type/type"}, "id": 1, "revision": 1} |

Read further about Infogami and type on :

[https://openlibrary.org/dev/docs/infogami](https://openlibrary.org/dev/docs/infogami)

