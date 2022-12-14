---
hide:
    - footer
tags:
    - MySQL
    - Database
    - Linux
---
# How To Copy a MySQL Database

This manual shows you how to copy a MySQL database on the same server and from a server to another. With this manual, you also be able to copy only one table from one MySQL database to another.

For the demonstration, we will copy the `db_test` database to `db_test_2` database. We assume that `db_test` exists.

Create the `db_test_2` database:

``` bash
mysql -u root -p
```

``` bash
> CREATE DATABASE db_test_2;
```

Dump database objects and data into SQL file using the mysqldump tool.  

## a) If you want to move the whole database:

``` bash
mysqldump -u root -p db_test > /tmp/db_test.sql
```

## b) If you want to move only certain tables:

``` bash
mysqldump -u root -p db_test tablename1 tablename2 > /tmp/db_test_tables.sql
```

Import the `db_test_tables.sql` or `db_test.sql` to your new database.

``` bash
mysql -u root -p db_test_2 < db_test_tables.sql
```
