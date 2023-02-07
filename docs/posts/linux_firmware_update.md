---
hide:
    - footer
tags:
    - Linux
    - Ubuntu
    - Debian
---
# Update Linux firmware from terminal

We will use `fwupd` which should be installed. If not, install it using package manager.

Open terminal and update your system:

```bash
sudo apt update && sudo apt upgrade -y
```

Next, just use the following commands. It will start the daemon, refresh the list of available firmware and install updates.

```bash
sudo systemctl start fwupd
```

```bash
sudo fwupdmgr refresh
```

```bash
sudo fwupdmgr update
```

That's it. After that your firmware will be up to date.
