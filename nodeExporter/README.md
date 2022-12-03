# Node Exporter Installation Playbook

## Discription

This playbook is written to install the Prometheus Node Exporter on ton of servers by Ansible.

### Usage

1. Edit the inventory file under /inventory, add your servers IPs and your SSH credentials, etc..
2. Run the following command

    ```$ ansible-playbook -i inventories/inventory nodeExporterInstallation.yml```
