---
comments: true
hide:
    - footer
tags:
    - Filesystem
    - Linux
---
# How to move /var to new partition in CentOS 7

```bash
mkdir /mnt/var_new
```

```bash
mount /dev/vg00/lvo10 /mnt/var_new
```

```bash
rsync -avH /var/ /mnt/var_new
```

```bash
diff -r /var /mnt/var_new
```

```bash
mv /var /var_old
```

```bash
mkdir /var
```

Add fstab entry:

```text
/dev/vg00/lvo10 /var xfs defaults 0 0
```

```bash
umount /mnt/var_new
```

```bash
mount /var
```
