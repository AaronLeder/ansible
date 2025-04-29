# ansible
ansible playbooks

# Ansible Elastic Stack (from Connor)
To prepare for deployment:
1. Prerequisites:
   ```
   sshpass
     fatal: [rhel1]: FAILED! => {"msg": "to use the 'ssh' connection type with passwords or pkcs11_provider, you must install the sshpass program"}
   ```
2. Edit inventory.yml to update node names/IP addresses
    ```
      hosts:
        host1.my.tld: <- This can be a hostname **or** IP address
          ansible_host: 192.168.1.1
    ```
3. Edit group_vars/all.yml
    1. Update `esVersion` to reflect desired version number for the Stack
    2. Update passwords
4.  Edit my-cluster-elasticsearch_cluster.yml
    1. Please note, this naming convention allows you to support multiple clusters with a single playbook if needed. You can set `<some name>_elasticsearch_cluster.yml` to differentiate between cluster, along with a 2nd (or 3rd) inventory.yml file that reflects the cluster name.
    2. Change any combination of node names and roles as desired. The default config will create a 3 node cluster with all nodes having all roles.
5. Run `ansible-playbook main.yml -i inventory.yml -t prepare_hosts --ask-vault-pass` first to generate certificates and configure the base OS itself
6. If deploying Fleet:
   1. Download Elastic Agent for your version of the Stack
   2. Place the `elastic-agent-<version>.tar.gz` file in the `packages` directory
7. Run `ansible-playbook main.yml -i inventory.yml --ask-vault-pass` to deploy all configured node types 
