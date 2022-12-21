---
hide:
    - footer
tags:
    - Iptables
    - Network
    - Linux
---
# Useful iptables commands

## Show iptables with line numbers

``` bash
iptables -nL --line-numbers
```

`-n` – Numeric output. IP addresses and port numbers will be printed in numeric format.  
`-L` – List all rules in the selected chain. If no chain is selected, all chains are listed.

## Insert (add) rule in the specific place in iptables as it works descending

``` bash
sudo iptables -I INPUT 9 -i eth0 -d 192.168.1.254 -j ACCEPT -m comment --comment "my new comment"
```

Number “9” in the example above stands for this rule will be inserted on the ninth place in iptables.
