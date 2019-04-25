# Ansible

## Playbooks

Playbooks are YAML files that are used to build applications.

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
