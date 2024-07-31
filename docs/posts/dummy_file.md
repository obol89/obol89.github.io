---
comments: true
hide:
    - footer
tags:
    - Filesystem
    - Linux
---
# Creating empty file to take disk space on Linux

The purpose of creating empty file might be to test the disk alerting, or just check if there are no issues with filesystem.

The command is simple:

```bash
dd if=/dev/zero of=/var/filename2 bs=$((1024*1024)) count=$((16*1024))
```

`count` is the place where you should put multiple of 1024MB.
