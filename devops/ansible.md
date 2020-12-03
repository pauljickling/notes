# Ansible

Ansible is a configuration management tool that defines how software should be provisioned. Ansible differs from other popular configuration management tools like Puppet and Chef because it is *agentless*. Ansible operates by using SSH to connect to a server, and run a set of tasks, and as long as the server you are connecting to has Python installed (and if you are using Linux servers, it almost certainly will), it will just work.

So like bash scripts, Ansible is used for for running a series of tasks to configure an installation of some software. However Ansible has the advantage of being idempotent. This means it makes it much easier to handle edge cases compared to bash scripts without creating a lot of extra complexity.

## Tech Stack

Under the hood, Ansible is just a bunch of Python, and it is open source so it is easy to take a look at what is going on. Ansible mostly uses yaml files for defining configurations, and it also uses Jinja as a templating engine to allow a little more flexibility with how those yaml files are composed. The Jinja templating syntax with look very familiar to anyone that has used Django's templating language.

## The Ansible Config File

The `ansible.cfg` file is primarily used to point to the path for various files like inventories, and roles that could exist in various places because of the needs of your configuration.

## Inventories

Ansible starts out with an inventory file. An inventory file is just a list of servers with information about how to connect to that server. There are also dynamic inventories which is useful for things like AWS EC2 instances. You can use the AWS API to collect the information you need (since you might not have a fixed IP address that you are using).

## Modules

Modules are what are used to configure things. Essentially, Python scripts that are executed on the specified machine. Some of the first modules you will learn about are `ping` and `setup`. `ping` is used to test connections to various servers, and `setup` will return a list of *facts* about that server.

As mentioned before, Ansible is idempotent so these modules won't change anything if the remote server already has the desired state.

The amount of Ansible modules available are significant, and can be found [here](https://docs.ansible.com/ansible/latest/modules/modules_by_category.html).

## Plugins

Ansible also has plugins. One useful one is called **Lookup**. You can use Lookup to find things like files, which is a useful way of managing API keys.

## Directory Structure

There are a lot of ways directories can be structured, but some best practices are to have the `ansible.cfg` and playbook file at the root directory, and then have inventory directories for your inventory files, and a group variables directory that has variables that only apply to certain groups of the inventory.

## Ansible CLI

Some useful Ansible command lines:

`ansible {host name} -m setup` is used to gather facts about the provided servers such as file systems, memory, OS, etc.

Some flags used by Ansible:

`-b` for "becomes". This runs a command as a sudo user.

`-m` for "module". Runs a particular module.

`-a` for passing optional CLI arguments.

`-K` for "`--ask-become-pass`" to supply a password

`--limit "{server IP/name}"` limits ansible ad-hoc command to specified server. You can use regular expressions for your limit argument if you prefix the quoted arguments with a tilde `~` character.

`-B {seconds}` specifies the max amount of time to let a job run

`-P {seconds}` spcifies the amount of time to wait between polling a server for the current job status

`ansible-playbook {playbook} --list-hosts` lists the hosts that will be affected by running the playbook

## Roles

A group of tasks that executes one or more modules with the purpose of setting something up. Roles can be located in a lot of different places in your directory structure, so you should definitely specify the path in your ansible config file.

An example might be your install role that installs all the prerequisite things for your software:

`ansible-galaxy install -r path/to/`

Roles will setup things by executing a sequential order of modules. Once you have your role configurations organized you are ready to compose a playbook.

## Playbook

A playbook will specify hosts from your inventory, and then the roles that are executed.

One thing to note is that `{{ inventory_hostname }}` is a magical variable name that always specifies the correct server item from your inventory.

Tags can also be used to create flags that are only executing specific roles. For example:

`ansible-playbook playbook.yaml -t setup`

Runs the playbook, but just the setup tag.

One useful thing to do in your playbook is volume management which can extend volume arbitrarily for cloud services like AWS.

### Playbook Anatomy

`- hosts: {hosts}` Describes which hosts the playbook applies to. Possible values could either be `all` or a value defined in your `hosts` file.

`become: {yes/no}` Describes if changes should be made as the root user

`tasks:` Describes the tasks that will be performed

`-name: {description}` Human readable account of what is happening
