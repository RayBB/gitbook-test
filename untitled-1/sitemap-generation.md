# Sitemap-Generation

See original documentation at: [http://gio.blog.archive.org/2015/02/02/ol-sitemap-generation/](http://gio.blog.archive.org/2015/02/02/ol-sitemap-generation/) [http://gio.blog.archive.org/2015/02/04/ol-how-to-generate-the-sitemaps/](http://gio.blog.archive.org/2015/02/04/ol-how-to-generate-the-sitemaps/)

## Data Dumps

Sitemaps are generated on `ol-home` using the latest data dump `ol_dump.txt.gz` as the source file. See [Generating Data Dumps](Generating-Data-Dumps) for more on generating `ol_dump`

The last dump is available at: [http://openlibrary.org/data/ol\_dump\_works\_latest.txt.gz](http://openlibrary.org/data/ol_dump_works_latest.txt.gz) for more details you can see [https://openlibrary.org/developers/dumps](https://openlibrary.org/developers/dumps).

## Generating Sitemaps

To generate the Sitemap, execute this code on ol-home:

```text
ssh ol-home
cd /1/var/tmp  # this way you can rsync it from rsync://ol-home/var_1/tmp/
python /1/var/lib/openlibrary/deploy/openlibrary/openlibrary/data/sitemap.py ol_dump_works_latest.txt.gz
# or is this the right script? /opt/openlibrary/openlibrary/scripts/2009/01/sitemaps/sitemap.py
```

## Making Sitemaps Live

After the sitemap is generated you need to place it in /1/var/lib/openlibrary/sitemaps as defined in /olsystem/etc/nginx/sites-available/openlibrary.conf on ol-www1.

```text
location ~ ^/static/(docs|tour|sitemaps|jsondumps|images/shelfview|sampledump.txt.gz)(/.*)?$ {
    root /1/var/lib/openlibrary/sitemaps;
    autoindex on;
    rewrite ^/static/(.*)$ /$1 break;
}
```

You can do this using rsync:

```text
sudo rsync -av rsync://ol-home/var_1/tmp/sitemaps/sitemaps /1/var/lib/openlibrary/sitemaps/
```

## Verify Sitemaps Available

```text
curl -I https://openlibrary.org/static/sitemaps/siteindex.xml.gz

HTTP/1.1 200 OK
Server: nginx/1.1.19
Date: Wed, 04 Feb 2015 16:48:50 GMT
Content-Type: text/plain
Content-Length: 14689
Last-Modified: Wed, 04 Feb 2015 12:48:06 GMT
Connection: keep-alive
Accept-Ranges: bytes
```

