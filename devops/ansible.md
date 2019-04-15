# Ansible

## Playbooks

Playbooks are YAML files that are used to build applications.

## Inventories

Inventories are target servers for playbooks. For example, a server for your web application, and a server for your database.

## Modules

There are over 450 provided modules to automate various tasks.

## Running Ansible

1. Running CLI `ansible <inventory>`

2. Running playbooks `ansible-playbook <playbook name>`

3. Check mode or "dry-run" is used to validate changes that have been made. `ansible -C <inventory>`
