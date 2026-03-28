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

List all VMs:

```bash
virsh list --all
```

If the target VM is running, suspend it:

```bash
virsh suspend debian12-base
```

Clone the VM:

```bash
virt-clone --original debian12-base --name debian12-base-clone01 --auto-clone
```

Resume the suspended base:

```bash
virsh resume debian12-base
```

## Prepare new system to be fresh and working

```bash
virt-sysprep -d debian12-base-clone01  --root-password password:abcABC123 --firstboot-command 'dpkg-reconfigure openssh-server'
```
