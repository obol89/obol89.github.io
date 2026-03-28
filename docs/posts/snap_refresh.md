---
comments: true
hide:
    - footer
tags:
    - Snap
    - Repository
    - Linux
    - Ubuntu
    - Debian
---
# Update Snap package and snap-store

Tested on Ubuntu 22.04. Kills the current snap-store process and upgrades all snap packages.

```bash
sudo killall snap-store && snap refresh
```
