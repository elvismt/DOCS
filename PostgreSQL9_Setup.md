# Setup PostgreSQL in Debian 9

## Packages installation

```bash
sudo apt-get install postgresql
```

## Make the server accept connections from remote hosts

Edit `/etc/postgresql/9.4/main/postgresql.conf` and search for the line

```bash
listen_addresses = 'localhost'
```

This line may be commented out, if so, uncomment it. The default value is `'localhost'`
this means that the postgres server will accept connections only from the local host.
It depends on your application, it will probably run in a separate machine, so add the
IPs of the machines that you want to enable access to the database, if you want to accept
connections from all addresses use:

```bash
listen_addresses = '*'
```

Additionally, if you to enable connections from a network different from that of the database
add a line to the file `/etc/postgresql/9.4/main/pg_hba.conf` that reads similar to

```bash
host    all             all             192.168.0.1/24            md5
```

substituting `192.168.0.1/24` by the network you wish to give access.

## [OPTIONAL] Create a new role and an associated batabse

```SQL
CREATE USER <UserName> WITH PASSWORD '<ThePassword>';
CREATE DATABASE <DatabaseName>;
GRANT ALL PRIVILEGES ON DATABASE <DatabaseName> TO <UserName>;
```
