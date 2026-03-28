---
comments: true
hide:
    - footer
tags:
    - Filesystem
    - Linux
---
# Delete files older than X days

List files older than 2 days:

```bash
find /home/user/some-folders-*/var/ -type f -name "*.log" -mtime +2
```

After confirming, delete them:

```bash
find /home/trades/venus-AvkLinkServer-*/var/ -type f -name "*.log" -mtime +2 -exec rm {} \;
```
