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

Systems like Ubuntu, Debian or Mint are using apt packages. There is also the possibility to share apt packages from CentOS.  
We are going to create an apt repository stored locally with gpg signed packages to avoid questions like "Do you trust the source?".

## Preparing gpg keys  

Keep in mind generating new keys can require some time for entropy generation.  
**as root, no sudo!**  
`gpg --gen-key`

For the first time you run `gpg --gen-key`, break it after it has generated some directories and files. By default they are located in `~/.gnupg/`
Add the SHA256 requirement to the `gpg.conf`

``` bash
cat <<'EOF' >> ~/.gnupg/gpg.conf
cert-digest-algo SHA256
digest-algo SHA256
EOF
```

Run gpg again and this time go through all the prompts to generate the key.  
`gpg --gen-key`  
This step might take a lot of time. If you need to speed this up you can open new terminal and run something like that:  

``` bash
while true; do dd if=/dev/sda of=/dev/zero; find / | xargs file >/dev/null 2>&1; done
```

Stop it with ctrl+c when gpg keys will finish to generate.
After that you will need to export generated keys

`gpg --list-keys`

`gpg --output your-PUBLIC-key-name-here.gpg --armor --export 123456AB`

`gpg --output your-PRIVATE-key-name-here.gpg --armor --export-secret-key 123456AB`

Preferably is to put the public key available in the repository directory.

## Installing required packages

Install epel-release that let you install dpkg-dev and tar packageds you need.

`yum -y install epel-release dpkg-dev tar`

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

Move any package to the repository directory and run the update script.

`./update-repo.sh`

Using repository as a client

You need to import public key and add entry in your sources.list file.

``` bash
sudo wget http://mirror.example.com/ubuntu/your-PUBLIC-key-name-here.gpg
```  

``` bash
sudo apt-key add /root/your-PUBLIC-key-name-here.gpg
```

``` bash
sudo wget http://mirror.example.com/ubuntu/example-ubuntu/example-ubuntu.list -O /etc/apt/sources.list.d/example-ubuntu.list
```

Update available package list

`apt-get update`
