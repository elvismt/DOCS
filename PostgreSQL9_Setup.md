# Setup PostgreSQL in Debian 9

## Packages installation

```bash
sudo apt-get install postgresql
```

## Make the server accept connections from all addresses

Edit `/etc/postgresql/9.6/mail/postgresql.conf` and search for the line

```bash
listen_addresses = 'localhost'
```

This line may be commented out, if so, uncomment it and replace it by

```bash
listen_addresses = '*'
```

## Create a new role and an associated batabse

```SQL
CREATE USER <UserName> WITH PASSWORD <ThePassword>;
CREATE DATABASE <DatabaseName>;
GRANT ALL PRIVILEGES ON DATABASE <DatabaseName> TO <UserName>;
```
