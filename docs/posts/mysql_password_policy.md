---
hide:
    - footer
tags:
    - MySQL
    - Database
    - Linux
---
# How to change MySQL Password Policy Level

The latest MySQL servers come with a validate password plugin. This plugin configures a password policy to make server MySQL more secure.

While changing the password, I got the error:  
`ERROR 1819 (HY000): Your password does not satisfy the current policy requirements.`

Follow the below tutorial to change password policy level for MySQL.

To change the default password policy level, we can change the settings at runtime using the command line or in the config file (`my.cnf` or `mysqld.cnf`) permanently.

Login to MySQL command prompt and execute the below query to view current settings of `validate_password`.

``` bash
mysql> SHOW VARIABLES LIKE 'validate_password%';
```

``` bash
+--------------------------------------+--------+
| Variable_name                        | Value  |
+--------------------------------------+--------+
| validate_password.check_user_name    | OFF    |
| validate_password.dictionary_file    |        |
| validate_password.length             | 8      |
| validate_password.mixed_case_count   | 1      |
| validate_password.number_count       | 1      |
| validate_password.policy             | MEDIUM |
| validate_password.special_char_count | 1      |
+--------------------------------------+--------+
7 rows in set (0.06 sec)
```

The default level is `MEDIUM`, we can change it to `LOW` by using the below query. The `LOW` level required only the password’s length to min 8 characters.

``` bash
mysql> SET GLOBAL validate_password.policy=LOW;
```

``` bash
Query OK, 0 rows affected (0.02 sec)
```

To make this setting permanent edit MySQL configuration (`my.cnf`) file and add below settings.

``` bash
[mysqld]
validate_password.policy=LOW
```
