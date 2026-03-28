---
comments: true
hide:
    - footer
tags:
    - MySQL
    - Database
    - Linux
---
# How To Copy a MySQL Database

Copy a MySQL database on the same server or between servers. This also works for copying individual tables.

In this example, `db_test` is copied to `db_test_2`. `db_test` must already exist.

Create the target database:

```bash
mysql -u root -p
```

```bash
CREATE DATABASE db_test_2;
```

## a) If you want to move the whole database

```bash
mysqldump -u root -p db_test > /tmp/db_test.sql
```

## b) If you want to move only certain tables

```bash
mysqldump -u root -p db_test tablename1 tablename2 > /tmp/db_test_tables.sql
```

Import the dump into the new database:

```bash
mysql -u root -p db_test_2 < db_test_tables.sql
```
