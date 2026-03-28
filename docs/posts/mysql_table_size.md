---
comments: true
hide:
    - footer
tags:
    - MySQL
    - Database
    - Linux
---
# How to get the size of a table in MySQL

The `INFORMATION_SCHEMA`.`TABLES` table contains around 20 columns. To determine disk space used by tables, we need two columns: `DATA_LENGTH` and `INDEX_LENGTH`.

* `DATA_LENGTH` is the length (or size) of all data in the table (in bytes).
* `INDEX_LENGTH` is the length (or size) of the index file for the table (also in bytes).

Query to list all tables in a database with their size in megabytes:

```sql
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

Single-line version:

```sql
SELECT TABLE_NAME AS `Table`, ROUND((DATA_LENGTH + INDEX_LENGTH) /
 1024 / 1024) AS `Size (MB)` FROM information_schema.TABLES WHERE
TABLE_SCHEMA = "bookstore" ORDER BY (DATA_LENGTH + INDEX_LENGTH) DESC;
```

Example output:

```text
+----------------------------------+-----------+
| Table                            | Size (MB) |
+----------------------------------+-----------+
| book                             |       267 |
| author                           |        39 |
| post                             |        27 |
| cache                            |        24 |
...
```

To get the size of a single table, add `AND TABLE_NAME = “your_table_name”` to the `WHERE` clause:

```sql
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

Single-line version:

```sql
SELECT TABLE_NAME AS `Table`, ROUND((DATA_LENGTH + INDEX_LENGTH) /
 1024 / 1024) AS `Size (MB)` FROM information_schema.TABLES WHERE
TABLE_SCHEMA = "bookstore" AND TABLE_NAME = "book" ORDER BY (DATA_LENGTH
 + INDEX_LENGTH) DESC;
```

```text
+-------+-----------+
| Table | Size (MB) |
+-------+-----------+
| book  |       267 |
+-------+-----------+
1 row in set (0.00 sec)
```
