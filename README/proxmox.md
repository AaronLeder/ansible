Why hosts: localhost and connection: local?

Because with community.general.proxmox_kvm you’re:

Not SSH’ing into the Proxmox node

Instead, you’re making API calls (HTTPS) from your Ansible controller to Proxmox

So:

hosts: localhost
connection: local