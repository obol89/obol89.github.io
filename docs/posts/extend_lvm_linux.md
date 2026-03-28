---
comments: true
hide:
    - footer
tags:
    - Filesystem
    - Fdisk
    - LVM
    - Linux
---
# How to extend LVM in Linux

## Physical Volumes

```bash
pvs
```

```text
PV         VG             Fmt  Attr PSize   PFree
/dev/sda2  centos_centos7 lvm2 a--  <19.00g    0
```

## Volume Groups

```bash
vgs
```

```text
VG             #PV #LV #SN Attr   VSize   VFree
centos_centos7   1   2   0 wz--n- <19.00g    0
```

## Logical Volumes

```bash
lvs
```

```text
LV   VG             Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
root centos_centos7 -wi-ao---- <17.00g
swap centos_centos7 -wi-ao----   2.00g
```

Locate the new volume to add as a Physical Volume:

```bash
fdisk -l
```

```text
Disk /dev/sda: 21.5 GB, 21474836480 bytes, 41943040 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000b3a0f

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     2099199     1048576   83  Linux
/dev/sda2         2099200    41943039    19921920   8e  Linux LVM

Disk /dev/sdb: 10.7 GB, 10737418240 bytes, 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/mapper/centos_centos7-root: 18.2 GB, 18249416704 bytes, 35643392 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/mapper/centos_centos7-swap: 2147 MB, 2147483648 bytes, 4194304 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

Partition the new disk:

```bash
fdisk /dev/sdb
```

To create new partition Press `n`  
Choose primary partition user `p`  
Change the type using `t`  
Type `8e` to change the partition type to Linux LVM  
Use `p` to print the create partition  
Press `w` to write the changes  

```text
Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table
Building a new DOS disklabel with disk identifier 0x38e14a7a.

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1):
First sector (2048-20971519, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-20971519, default 20971519):
Using default value 20971519
Partition 1 of type Linux and of size 10 GiB is set

Command (m for help): t
Selected partition 1
Hex code (type L to list all codes): 8e
Changed type of partition 'Linux' to 'Linux LVM'

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

Create the new Physical Volume:

```bash
pvcreate /dev/sdb1
```

```text
Physical volume /dev/sdb1 successfully created.
```

Verify:

```bash
pvs
```

```text
PV         VG             Fmt  Attr PSize   PFree
/dev/sda2  centos_centos7 lvm2 a--  <19.00g      0
/dev/sdb1                 lvm2 ---  <10.00g <10.00g
```

Add the PV to the volume group:

```bash
vgextend centos_centos7 /dev/sdb1
```

```text
Volume group "centos_centos7" successfully extended
```

```bash
vgs
```

```text
VG             #PV #LV #SN Attr   VSize  VFree
centos_centos7   2   2   0 wz--n- 28.99g <10.00g
```

Extend the logical volume with all free space:

```bash
lvextend -l +100%FREE /dev/centos_centos7/root
```

```text
Size of logical volume centos_centos7/root changed from <17.00 GiB (4351 extents) to 26.99 GiB (6910 extents).
Logical volume centos_centos7/root successfully resized.
```

Resize the filesystem:

```bash
xfs_growfs /dev/centos_centos7/root
```

```text
meta-data=/dev/mapper/centos_centos7-root isize=512    agcount=4, agsize=1113856 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0 spinodes=0
data     =                       bsize=4096   blocks=4455424, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal               bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
data blocks changed from 4455424 to 7075840
```

Confirm the result:

```bash
pvs
```

```text
PV         VG             Fmt  Attr PSize   PFree
/dev/sda2  centos_centos7 lvm2 a--  <19.00g    0
/dev/sdb1  centos_centos7 lvm2 a--  <10.00g    0
```

```bash
lvs
```

```text
LV   VG             Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
root centos_centos7 -wi-ao---- 26.99g
swap centos_centos7 -wi-ao----  2.00g
```

```bash
vgs
```

```text
VG             #PV #LV #SN Attr   VSize  VFree
centos_centos7   2   2   0 wz--n- 28.99g    0
```
