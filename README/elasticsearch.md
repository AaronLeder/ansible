# Elasticsearch

## Dependencies 
The following dependencies are required! All of them will be installed by the playbook, if not already installed.

### Ansible
This playbook was built and tested, with the following;
```
user@maint001:~$ ansible --version
ansible [core 2.16.3]
  config file = /home/user/Desktop/ansible/elastic/ansible.cfg
  configured module search path = ['/home/user/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  ansible collection location = /home/user/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.12.3 (main, Aug 14 2025, 17:47:21) [GCC 13.3.0] (/usr/bin/python3)
  jinja version = 3.1.2
  libyaml = True
```

#### Collections
* [ansible.builtin](https://docs.ansible.com/projects/ansible/latest/collections/ansible/builtin/index.html)
* [ansible.posix](https://docs.ansible.com/projects/ansible/latest/collections/ansible/posix/index.html)
* [ansible.community.crypto](https://docs.ansible.com/projects/ansible/latest/collections/community/crypto/index.html)

### Applications & Programs
* ssh-pass
* gpg
* apt-transport-https
* nfs-kernel-server

## Elastic
Role(s):
* elastic

Tag(s):
* 

## Kibana
Role(s):
* kibana

Tag(s):
* 

## Logstash
Role(s):
* Logstash

Tag(s):
* 

