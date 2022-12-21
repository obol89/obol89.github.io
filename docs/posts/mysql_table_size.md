---
hide:
    - footer
tags:
    - MySQL
    - Database
    - Linux
---
# How to get the size of a table in MySQL

As can see in the official documentation, the `INFORMATION_SCHEMA`.`TABLES` table contains around 20 columns, but for the purpose of determining the amount of disk space used by tables, we’ll focus on two columns in particular: `DATA_LENGTH` and `INDEX_LENGTH`.

* `DATA_LENGTH` is the length (or size) of all data in the table (in bytes).
* `INDEX_LENGTH` is the length (or size) of the index file for the table (also in bytes).

Armed with this information, we can execute a query that will list all tables in a specific database along with the disk space (size) of each. We can even get a bit fancier and convert the normal size values from bytes into something more useful and understandable to most people like megabytes.

``` bash
SELECT
  TABLE_NAME AS `Table`,
  ROUND((DATA_LENGTH + INDEX_LENGTH) / 1024 / 1024) AS `Size (MB)`
FROM
  information_schema.TABLES
WHERE
  TABLE_SCHEMA = "bookstore"
ORDER BY
  (DATA_LENGTH + INDEX_LENGTH)
DESC;
```

Version to copy:

``` bash
SELECT TABLE_NAME AS `Table`, ROUND((DATA_LENGTH + INDEX_LENGTH) /
 1024 / 1024) AS `Size (MB)` FROM information_schema.TABLES WHERE 
TABLE_SCHEMA = "bookstore" ORDER BY (DATA_LENGTH + INDEX_LENGTH) DESC;
```

In this example using the bookstore database, we’re combining the `DATA_LENGTH` and `INDEX_LENGTH` as bytes, then dividing it by 1024 twice to convert into kilobytes and then megabytes. Our result set will look something like this:

``` bash
+----------------------------------+-----------+
| Table                            | Size (MB) |
+----------------------------------+-----------+
| book                             |       267 |
| author                           |        39 |
| post                             |        27 |
| cache                            |        24 |
...
```

If you don’t care about all tables in the database and only want the size of a particular table, you can simply add `AND TABLE_NAME = “your_table_name”` to the `WHERE` clause. Here we only want information about the book table:

``` bash
SELECT
  TABLE_NAME AS `Table`,
  ROUND((DATA_LENGTH + INDEX_LENGTH) / 1024 / 1024) AS `Size (MB)`
FROM
  information_schema.TABLES
WHERE
    TABLE_SCHEMA = "bookstore"
  AND
    TABLE_NAME = "book"
ORDER BY
  (DATA_LENGTH + INDEX_LENGTH)
DESC;
```

Version to copy:

``` bash
SELECT TABLE_NAME AS `Table`, ROUND((DATA_LENGTH + INDEX_LENGTH) /
 1024 / 1024) AS `Size (MB)` FROM information_schema.TABLES WHERE 
TABLE_SCHEMA = "bookstore" AND TABLE_NAME = "book" ORDER BY (DATA_LENGTH
 + INDEX_LENGTH) DESC;
```

The results, as expected, are now:

``` bash
+-------+-----------+
| Table | Size (MB) |
+-------+-----------+
| book  |       267 |
+-------+-----------+
1 row in set (0.00 sec)
```
