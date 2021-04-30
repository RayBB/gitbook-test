# Creating-Community-Lists

The purpose of this guide is helping librarians and individuals take a list of Open Library keys \(e.g. `/works/OL123W, /works/OL234W`\) and populate a themed list in bulk

## The Open Library Official Book Lists Account

Open Library has an account specifically for featuring organization-vetted lists: [https://openlibrary.org/people/booklists](https://openlibrary.org/people/booklists)

An administrator may login to this account by going to: [https://openlibrary.org/admin/people/booklists](https://openlibrary.org/admin/people/booklists)

## Notes

* You must be logged in to Open Library in order to create or add to a list.
* These same instructions should work for your own personal account if you have a list of Open Library identifiers \(olids\) and want to test creating a list in bulk.

## Instructions

1. Once logged in, a list must be created manually for at least 1 seed and given a "title" and a "description".
2. At this point, the list should exist and appear under the booklists account's lists page: e.g. [https://openlibrary.org/people/booklists/lists/OL194558L/International\_Women%27s\_Day\_Celebrate\_Women\_Authors](https://openlibrary.org/people/booklists/lists/OL194558L/International_Women%27s_Day_Celebrate_Women_Authors)
3. Next, open up your browser's developer javascript console by pressing "control + shift + i" \(linux/windows\) or right-click on page and select the "Inspect" option in the right-click menu. Navigate to the `console` tab which will let you submit code.
4. If you know the Open Library IDs \(olids\) of works and editions in advance, you may use the following javascript incantation to add them in bulk to the list you created above.
5. Remove the human readable slug portion of the URL from step \(3\) -- in this example "/International\_Women%27s\_Day\_Celebrate\_Women\_Authors" -- and add /seeds.json: e.g. [https://openlibrary.org/people/booklists/lists/OL194558L/seeds.json](https://openlibrary.org/people/booklists/lists/OL194558L/seeds.json)

This url will be added into line 1 of the code below:

```text
await fetch("https://openlibrary.org/people/booklists/lists/OL194558L/seeds.json", {
    "credentials": "include",
    "body": JSON.stringify({add: [
       // IDs here
     ]}),
    "method": "POST",
    "mode": "cors"
});
```

Replace the `// IDs here` with a list of IDs of the form `{key: '/works/OL20008022W'}, ...`

1. A complete example should look like:

```text
await fetch("https://openlibrary.org/people/booklists/lists/OL194558L/seeds.json", {
    "credentials": "include",
    "body": JSON.stringify({add: [
       {key: '/works/OL20008022W'}, {key: '/works/OL5731542W'}, {key: '/works/OL19966826W'},
       {key: '/works/OL8476068W'}, {key: '/works/OL2335989W'}, {key: '/works/OL21097322W'},
     ]}),
    "method": "POST",
    "mode": "cors"
});
```

There is a limit of how many keys you can include in each request submitting the above code, which I \(@mekarpeles\) believe is either 100 or 50. You can resubmit the code as many times as needed with different batches of keys.

