---
comments: true
hide:
    - footer
tags:
    - Filesystem
    - Linux
    - SSH
---
# Required permissions for SSH keys, files and folders

|Item|Sample|Numeric|Bitwise|Command|
|----|------|-------|-------|-------|
|SSH folder|`~/.ssh`|`700`|`drwx------`|`chmod 0700 ~/.ssh`|
|Public key|`~/.ssh/id_rsa.pub`|`644`|`-rw-r--r--`|`chmod 0644 ~/.ssh/id_rsa.pub`|
|Private key|`~/.ssh/id_rsa`|`600`|`-rw-------`|`chmod 0600 ~/.ssh/id_rsa`|
|Home folder|`~`|`755` at most|`drwxr-xr-x` at most|`chmod 0755 ~`|
