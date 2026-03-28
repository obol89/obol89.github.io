---
comments: true
hide:
    - footer
tags:
    - Filesystem
    - Linux
---
# Creating empty file to take disk space on Linux

Useful for testing disk alerting or verifying filesystem behavior.

```bash
dd if=/dev/zero of=/var/filename2 bs=$((1024*1024)) count=$((16*1024))
```

`count` is a multiple of 1024 MB.

Alternative:

```bash
fallocate -l 85G filename3
```
