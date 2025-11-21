# Role: prepare_hosts
Prepare hosts for ansible deployment.

## Usage
```
ansible-playbook playbooks/<name>.yml -t <tag(s)>
```

## Available tags
- prepare_hosts
- share_pub_key
- create_pki
- update_etc_hosts

## Notes:
The tag, `prepare_hosts` will run the entire role.