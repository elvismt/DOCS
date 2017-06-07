# Setup PostgreSQL in Debian 9

## Install Packages

```bash
sudo apt-get install postgresql
```

## Make the server accept connections from your favorite networks
To enable connections from a particular network
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
