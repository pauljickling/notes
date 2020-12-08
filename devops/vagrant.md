# Vagrant

Vagrant is a virtual server provisioning tool.

## CLI

`vagrant box add {repo}/{box}` Adds a particular pre-made virtual server

`vagrant init {repo}/{box}` Creates default virtual server configuration

`vagrant up` Starts up virtual server

`vagrant halt` Shuts down virtual server

`vagrant destroy` Removes virtual server from VirtualBox

`vagrant ssh` SSH access to virtual server

`vagrant ssh-config` Manual configuration of SSH access to virtual server

`vagrant provision` Provisions virtual server resources as defined in your Vagrantfile (for example, running an Ansible playbook).

`vagrant global-status` provides the state of all active Vagrant environments in the currently running system

## Using Vagrant VMs with Ansible

When learning how to use Ansible, Vagrant is a nice solution for wanting to test out deploying configurations to multiple servers without having to actually spin up multiple cloud resources that could potentially cost you money. However sometimes when trying to run your Ansible playbook there will be problems with trying to make a SSH connection to your VMs even if you specify the variables in your inventory file. In those instances you may need to set the environmental variable `ANSIBLE_HOST_KEY_CHECKING=False` before running the playbook.
