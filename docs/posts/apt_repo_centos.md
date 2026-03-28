---
comments: true
hide:
    - footer
tags:
    - APT
    - Repository
    - Linux
---
# Create an APT repository on CentOS

Systems like Ubuntu, Debian or Mint use apt packages. You can also share apt packages from CentOS by creating an apt repository stored locally with GPG-signed packages.

## Preparing gpg keys  

Generating new keys can require some time for entropy generation.
**as root, no sudo!**

```bash
gpg --gen-key
```

On the first run, break after it has generated directories and files in `~/.gnupg/`. Then add the SHA256 requirement to `gpg.conf`:

``` bash
cat <<'EOF' >> ~/.gnupg/gpg.conf
cert-digest-algo SHA256
digest-algo SHA256
EOF
```

Run gpg again and go through all the prompts to generate the key:

```bash
gpg --gen-key
```

This step might take a long time. To speed up entropy generation, run in another terminal:  

``` bash
while true; do dd if=/dev/sda of=/dev/zero; find / | xargs file >/dev/null 2>&1; done
```

Stop it with ctrl+c when gpg key generation finishes. Then export the generated keys:

```bash
gpg --list-keys
```

```bash
gpg --output your-PUBLIC-key-name-here.gpg --armor --export 123456AB
```

```bash
gpg --output your-PRIVATE-key-name-here.gpg --armor --export-secret-key 123456AB
```

Place the public key in the repository directory.

## Installing required packages

```bash
yum -y install epel-release dpkg-dev tar
```

## Script to build the repository

``` bash
updatescript=/mnt/mirror/ubuntu/example-ubuntu/update-repo.sh
cat <<'EOFSH' >${updatescript}
#!/bin/sh

# working directory
repodir=/mnt/mirror/ubuntu/example-ubuntu/
cd ${repodir}

# create the package index
dpkg-scanpackages -m . > Packages
cat Packages | gzip -9c > Packages.gz

# create the Release file
PKGS=$(wc -c Packages)
PKGS_GZ=$(wc -c Packages.gz)
cat <<EOF > Release
Architectures: all
Date: \$(date -R)
MD5Sum:
 $(md5sum Packages  | cut -d" " -f1) $PKGS
 $(md5sum Packages.gz  | cut -d" " -f1) $PKGS_GZ
SHA1:
 $(sha1sum Packages  | cut -d" " -f1) $PKGS
 $(sha1sum Packages.gz  | cut -d" " -f1) $PKGS_GZ
SHA256:
 $(sha256sum Packages | cut -d" " -f1) $PKGS
 $(sha256sum Packages.gz | cut -d" " -f1) $PKGS_GZ
EOF
gpg -abs -o Release.gpg Release
EOFSH
chmod 755 ${updatescript}
```

## Adding packages to the repository

Move any package to the repository directory and run the update script:

```bash
./update-repo.sh
```

To use the repository as a client, import the public key and add an entry in `sources.list`:

``` bash
sudo wget http://mirror.example.com/ubuntu/your-PUBLIC-key-name-here.gpg
```  

``` bash
sudo apt-key add /root/your-PUBLIC-key-name-here.gpg
```

``` bash
sudo wget http://mirror.example.com/ubuntu/example-ubuntu/example-ubuntu.list -O /etc/apt/sources.list.d/example-ubuntu.list
```

```bash
apt-get update
```
