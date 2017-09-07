# Setup PostgreSQL for basic use

This instructions were tested in Debian 8 (Jessie) and Debian 9 (Stretch)

## Install Packages

```bash
sudo apt-get install postgresql
```

## Make the server accept connections from your favorite networks
To enable connections from a particular network
add a line to the file `/etc/postgresql/9.4/main/pg_hba.conf`
that reads similar to

```bash
host    all             all             192.168.0.1/24            md5
```

substituting `192.168.0.1/24` by the network you wish to give access.

## [Optional 1] Change postgres user password
Change the UNIX password of the postgres user, it has none by default, with (as root):

```bash
passwd postgress
```

## [Optional 2] Change default data directory
The place wher epostggres will store your data is somewhere like
`/var/lib/postgresql/9.6/main` It may be a good idea to change it, for example to
store the data in a separate devide or partition apart from your root
file system. Get the actual place in your system in `psql` as the postgres user
with:

```bash
su - postgres
psql
postgres=# SHOW data_directory;

Output
       data_directory       
------------------------------
/var/lib/postgresql/9.6/main
(1 row)
```

To change this location you have to shoutdown the DBMS, copy the contents of this directory
to your new data directory and then update the postgres configuration to point to the new
location.After that you can start your DBMS again.

```bash
systemctl stop postgres
rsync -av /var/lib/postgresql /wherever/you/want/the/data
your_text_editor /etc/postgresql/9.6/main/postgresql.conf
```

Note that I copy from the parent of the `9.6/main` directory, that is optional,
you can copy from `main` directly. Look for the line in `postgresql.conf` that reads
similar to:

```bash
data_directory = '/var/lib/postgresql/9.6/main' # use data in another directory
```

And of course change to your new data directory

```bash
data_directory = '/wherever/you/want/the/data/9.6/main' # use data in another directory
```

## [Optional 3] Create a new role (user), a databse, and permit the user to log in

```SQL
CREATE ROLE <UserName> WITH PASSWORD '<Password>';
ALTER ROLE <UserName> WITH LOGIN;
CREATE DATABASE <DatabaseName>;
GRANT ALL PRIVILEGES ON DATABASE <DatabaseName> TO <UserName>;
```
