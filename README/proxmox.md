# Proxmox

## Dependencies
- `community.proxmox` ansible collection

## How to Create a Token ID
### 1. Decide on your API user

You’ll use a dedicated Proxmox user for automation, e.g.:
```text
ansible@pve
```
* ansible > username
* @pve > local Proxmox realm (standard for local accounts)

You can reuse an existing user or create a fresh one.

### 2. (Optional) Create the ansible@pve user

In the Proxmox Web UI:

1. Go to:
    * `Datacenter > Permissions > Users`
2. Click `Add`.
3. Fill in:
    * User name: ansible
    * Realm: PVE
    * Enabled: True
    * Password: a strong random password (you won’t usually type it; token will be used instead)
    * Expire: as needed (or leave blank for no expiry)
4. Click `Add`.

### 3. Assign permissions to the user

Still in the Proxmox Web UI:

1. Go to:
    * `Datacenter > Permissions`
2. Click `Add > Add Permission`.
3. Set:
    * Path:
      * / > permissions across the whole cluster (simple but broad), or
      * a narrower path like /vms, /storage, or /node/<node> if you want least privilege.
    * User: ansible@pve
    * Role:
      * PVEVMAdmin > usually enough for VM operations (clone, start, stop, shutdown), or
      * PVEAdmin > broader admin powers (more than you strictly need).

4. Click `Add`.

*Note: ansible@pve must have enough permissions on the paths where you’ll clone/start/shutdown VMs.*

### 4. Create the API token

Now create an API token for this user:

1. Go to:
    * `Datacenter > Permissions > API Tokens`
2. Click `Add`.
3. Fill in:
    * User: ansible@pve
    * Token ID (token name): e.g. ansible-maint001
    * Privilege Separation / similar option:
    * Leave enabled so the token inherits the user’s permissions.
    * Add a Comment if you want ("Token for Ansible on maint001").
4. Click `Add`.

Proxmox will show:

Token ID:
```text
ansible@pve!ansible-maint001
```

Value (the secret):
```text
82be...
```

### Copy the Value immediately. **You will not see it again.**

## Tags
- proxmox_onnectivity
- proxmox_start_vms
- proxmox_clone_vms
- proxmox_shutdown_vms

## Notes:
For Proxmox connections; 
  * hosts: localhost
  * connection: local

With community.general.proxmox_kvm you’re **not** SSH’ing into the Proxmox node; you’re making API calls (HTTPS) from your Ansible controller to Proxmox.