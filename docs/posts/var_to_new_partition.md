---
comments: true
hide:
    - footer
tags:
    - Filesystem
    - Linux
---
# How to move /var to new partition in CentOS 7

If you need to move you /var to new partition in CentOS 7 you will need commands like below.

Create new directory:  

``` bash
mkdir /mnt/var_new
```

Mount lvm:  

``` bash
mount /dev/vg00/lvo10 /mnt/var_new
```

Copy and sync content:  

``` bash
rsync -avH /var/ /mnt/var_new
```

Check content in two directories:  

``` bash
diff -r /var /mnt/var_new
```

Rename `/var`:  

``` bash
mv /var /var_old
```

Create new `/var`:  

``` bash
mkdir /var
```

Add fstab entry:  

``` bash
/dev/vg00/lvo10 /var xfs defaults 0 0
```

Unmount `/mnt/var_new`:  

``` bash
umount /mnt/var_new
```

Mount `/var`:  

``` bash
mount /var
```
