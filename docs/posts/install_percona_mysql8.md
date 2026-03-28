---
comments: true
hide:
    - footer
tags:
    - MySQL
    - Database
    - Linux
---
# How To Install Percona MySQL Server 8 on CentOS 8 / RHEL 8

Update the system first:

```bash
sudo yum -y update
```

## Add Percona YUM repository

```bash
sudo yum install https://repo.percona.com/yum/percona-release-latest.noarch.rpm
```

## Enable repository for MySQL 8.0

```bash
sudo percona-release setup ps80
```

When prompted, allow disabling the RHEL 8 MySQL module.

## Install Percona Server for MySQL 8.0

```bash
sudo yum install percona-server-server percona-toolkit
```

```bash
sudo percona-release enable-only tools release
```

```bash
sudo yum install percona-xtrabackup-80
```

## Start and secure Percona MySQL Server

```bash
sudo systemctl enable mysqld
```

```bash
sudo systemctl start mysqld
```

## Find generated root temporary password

```bash
sudo grep "temporary password" /var/log/mysqld.log
```

Then run the secure installation:

```bash
mysql_secure_installation
```
