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

`vagrnt provision` Provisions virtual server resources as defined in your Vagrantfile (for example, running an Ansible playbook).
