---
comments: true
hide:
    - footer
tags:
    - Linux
    - Ubuntu
    - Debian
---
# Update Linux firmware from terminal

Requires `fwupd` (install via package manager if missing).

Update your system first:

```bash
sudo apt update && sudo apt upgrade -y
```

Start the daemon, refresh available firmware, and install updates:

```bash
sudo systemctl start fwupd
```

```bash
sudo fwupdmgr refresh
```

```bash
sudo fwupdmgr update
```
