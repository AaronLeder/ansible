# Ansible
## This is where all of my ansible playbooks go
For writeups on each playbook, available roles, and what's available, please see the directory `README/`

## Directory Layout Explained:
These are all directories in the root of `ansible/`.
```
user@maint001:~/Desktop/ansible$ tree -d -L 1
.
├── admin
├── archive
├── certificates
├── group_vars
├── inventory
├── packages
├── playbooks
├── README
├── roles
└── vault
```

### If you Want Multiple Inventories
If you don't want a super convoluted all.yml...
To have multiple inventories, I recommend the following layout;

How to have multiple `group_vars/all.yml`:
```
~/Desktop/ansible/
  ansible.cfg
  inventories/
    elastic/
      hosts.yml
      group_vars/
        all.yml          # only for elastic
    k8s/
      hosts.yml
      group_vars/
        all.yml          # only for k8s
  playbooks/
    elastic.yml
    k8s.yml
```

Run like:
```
ansible-playbook -i inventory/elastic/hosts.yml playbooks/elastic.yml --ask-vault-pass
ansible-playbook -i inventory/k8s/hosts.yml     playbooks/k8s.yml     --ask-vault-pass
```