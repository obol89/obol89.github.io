---
comments: true
hide:
    - footer
tags:
    - Network
    - Linux
---
# Configuration of a static IP address on CentOS 8

There are a few ways to configure static IP on CentOS systems but I’ll show you how to do it just through editing config files

We need to identify our interface that we would like to edit. We can do this using the following command:

``` bash
ip a
```

``` bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:d7:61:91 brd ff:ff:ff:ff:ff:ff
    inet 192.168.178.89/24 brd 192.168.178.255 scope global dynamic noprefixroute enp0s3
       valid_lft 85177sec preferred_lft 85177sec
    inet6 2002:c2bf:e06a:0:9204:bf9d:ab95:aa91/128 scope global dynamic noprefixroute
       valid_lft 5978sec preferred_lft 2378sec
    inet6 2002:c2bf:e06a:0:771:5fa8:a2c7:4322/64 scope global dynamic noprefixroute
       valid_lft 6956sec preferred_lft 3356sec
    inet6 fe80::9204:bf9d:ab95:aa91/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
3: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default qlen 1000
    link/ether 52:54:00:13:f3:a7 brd ff:ff:ff:ff:ff:ff
    inet 192.168.122.1/24 brd 192.168.122.255 scope global virbr0
       valid_lft forever preferred_lft forever
4: virbr0-nic: <BROADCAST,MULTICAST> mtu 1500 qdisc fq_codel master virbr0 state DOWN group default qlen 1000
    link/ether 52:54:00:13:f3:a7 brd ff:ff:ff:ff:ff:ff
```

Next we need to edit or create config file for this interface in this location:

``` bash
vim /etc/sysconfig/network-scripts/ifcfg-ens10
```

and add following arguments:

``` bash
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp0s3
UUID=6f23bb02-a7bf-412d-beca-4b425d866596
DEVICE=enp0s3
ONBOOT=yes
IPADDR=192.168.178.89
PREFIX=24
GATEWAY=192.168.178.1
```

Parameters like `IPADDR`, `PREFIX`, `GATEWAY`, `DNS1`, `DNS2` etc. are almost necessary.  

We need to remember to change `BOOTPROTO` to `none` or `static`.  

Thanks!
