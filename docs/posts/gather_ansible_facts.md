---
comments: true
hide:
    - footer
tags:
    - Linux
    - Ansible
---
# Gather or update ansible facts

Ansible facts are data related to your remote systems, accessible via the `ansible_facts` variable.

Gather or update facts from a specific server (the `-e` parameter for a custom Python interpreter is optional):

``` bash
ansible -i /etc/ansible/hosts yourserver.yourdomain.com -m gather_facts -e 'ansible_python_interpreter=/usr/bin/python'
```
