---
comments: true
hide:
    - footer
tags:
    - Cisco
    - Network
---

# How to bring the port up from the flapping state on Cisco?

Locate the flapping port:

```bash
sh int status
```

Enter configuration mode and select the port (using `eth1/6` as an example):

```bash
conf t
```

```bash
int eth1/6
```

Bounce the port:

```bash
shut
```

```bash
no shut
```

Exit configuration mode and save:

```bash
exit
```

```bash
exit
```

```bash
wr
```
