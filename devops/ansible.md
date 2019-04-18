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

## Remote Connection Information

By default Ansible uses native OpenSSH for remote communication when possible. When this is not possible, it will fallback into a Python implementation of OpenSSH called "paramiko".

In cases where a device does not support SFTP you should switch to SCP mode in your ansible config file.

Ansible assumes the use of SSH keys. When using password authentication instead supply the `--ask-pass` flag. If using sudo features use the `--ask-become-pass` flag.

Ansible runs best when it is near the machine being managed. Therefore if running in the cloud it is best to setup a machine inside the cloud to run it on.
