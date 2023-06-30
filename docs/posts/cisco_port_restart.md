---
comments: true
hide:
    - footer
tags:
    - Cisco
    - Network
---

# How to bring the port up from the flapping state on Cisco?

If you have issues with port on your Cisco switch you need to use these commands:
With this command you will be able to locate flapping port:

``` bash
sh int status
```

Go to the configuration state:

``` bash
conf t
```

Let's use `eth1/6` as an example.

``` bash
int eth1/6
```

Switch off and switch on the port:

``` bash
shut
```

``` bash
no shut
```

We need to `exit` twice:

``` bash
exit
```

``` bash
exit
```

And save our config:

``` bash
wr
```
