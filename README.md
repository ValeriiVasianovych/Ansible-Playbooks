# This repository contains Ansible Playbooks.

Playbooks located in this directory: playbooks
Roles located in this directory: roles

### To run a playbook, use the following command:
ansible-playbook all -i /path/to/inventory /path/to/playbook.yml

### To run roles, use the following command:
ansible-playbook all -i /path/to/inventory /path/to/playbook.yml --tags "role_name"