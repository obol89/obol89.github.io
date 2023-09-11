---
comments: true
hide:
    - footer
tags:
    - Filesystem
    - Fdisk
    - Linux
---
# Extending disk space on Linux without LVM

This method requires host rebooting and using `lsblk`, `fdisk` tools.  
Removing or creating a partition doesn't remove a data or a filesystem on a disk. It will only update the MBR or GPT information where partition lies and its size.

## Find partition to extend

Run `lsblk` command to see which mountpoint is assigned to which partition

```bash
root@debian1101:~# lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0   20G  0 disk
├─sda1   8:1    0  4.1G  0 part /
├─sda2   8:2    0    1K  0 part
├─sda5   8:5    0  1.7G  0 part /var
├─sda6   8:6    0  976M  0 part [SWAP]
├─sda7   8:7    0  371M  0 part /tmp
└─sda8   8:8    0 12.9G  0 part /home
sr0     11:0    1 1024M  0 rom
```

Let's say we would like to extend `/var` partition

## Add diskspace, extend disk, i.e. in vCenter, VirtualBox

Wherever you have your virtual machine installed, you need to shut it down and increase disk size.

## Use `fdisk` to delete and re-add partition

Run `fidsk /dev/sda` to start the fdisk menu for this specific partition:

```bash
root@debian1101:~# fdisk /dev/sda

Welcome to fdisk (util-linux 2.36.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.
```

### Print the partition table

With `p` command you can print partition table:

```bash
Command (m for help): p
Disk /dev/sda: 20 GiB, 21474836480 bytes, 41943040 sectors
Disk model: VBOX HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xfeb22017

Device     Boot    Start      End  Sectors  Size Id Type
/dev/sda1  *        2048  8638463  8636416  4.1G 83 Linux
/dev/sda2        8640510 41940991 33300482 15.9G  5 Extended
/dev/sda5        8640512 12167167  3526656  1.7G 83 Linux
/dev/sda6       12169216 14168063  1998848  976M 82 Linux swap / Solaris
/dev/sda7       14170112 14929919   759808  371M 83 Linux
/dev/sda8       14931968 41940991 27009024 12.9G 83 Linux
```

### Delete the partition we want to extend with `d` command

```bash
Command (m for help): d
Partition number (1,2,5-8, default 8): 5

Partition 5 has been deleted.
```

### Create a new partition with `n` command

In this stage you need to be careful which blocks are youg going to use. This part might be tricky.

```bash
Command (m for help): n
All space for primary partitions is in use.
Adding logical partition 8
First sector (8642558-12167167, default 8642560):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (8642560-12167167, default 12167167):

Created a new partition 8 of type 'Linux' and of size 1.7 GiB.
```

### Save new partition table with `w` command

```bash
Command (m for help): w
The partition table has been altered.

The kernel still uses the old partitions. The new table will be used at the next reboot.
Syncing disks.
```

## Resize filesystem for new partition

```bash
root@debian1101:~# resize2fs /dev/sda8
resize2fs 1.46.2 (28-Feb-2021)
```

That's it. Your partition will be bigger now.
