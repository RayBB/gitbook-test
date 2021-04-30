# Anti-Spam-Tools

The following was originally published by Giovanni Damiola @gdamdam via [http://gio.blog.archive.org/2016/03/10/ol-anti-spam-tools](http://gio.blog.archive.org/2016/03/10/ol-anti-spam-tools). Gio writes:

We are addressing the spam problem with some volunteers help. In particular Charles is working hard on it and having great results. I need to contact him to have his updates.

Here some notes from Anand, December 2014: I spent most of today fighting spam. Blocked hundreds of accounts and built some simple tools to help block spam.

Tools:

Added “block and revert all edits” button to admin profile page Added /admin/spamwords page to mark some words as spam [https://openlibrary.org/admin/spamwords](https://openlibrary.org/admin/spamwords) Also a way to blacklist a domain from /admin/spamwords. If the edit to a page contains any of the spam words or email of the user is from the blacklisted domains, the edit won’t be accepted. New registrations with emails from those domains are also not accepted.

Measures taken: I’ve added the common words found in the recent spam to the spam words blacklisted mail.com as almost all of the spam was coming from that domain. This may stop some genuine people from registering and making edits. blocked and reverted edits lot of accounts

Other approaches:

On `ol-db1` investigate volume and patterns:

* `select * from store where key like 'account/%/verify' order by id desc limit 50;`

Check nginx access logs for common vectoros on `ol-www1`

* `sudo cat /var/log/nginx/access.log | grep "/people"`
* \`sudo cat /var/log/nginx/access.log \| grep "/account/create"

Sam's magic sauce:

```text
netstat -n | /home/samuel/work/reveal-abuse/mktable
sudo cat /var/log/nginx/access.log | cut -d ' ' -f 1 | sort | uniq -c  | sort -n | tail -n 10 | /home/samuel/work/reveal-abuse/reveal | /home/samuel/work/reveal-abuse/shownames
```

