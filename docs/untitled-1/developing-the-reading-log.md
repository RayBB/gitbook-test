# Developing-the-Reading-Log

Working with Reading Log: A mini-tutorial.

## Overview

The html template for the reading log code is here: [https://github.com/internetarchive/openlibrary/blob/master/openlibrary/templates/account/books.html](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/templates/account/books.html)

This is what the rendered page looks like: [https://openlibrary.org/people/mekBot/books/want-to-read](https://openlibrary.org/people/mekBot/books/want-to-read)

The template is rendered by a view method here: [https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/upstream/account.py\#L733-L760](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/upstream/account.py#L733-L760)

## Data Flow

A user visits [https://openlibrary.org/people/mekBot/books/want-to-read](https://openlibrary.org/people/mekBot/books/want-to-read). This url pattern is matched by [https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/upstream/account.py\#L734](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/upstream/account.py#L734) within the code and thus is handled by the `public_my_books` function in [https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/upstream/account.py\#L733](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/upstream/account.py#L733).

The `public_my_books` in function in `plugins/upstream/account.py` calls the `Bookshelves` API model in [https://github.com/internetarchive/openlibrary/blob/master/openlibrary/core/bookshelves.py\#L6](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/core/bookshelves.py#L6) which asks/queries for Reading Log data from the database.

The `Bookshelves` model returns data back to `public_my_books` where it is used to render \(i.e. passed from [https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/upstream/account.py\#L755-L759](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/upstream/account.py#L755-L759) into\) the html template \([https://github.com/internetarchive/openlibrary/blob/master/openlibrary/templates/account/books.html](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/templates/account/books.html)\).

## An example

In [\#5080](https://github.com/internetarchive/openlibrary/issues/5080), you can read through a slightly unrealistic example of adding Search filtering capabilities to the Reading Log:

By unrealistic, we mean that this proposal currently will not work as implemented because book titles, authors, and the other data we'd like to search for are not kept in our ReadingLog db table, only the OL identifiers. We may be able to achieve this with solr in the future. But assuming we did have the desired info in our database \(and as a thought exercise\):

1. First, we'd need to update the Reading Log html template \([https://github.com/internetarchive/openlibrary/blob/master/openlibrary/templates/account/books.html](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/templates/account/books.html)\) to include a search box \(design task\). For a first version, we'd probably use an html form which submits a GET search query , similar to what we have on the author's page: [https://openlibrary.org/authors/OL7283091A](https://openlibrary.org/authors/OL7283091A)

   ![openlibrary org\_authors\_OL7229114A\_Robert\_Alan\_Hill \(1\)](https://user-images.githubusercontent.com/978325/115574144-38ef9c80-a276-11eb-9ff9-5c53f78e45cb.png). In the future, we might want to use javascript \(similar to how we the real-time Search box works at the top of the website\):

   ![openlibrary org\_authors\_OL7229114A\_Robert\_Alan\_Hill \(2\)](https://user-images.githubusercontent.com/978325/115574443-7a804780-a276-11eb-875f-5325f5b61ebc.png)

2. Next, we'd need to update the `public_my_books` controller method in [https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/upstream/account.py\#L733-L760](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/upstream/account.py#L733-L760) to accept a GET parameter. Already, the function expects a `page` variable to be sent as a GET parameters \([https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/upstream/account.py\#L738](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/upstream/account.py#L738)\) so accomplishing this should be as straightforward as adding another parameters like, `i = web.input(page=1, search=None)`.
3. When/where we fetch the patron's books here: [https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/upstream/account.py\#L754](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/upstream/account.py#L754), we need to alter the logic to check whether a `i.search` query is present \(e.g. `if i.search`\). If the `i.search` value is present, we'll need change the line `readlog.get_works` call so this optional `search` parameter is passed along with our request for matching books.
4. `readlog` is an instance of `plugins.upstream.account.ReadingLog` \(class defined here: [https://github.com/internetarchive/openlibrary/blob/1f57759886b65430d805270830677120c1dc067d/openlibrary/plugins/upstream/account.py\#L645](https://github.com/internetarchive/openlibrary/blob/1f57759886b65430d805270830677120c1dc067d/openlibrary/plugins/upstream/account.py#L645)\). Its `get_works` method \([https://github.com/internetarchive/openlibrary/blob/1f57759886b65430d805270830677120c1dc067d/openlibrary/plugins/upstream/account.py\#L716](https://github.com/internetarchive/openlibrary/blob/1f57759886b65430d805270830677120c1dc067d/openlibrary/plugins/upstream/account.py#L716)\) will need to be updated to accept an optional `search` parameter \(e.g. `(key, page=1, limit=RESULTS_PER_PAGE, search=None)`\). This `ReadingLog.get_works` function essentially uses a `KEYS` dictionary \(defined here: [https://github.com/internetarchive/openlibrary/blob/1f57759886b65430d805270830677120c1dc067d/openlibrary/plugins/upstream/account.py\#L654-L660](https://github.com/internetarchive/openlibrary/blob/1f57759886b65430d805270830677120c1dc067d/openlibrary/plugins/upstream/account.py#L654-L660)\) to lookup and then invoke the proper book-fetching function.
5. Each of the corresponding `ReadingLog` methods referenced by the `KEYS` dictionary \(namely: `get_waitlisted_editions`, `get_loans`, `get_want_to_read`, `get_currently_reading`, `get_already_read`\) must thus also be updated to take an optional `search` parameter. Each of these functions ultimately makes an API call to the same function within our `Bookshelves` API model: `Bookshelves.get_users_logged_books` \([https://github.com/internetarchive/openlibrary/blob/master/openlibrary/core/bookshelves.py\#L118-L149](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/core/bookshelves.py#L118-L149)\)
6. After a search box form has been added to the `template`, the `public_my_books` view/controller has been edited to expect a `search` parameter, this `search` parameter is forwarded to our `readlog.get_works` call, and the `readlog` object \(i.e. the `ReadingLog` class\) have all been updated to accept an optional `search` parameter, we'll then need to do the hard work of modifying the actual API `Bookshelves.get_users_logged_books` \(the thing which calls the database\) to consider the possibility of an optional search parameter when requesting data from the database: [https://github.com/internetarchive/openlibrary/blob/master/openlibrary/core/bookshelves.py\#L118-L149](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/core/bookshelves.py#L118-L149)\).

## More practical applications

[The same example above](https://github.com/internetarchive/openlibrary/issues/5080#issue-864008133) \(which pretends to add Search filtering to the Reading Log\) can be adapted to add an option to sort one's Reading Log entries by date added, such as is requested in [Issue \#4267](https://github.com/internetarchive/openlibrary/issues/4267).

