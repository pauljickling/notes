# Ansible

## Playbooks

Playbooks are YAML files that are used to build applications.

### Hosts and Users

For each play in a playbook there is a choice about which machines are targetted, and which remote user is used to complete the tasks.

The `hosts` is a list of one or more groups or host patterns, separated by colons. The `remote_user` is the name of the user account.

```
---
- hosts: webservers
  remote_user: ubuntu
  tasks:
    - service:
        name: apache
        state: started
      become: yes
      become_method: sudo
```

### Order of Operations

You can control the order in which hosts are run. The default is to follow the order supplied by the inventory. Valid parameters:

- `inventory` (default)
- `reverse_inventory` reverse of the order provided by the inventory
- `sorted` sorted alphabetically
- `reverse_sorted` reverse alphabetical order
- `shuffle` shuffled order

### Tasks List

Each play contains a list of tasks. Tasks are executed in order, one at a time, against all machines matched by the host pattern before moving onto the next task.

**NOTE:** A host that fails a task is taken out of rotation entirely. In this instance the point of failure should be corrected, and the playbook rerun.

Every task should have a `name` which is included in the output from running the playbook and acts as a description of the task. Without a name the string fed to "action" will be used for output.

A basic task looks like the following:

```
tasks:
  - name: make sure apache is running
    service:
      name: httpd
      state: started
```

The *command* and *shell* modules take a list of arguments instead of a key/value pair.

## Inventories

Inventories are target servers for playbooks. For example, a server for your web application, and a server for your database.

The default inventory file is found in `/etc/ansible/hosts` and a simple configuration would look like this:

```
[webservers]
ec2-instance1
 
[webservers:vars]
ansible_host=<ip_address>
ansible_user=ubuntu
ansible_ssh_private_key_file=path/to/aws.pem
ansible_python_interpreter=/usr/bin/python3
```

### Dynamic Inventories

If your inventory fluctuates over time the default inventory solution will be insufficient for your needs.

There are two tools available for more dynamic needs: inventory plugins, and inventory scripts.

#### Inventory Script Examples

[Cobbler](https://cobbler.github.io) is an example of an inventory script. Using it involves adding the `cobbler.ini` file to `/etc/ansible`.

There is also an [EC2 external inventory script](https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/ec2.py). There are two ways to use this script. The easiest is with the `-i` CLI flag, and specifying the path:

`ansible -i ec2.py -u ubuntu us-west-1d -m ping`

You can also copy the script to `/etc/ansible/hosts` and set permissions with `chmod + x` as well as copy the *ec2.ini* file to `/etc/ansible/`.

For AWS you need to configure the Python interface for AWS, Boto. You can do this by exporting the following two environment variables:

```
export AWS_ACCESS_KEY_ID='AK123'
export AWS_SECRET_ACCESS_KEY='abc123'
```

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
