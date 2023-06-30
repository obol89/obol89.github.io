---
comments: true
hide:
    - footer
tags:
    - Filesystem
    - SCP
    - Linux
---
# How to use SCP command to transfer files

SCP (secure copy) is a command-line utility that allows you to securely copy files and directories between two locations.

SCP command syntax

``` bash
scp [-346BCpqrTv] [-c cipher] [-F ssh_config] [-i identity_file] [-J destination] [-l limit] [-o ssh_option] [-P port] [-S program] source ... target
```

Every option is explained in manual that you can reach using command:

``` bash
man scp
```

Copy Files and Directories between two systems with SCP

``` bash
scp file.txt username@ip_address:/remote/directory
```

If SSH on the remote host is listening on a port other than default 22 then you can specify port using the `-P` parameter:

``` bash
scp -P 2232 file.txt username@ip_address:/remote/directory
```

To copy directory you need to use `-r` parameter, that means recursive:

``` bash
scp -r /local/directory username@ip_address:/remote/directory
```
