---
comments: true
hide:
    - footer
tags:
    - Network
    - Linux
---
# How to test link speed between two hosts

In this manual we are going to use `iperf`

## Installation

On both hosts run:

```bash
sudo apt-get install iperf
```

## Start `iperf` as a server on one of the machines

```bash
iperf -s
```

## Run `iperf` as a client

```bash
iperf -c IPERF_SERVER_ADDRESS
```

After this command you will see something like:
```bash
user@IPERF_CLIENT:~$ iperf -c IPERF_SERVER_ADDRESS
------------------------------------------------------------
Client connecting to IPERF_SERVER_ADDRESS, TCP port 5001
TCP window size: 16.0 KByte (default)
------------------------------------------------------------
[  3] local 192.168.0.4 port 37248 connected with 192.168.0.5 port 5001
[ ID] Interval       Transfer     Bandwidth
[  3]  0.0-10.0 sec  1.04 GBytes    893 Mbits/sec
```

As you can see it is running on the 5001 port by default. If you want to change the port, use `-p` parameter.
