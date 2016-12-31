# Apache with mod_wsgi setup on Debian 8.6

### Install the packages

```bash
sudo apt-get install apache2 apache2-doc apache2-utils libapache2-mod-wsgi-py3
```
Apache should be running now try accessing localhost:80. It will serve a default greeting html page.

### Configure the server

*note:* Debian and it's derivatives do not create the `httpd.conf` file by defualt. Put configs in
`/etc/apache2/apache2.conf` instead.

To enable mod_wsgi in the apache server all the following line in `/etc/apache2/apache2.conf`:

```bash
LoadModule wsgi_module /usr/lib/apache2/modules/mod_wsgi.so
```

Take care to check is this is really the location of `mod_wsgi.so`.

To add static files location add the following

```bash
Alias /static/  /path/to/static-files/

<Directory  /path/to/static-files/>
    Require all granted
</Directory>
```

And finally to add the WSGI script add

```bash
WSGIScriptAlias  /app-domain-name  /path/to/project-wsgi.py
WSGIPythonPath  /path/to/project

<Directory  /path/to/project>
<Files wsgi.py>
    Require all granted
</Files>
</Directory>
```

Restart apache with the command bellow and try to access the `app-domain-name` given above
from a web browser.

```bash
service apache2 restart
```

If you get a server error look at the end of the log file in `/var/log/apache2/error.log`
A possible source of error is that the apache script does not find some of your project's modules
in this case a possible strategy is to add the following line to your project's wsgi file

```python
import sys
sys.path.append(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))
```

Or adapt the path if your `wsgi.py` is not in the default location
