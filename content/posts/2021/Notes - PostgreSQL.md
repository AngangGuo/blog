---
title: "Notes - PostgreSQL"
date: 2021-07-16T10:52:39-07:00
draft: true
---

## PostgreSQL Notes

## Installation
* Download the installer from [PostgreSQL Download Page](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads)
* Install the server, pgAdmin and command line tools, but not Stack Builder(Drivers and add-ons for other language)
* You can install multiple database server in the same computer as long as their port is different

### For Windows
* Data Directory: `C:\Program Files\PostgreSQL\13\data`
* Superuser: `postgres` / my-password
* Locale: `[Default locale]`

### For Linux
```
// check version
lsb_release -a
sudo apt update
// version 12 in Ubuntu 20
sudo apt install postgresql

sudo -u postgres createuser --interactive

sudo -i -u postgres
createdb dbname

psql
\? // list all psql commands
\h // list all the SQL commands
\conninfo
\l
\d
ALTER USER postgres PASSWORD 'root';
\q // quit
```
### Verify
From the Start > PostgrSQL 13, you can open pgAdmin or SQL Shell to test the installation.

* For pgAmdin: Select the `postgres` database > Tools > Query Tool 
* From SQL Shell, press Enter to accept the default settings 
```
SELECT version();
                          version
------------------------------------------------------------
 PostgreSQL 13.3, compiled by Visual C++ build 1914, 64-bit
(1 row)
```



## Restore Database Data
```
// from SQL Shell
CREATE DATABASE dvdrental

// from command line: 
cd C:\Program Files\PostgreSQL\13\bin\pg_restore
pg_restore -U postgres -d dvdrental C:\temp\dvdrental.tar
```
You can also restore the database by right-clicking the database in pgAdmin, select `Restore...`.
You may need to set the `PostgreSQL Binary Path` as `$DIR/../../bin`(C:\Program Files\PostgreSQL\13\bin)
in File > Preferences > Paths

### How to move data directory?
See [here](https://stackoverflow.com/questions/67518061/how-to-move-location-of-postrgresql-13-database)

```
// check current data location
SHOW data_directory;
           data_directory
-------------------------------------
 C:/Program Files/PostgreSQL/13/data
(1 row)
```
Shutdown PostgreSQL service
```
sudo systemctl status postgres-12
sudo systemctl stop postgres-12
```
Move data directory
```
chown postgres:postgres /postgres/data
chmod -R 750 /postgres/data


```