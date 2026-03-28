---
comments: true
hide:
    - footer
tags:
    - Filesystem
    - Diskpart
    - Windows
---
# How to repair corrupted USB drive in Windows

Restore a USB drive using Diskpart.

Open a Command Prompt as administrator (`cmd.exe`) and run:

1. `Diskpart`
2. `List Disk`
3. `Select Disk X` (where X is the disk number of your USB drive)
4. `Clean`
5. `Create Partition Primary`
6. `Format fs=Fat32 Quick` (you can also use `NTFS` or `exFAT`)
7. `Active`
