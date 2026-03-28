---
comments: true
hide:
    - footer
tags:
    - Filesystem
    - Linux
---
# Increasing Swap Space Using a Swap File on Linux

## Step 1: Turn Off the Existing Swap

```bash
sudo swapoff -a
```

## Step 2: Create a New Swap File

Create an 8GB swap file:

```bash
sudo dd if=/dev/zero of=/swapfile bs=1G count=8
```

Adjust the `count` value to change the size of the swap file.

## Step 3: Set the Correct Permissions

```bash
sudo chmod 0600 /swapfile
```

## Step 4: Set Up the Swap Area

```bash
sudo mkswap /swapfile
```

## Step 5: Enable the Swap File

```bash
sudo swapon /swapfile
```

## Step 6: Make the Swap File Permanent

Add to `/etc/fstab`:

```text
/swapfile none swap defaults 0 0
```
