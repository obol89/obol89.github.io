---
hide:
    - footer
tags:
    - MySQL
    - Database
    - Linux
---
# MySQL 8 – Authentication plugin ‘caching_sha2_password’ cannot be loaded

This is an issue on MySQL 8 that changed the default authentication method and mysql client doesn’t understand it.
If you want to use MySQL 8 you should set it up using the native password authentication method.
To do that you need to use the following command in your MySQL instance:

``` bash
ALTER USER 'username'@'ip_address' IDENTIFIED WITH mysql_native_password BY 'password';
```
