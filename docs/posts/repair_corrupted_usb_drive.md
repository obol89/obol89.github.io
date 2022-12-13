---
hide:
    - footer
tags:
    - Filesystem
    - Fdisk
    - Linux
---
# How to repair corrupted USB drive in Linux

If you have a USB drive you can’t read or write or it’s shrunk you should follow steps below.

Locate the name of your USB drive with `lsblk` command.

## Remove old partitions

Go to `fdisk` using the following command and delete all partitions.

``` bash
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

Next, you need to unmount drive with:

``` bash
umount /dev/sdc1
```

New filesystem:

Create filesystem with the following commands:

``` bash
mkfs.ext4 -f /dev/sdc1
```

or

``` bash
mkfs.ntfs -f /dev/sdc1
```
