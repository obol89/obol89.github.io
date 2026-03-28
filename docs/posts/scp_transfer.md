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

Syntax:

```text
scp [-346BCpqrTv] [-c cipher] [-F ssh_config] [-i identity_file] [-J destination] [-l limit] [-o ssh_option] [-P port] [-S program] source ... target
```

All options are documented in `man scp`.

## Copy files and directories between two systems

``` bash
scp file.txt username@ip_address:/remote/directory
```

If SSH on the remote host is listening on a non-default port, specify it with `-P`:

``` bash
scp -P 2232 file.txt username@ip_address:/remote/directory
```

To copy a directory recursively, use `-r`:

``` bash
scp -r /local/directory username@ip_address:/remote/directory
```
