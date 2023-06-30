---
comments: true
hide:
    - footer
tags:
    - Linux
    - Ansible
---
# Gather or update ansible facts

Ansible facts are very useful data related to your remote systems. You can access this data in the `ansible_facts` variable.

This is how to gather or update known facts from the specific server with one command. In this example there is usage of specific python as well. If you want to you can use it without `-e` parameter.

``` bash
ansible -i /etc/ansible/hosts yourserver.yourdomain.com -m gather_facts -e 'ansible_python_interpreter=/usr/bin/python'
```
