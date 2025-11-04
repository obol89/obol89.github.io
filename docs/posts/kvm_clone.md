---
comments: true
hide:
    - footer
tags:
    - KVM
    - Linux
---
# Clone and reset KVM virtual machine

## Clone your VM and spawn new instances in KVM

List all VMs to see which one you would like to clone:

```shell
virsh list --all
```

If the chosen one is running, shut it down:

```shell
virsh suspend debian12-base
```

Clone the VM:

```shell
virt-clone --original debian12-base --name debian12-base-clone01 --auto-clone
```

Resume the suspended base:

```shell
virsh resume debian12-base
```

## Prepare new system to be fresh and working

```shell
virt-sysprep -d debian12-base-clone01  --root-password password:abcABC123 --firstboot-command 'dpkg-reconfigure openssh-server'
```
