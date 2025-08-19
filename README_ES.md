# Ansible Elastic Stack
To prepare for deployment:
1. Prerequisites:
    - install sshpass
   ```
     fatal: [ubuntu001]: FAILED! => {"msg": "to use the 'ssh' connection type with passwords or pkcs11_provider, you must install the sshpass program"}
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
5. Run `ansible-playbook main.yml -i inventory.yml -t prepare_hosts` first to generate certificates and configure the base OS itself
6. If deploying Fleet:
   1. Download Elastic Agent for your version of the Stack
   2. Place the `elastic-agent-<version>.tar.gz` file in the `packages` directory
7. Run `ansible-playbook -i inventory.yml` to deploy all configured node types  





wget -4 https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-9.0.0-x86_64.rpm
wget -4 https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-9.0.0-x86_64.rpm.sha512
shasum -a 512 -c elasticsearch-9.0.0-x86_64.rpm.sha512
sudo rpm --install elasticsearch-9.0.0-x86_64.rpm

wget -4 https://www.haproxy.com/static/install_haproxy_enterprise.sh.sha512.asc
gpg --keyserver hkp://keyserver.ubuntu.com --recv-keys 0xCA2DF14657C5A207
gpg --verify ./install_haproxy_enterprise.sh.sha512.asc