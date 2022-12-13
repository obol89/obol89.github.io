---
hide:
    - footer
tags:
    - Filesystem
    - Diskpart
    - Windows
---
# How to repair corrupted USB drive in Windows

Restore a USB drive using Diskpart – Windows

Open a command Prompt as administrator (`cmd.exe`) and use the following commands.

1. `Diskpart` then press `Enter`  
2. `List Disk` then press `Enter`  
3. `Select Disk X` (where X is the disk number of your USB drive) then press `Enter`  
4. `Clean` then press `Enter`  
5. `Create Partition Primary` then press `Enter`
6. `Format fs=Fat32 Quick` then press `Enter` (You can also use `NTFS` or `exFAT`)
7. `Active` then press `Enter`
