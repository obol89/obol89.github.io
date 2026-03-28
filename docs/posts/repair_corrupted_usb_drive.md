---
comments: true
hide:
    - footer
tags:
    - Filesystem
    - Fdisk
    - Linux
---
# How to repair corrupted USB drive in Linux

If your USB drive is unreadable, unwritable, or showing a reduced size, follow the steps below.

Locate the USB drive name with `lsblk`.

## Remove old partitions

Open `fdisk` and delete all existing partitions:

```bash
fdisk /dev/sdc
```

Type `d` and press `enter` and proceed with `d` and `enter` to delete another portion, if necessary.

## New partitions

Next, we need to create a new partition.  
Type `n` for new partition.  
Type `p` to make this primary.  
Type `1` to make this the first partition.  
`enter` to accept default first sector and last sector.  
Type `w` to write the new partition information

Unmount the drive:

```bash
umount /dev/sdc1
```

## New filesystem

```bash
mkfs.ext4 -f /dev/sdc1
```

or

```bash
mkfs.ntfs -f /dev/sdc1
```
