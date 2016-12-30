# Apache setup made on Debian 8.6

1 - Install the packages

```bash
sudo apt-get install apache2 apache2-doc apache2-utils
```
Apache should be running now try accessing localhost:80. It will serve a default greeting html page.

*note:* Debian and it's derivatives do not create the httpd.conf file by defualt. Put configs in
/etc/apache2/apache2.conf instead.
