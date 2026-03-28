---
comments: true
hide:
    - footer
tags:
    - MySQL
    - Database
    - Linux
---
# How to change MySQL Password Policy Level

MySQL servers ship with a validate password plugin that enforces a password policy. This can cause the error:
`ERROR 1819 (HY000): Your password does not satisfy the current policy requirements.`

The policy level can be changed at runtime via the MySQL CLI or permanently in the config file (`my.cnf` or `mysqld.cnf`).

View current `validate_password` settings:

```bash
mysql> SHOW VARIABLES LIKE ‘validate_password%’;
```

```text
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

The default level is `MEDIUM`. Change it to `LOW` (requires only minimum 8-character length):

```bash
mysql> SET GLOBAL validate_password.policy=LOW;
```

```text
Query OK, 0 rows affected (0.02 sec)
```

To make this permanent, add to the MySQL configuration (`my.cnf`):

```text
[mysqld]
validate_password.policy=LOW
```
