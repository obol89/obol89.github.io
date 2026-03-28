---
comments: true
hide:
    - footer
tags:
    - MySQL
    - Database
    - Linux
---
# Host is not allowed to connect to this MySQL server

## Symptoms

```text
Error: The test connection to the database server has failed because of network problems:

Host '203.0.113.2' is not allowed to connect to this MySQL server
```

## Cause

*root user is not allowed to access the MySQL database server remotely.*  

## Solution

Log in to MySQL on the remote server and create a user allowed to connect from 203.0.113.2:

```bash
mysql> CREATE USER 'root'@'203.0.113.2' IDENTIFIED BY 'root_password';
```

```bash
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'203.0.113.2' WITH GRANT OPTION;
```

```bash
mysql> FLUSH PRIVILEGES;
```

Or allow access from any host:

```bash
mysql> CREATE USER 'root'@'%' IDENTIFIED BY 'root_password';
```

```bash
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
```

```bash
mysql> FLUSH PRIVILEGES;
```
