# Role: pki
Generate local PKI material on the Ansible controller.

This role creates a local certificate authority and host certificates for inventory hosts. Certificate generation is delegated to `localhost`, while certificates are generated per target inventory host.

## Usage
```bash
ansible-playbook playbooks/pki.yml -i inventory/master_hosts.yml --limit elastic -t pki
```

Generate only the local CA:

```bash
ansible-playbook playbooks/pki.yml -i inventory/master_hosts.yml --limit elastic -t pki_ca
```

Generate only host certificates:

```bash
ansible-playbook playbooks/pki.yml -i inventory/master_hosts.yml --limit elastic -t pki_host_certs
```

Reset and regenerate all PKI material:

```bash
ansible-playbook playbooks/pki.yml -i inventory/master_hosts.yml --limit elastic -t pki \
  -e pki_reset_output_dir=true \
  -e pki_confirm_reset_output_dir=true
```

## Available tags

| Tag | Purpose |
|---|---|
| `pki` | Runs the full PKI role, including directory creation, CA generation, and host certificate generation. |
| `pki_directories` | Creates the local PKI output directory on the Ansible controller. |
| `pki_ca` | Runs all local CA tasks, including CA private key, CSR, and self-signed CA certificate generation. |
| `pki_ca_key` | Generates the CA private key. |
| `pki_ca_csr` | Generates the CA certificate signing request. |
| `pki_ca_cert` | Generates the self-signed CA certificate. |
| `pki_host_certs` | Runs all host certificate tasks for each inventory host. |
| `pki_host_key` | Generates each host private key locally on the Ansible controller. |
| `pki_host_csr` | Generates each host certificate signing request. |
| `pki_host_cert` | Signs each host certificate with the locally generated CA. |
| `pki_reset` | Deletes the local PKI output directory when reset variables are explicitly enabled. |
| `destructive` | Marks tasks that remove existing PKI material. Should only be used with reset confirmation variables. |

## Variables

Common variables:

```yaml
pki_profile_name: elastic
pki_output_dir: "{{ playbook_dir }}/certificates/elastic"

pki_generate_ca: true
pki_generate_host_certs: true

pki_ca_common_name: "Elastic Local CA"
```

Standard Elastic SAN configuration:

```yaml
pki_host_subject_alt_names:
  - "DNS:{{ inventory_hostname }}"
  - "DNS:{{ ansible_hostname | default(inventory_hostname) }}"
  - "DNS:{{ ansible_fqdn | default(inventory_hostname) }}"
  - "IP:{{ ansible_host }}"

pki_host_extra_subject_alt_names: []
```

Use `pki_host_extra_subject_alt_names` in `host_vars` only when a host needs additional aliases, VIPs, alternate DNS names, or alternate IP addresses.

Example:

```yaml
pki_host_extra_subject_alt_names:
  - "DNS:elastic-vip.aaron.lan"
  - "IP:10.10.10.50"
```

## Notes:
* This role generates certificates locally on the Ansible controller using `delegate_to: localhost`.
* The CA is generated once using `run_once: true`.
* Host certificates are generated once per inventory host but are still created locally.
* Do not install `community.crypto` from inside the role. Install it before running the playbook.
* Required collection:

```bash
ansible-galaxy collection install community.crypto
```

* Destructive cleanup is disabled by default.
* To delete existing PKI material, both variables must be set:

```yaml
pki_reset_output_dir: true
pki_confirm_reset_output_dir: true
```

* If `ansible_host` is a DNS name instead of an IP address, do not use it as an `IP:` SAN. Use a separate variable for the host IP address.