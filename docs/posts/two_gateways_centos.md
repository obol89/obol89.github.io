---
comments: true
hide:
    - footer
tags:
    - Linux
    - CentOS
    - Network
---
# Two default gateways on CentOS

You have two or more network cards (interfaces) in one Linux system and each of these interfaces has its own default gateway. By default, you can only have one default gateway on a system.

We assume that we have two interfaces:  
`eth0`  
`eth1`

Two networks that should be used are:  
`192.168.1.0/24`  
`10.10.0.0/24`

whereby the first IP address in each respective network should be the gateway. Config files for these interfaces are in:  
`/etc/sysconfing/network-scripts/ifcfg-eth0`  
and  
`ifcfg-eth1`  
and it looks like this:  
`cat /etc/sysconfig/network-scripts/ifcfg-eth0`

``` bash
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=no
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME=eth0
UUID=7f164bfd-1ae4-4062-aadd-e2ea8bc0121e
DEVICE=eth0
ONBOOT=yes
IPADDR=192.168.1.5
PREFIX=24
GATEWAY=192.168.1.1
DNS1=8.8.8.8
DNS2=1.1.1.1
```

`cat /etc/sysconfig/network-scripts/ifcfg-eth1`

``` bash
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none
IPADDR=10.10.0.124
PREFIX=24
DNS1=8.8.8.8
DNS2=1.1.1.1
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=no
NAME=eth1
UUID=9320b706-e68e-3610-9fec-9988316bd478
DEVICE=eth1
ONBOOT=yes
GATEWAY=10.10.0.1
NETMASK=255.255.255.0
NM_CONTROLLED=yes
```

To use a second interface and address we need to add another routing table. To do this go to file:  
`vim /etc/iproute2/rt_tables`

and add at the end `1 rt2`:

``` bash
#
# reserved values
#
255     local
254     main
253     default
0       unspec
#
# local
#
#1      inr.ruhep
1 rt2
```

Now we need to add routing rules and routes:  
`ip route add default via 10.10.0.1 dev eth1 table rt2`  
`ip rule add from 10.10.0.0/24 table rt2`

You can check these changes with commands:  
`ip route show table rt2`  
`ip rule show`

If you would like to keep these changes after a system reboot it will be necessary to create systemd service. So, the first step is to create a script:  
`vim /usr/local/bin/default_routes.sh`

``` bash
# !/bin/bash
ip route add default via 10.70.70.2 dev eth1 table int
```

Don’t forget to change permissions to allow execute this file:  
`chmod +x /usr/local/bin/default_routes.sh`

Now, we can create a systemd service. Create a file and add the necessary parameters:  
`vim /etc/systemd/system/default_route.service`

``` bash
[Unit]
Description=It adds default route for eth1
Wants=network-online.target
After=network-online.target

[Service]
Type=oneshot
ExecStart=/usr/local/bin/default_routes.sh
TimeoutStartSec=0

[Install]
WantedBy=default.target
```

That’s all. Thank you.
