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

If you are not able to connect to your MySQL server you might not have the right permission to get there.

## Symptoms

``` bash
Error: The test connection to the database server has failed because of network problems:

Host '203.0.113.2' is not allowed to connect to this MySQL server
```

## Cause

*root user is not allowed to access the MySQL database server remotely.*  

## Solution

Log in to the MySQL on the remote database server and create a user who is allowed to access database server remotely from 203.0.113.2:

``` bash
mysql> CREATE USER 'root'@'203.0.113.2' IDENTIFIED BY 'root_password';
```

``` bash
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'203.0.113.2' WITH GRANT OPTION;
```

``` bash
mysql> FLUSH PRIVILEGES;
```

Or a database user who can access the server from any host:

``` bash
mysql> CREATE USER 'root'@'%' IDENTIFIED BY 'root_password';
```

``` bash
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
```

``` bash
mysql> FLUSH PRIVILEGES;
```
