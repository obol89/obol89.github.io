---
comments: true
hide:
    - footer
tags:
    - MySQL
    - Database
    - Linux
---
# MySQL 8 – Authentication plugin ‘caching_sha2_password’ cannot be loaded

MySQL 8 changed the default authentication plugin to `caching_sha2_password`, which older clients do not support. Switch the user to native password authentication:

```bash
ALTER USER ‘username’@’ip_address’ IDENTIFIED WITH mysql_native_password BY ‘password’;
```
