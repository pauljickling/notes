# Ansible

Ansible is a configuration management tool that defines how software should be provisioned. Ansible differs from other popular configuration management tools like Puppet and Chef because it is *agentless*. Ansible operates by using SSH to connect to a server, and run a set of tasks, and as long as the server you are connecting to has Python installed (and if you are using Linux servers, it almost certainly will), it will just work.

So like bash scripts, Ansible is used for for running a series of tasks to configure an installation of some software. However Ansible has the advantage of being idempotent. This means it makes it much easier to handle edge cases compared to bash scripts without creating a lot of extra complexity.

If you wish to learn more about Ansible in a more structured way, I recommend [Ansible for DevOps by Jeff Geerling](https://www.ansiblefordevops.com/).

## Tech Stack

Under the hood, Ansible is just a bunch of Python, and it is open source so it is easy to take a look at what is going on. Ansible mostly uses yaml files for defining configurations, and it also uses Jinja as a templating engine to allow a little more flexibility with how those yaml files are composed. The Jinja templating syntax with look very familiar to anyone that has used Django's templating language.

## The Ansible Config File

The `ansible.cfg` file is primarily used to point to the path for various files like inventories, and roles that could exist in various places because of the needs of your configuration.

## Inventories

Ansible starts out with an inventory file. An inventory file is just a list of servers with information about how to connect to that server. There are also dynamic inventories which is useful for things like AWS EC2 instances. You can use the AWS API to collect the information you need (since you might not have a fixed IP address that you are using).

## Modules

Modules are what are used to configure things. Essentially, Python scripts that are executed on the specified machine. Some of the first modules you would want to take a look at to get a sense of what Ansible is about are `ping` and `setup`. `ping` is used to test connections to various servers, and `setup` will return a list of *facts* about that server.

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

`-l` or `--limit "{server IP/name}"` limits ansible ad-hoc command to specified server. You can use regular expressions for your limit argument if you prefix the quoted arguments with a tilde `~` character.

`-B {seconds}` specifies the max amount of time to let a job run

`-P {seconds}` spcifies the amount of time to wait between polling a server for the current job status

`ansible-vault encrypt {path/to/file}` encrypts a file. Useful for a variable file that contains passwords and other sensitive information.

`ansible-playbook {playbook} --list-hosts` lists the hosts that will be affected by running the playbook

`ansible-playbook {playbook} --connection=local` runs the playbook on local host (also referred to as self-provisioning)

`ansible-playbook {playbook} --check` performs a dry run of the playbook

`ansible-playbook {playbook} --extra-vars "{var}={value}"` provides variable values for your playbook. Can accept JSON or YML files for variable definitions with the syntax `--extra-vars "@vars.json"`

`ansible-playbook {playbook} --ask-vault-pass` if you use the `ansible-vault` feature you can use the `--ask-vault-pass` flag to decrypt files encrypted using ansible-vault.

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

`pre-tasks:` Are tasks that must be executed before other tasks are performed. A common pre-task would be updating a server's package management.

`tasks:` Describes the tasks that will be performed

`-name: {description}` Human readable account of what is happening

`{module name}: {module params}` After having a name for your task you will use specified Ansible modules to perform that task

`handlers:` Handlers are special tasks that are called by other tasks with the `notify:` option. You can even use `notify` for handlers so that they can call other handlers. A common type of handler might be restarting a service where a restart is necessary after certain configuration changes have taken place. Handlers only run if a task notifies the handler, and they only run once at the end of the play.

`vars:` The vars section allows you to not have to hardcode values that will be used multiple times by your playbook so you can keep your playbook DRY. Ansible accepts any variable name that would be valid in Python code. Sticking to Python style is considered good practice.

`vars_files:` If your `vars` section is lengthy, it can make sense to create separate files, and use `vars_file` to include them. When you create a vars yaml, you don't need to place them under a `vars` heading, you can have a simple yaml file that just declares the values.

`register:` Register takes the output of a command, and stores it for future use. For example, this is how environmental variables are declared (see Environmental Variables section below for an example).

## Environmental Variables

You can set an environmental variable by adding it to the remote user's `.bash_profile`.

```
- name: Add environmental variable foo
  lineinfile:
    dest: ~/.bash_profile
    regexp: '^FOO='
    line: "FOO=bar"
```

This will make the environmental variable usable with Ansible's `shell` module. To use the variable in additional tasks you need to register it.

```
- name: Register environmental variable
  shell: 'source ~/.bash_profile && echo $FOO'
  register: foo

- name: Print registered environmental variable
  debug:
    msg: "The value for the variable FOO is {{ foo.stdout }}"
```

For Linux systems you can also store global environmental variables in `/etc/environment`.

## Using Variables

Declared variables are used with double curly braces e.g. `{{ foo }}`. When you declare a variable list, you can select items in that list with square bracket notation e.g. `{{ foo[0] }}`. For responses that return array objects you can retrieve nested values with square bracket or dot notation.

Files located in the `group_vars` and `host_vars` directories are automatically loaded.

### Pre-Assigned Variables

Ansible has the magic variable `hostvars` so you can retrieve host variables from any other host defined in the inventory file. Other variables like this include `groups`, `group_names`, `inventory_hostname`, and `play_hosts`. A full list of these can be found [here](https://docs.ansible.com/ansible/latest/reference_appendices/special_variables.html). 

## Conditionals

Ansible can use certain conditionals for sections of a playbook that only need to be run when certain facts are true. Examples of conditionals:

`when: is_foo`

The task that contains this conditional will run when you have a variable registered called `is_foo` that returns a value of `True`. Nearly any conditional test you might apply in a Python script applies in Ansible.

`ignore_errors` is another conditional that will force a task to run. This is useful for tasks that always need to happen regardless of other circumstances that might occur during playbook execution.

Somewhat related to conditionals, you can also use the phrase `delegate_to` to specify the particular host for a task. `local_action` is another phrase that can include the command type, and automatically specifies the local host as the host executing the action for a particular task.

`wait_for` is a module that can be used to pause a playbook until certain conditions are true.

## Tags

Tags are a simple way to to create sets, and subsets of tasks in a playbook. Tags can be strings, or lists of strings. When running the playbook you can include the flag `--tags "foo,bar,cool"` to only run tasks with those tags, or you can use the flag `--skip-tags "foo,bar,cool"` to run everything except those tags.

## Blocks

Blocks are tasks that are bundled together. Blocks are useful if a set of tasks logically fit together for a certain service to operate. You can use conditionals with blocks to improve error handling for your services.

## Importing Tasks

Just as variable files can be imported into a playbook

```
var_files:
  - vars.yml
```

Tasks can also be imported into a playbook

```
tasks:
  - import_tasks: tasks.yml
```

As with variables, your tasks yaml file will be a flat list. Import tasks are always run in a playbook. When you need to conditionally run tasks, you can use `include_tasks`

```
- include_tasks: tasks.yml
  when: foo_file.stat.exists
```

**Note** that in this hypothetical example stat is a module that is used to retrieve file or file system status.

You could also include tasks with a `with_items` loop.

Handlers are also imported the same way as other tasks.

At the top level of your playbook, you can also import additional playbooks with the `import_playbook:` definition.

## Roles

Instead of importing playbooks, roles are a useful way to organize related configuration tasks. To create a role, you will need to create a `roles/` directory, and then a directory within `roles/` with you role name, and create two sub-directories called `meta/` and `tasks/`. The yaml files in these subdirectories should be called `main.yml`. If you do that Ansible will run all the tasks defined if your playbook has the following config:

```
---
- hosts: all
  roles:
    - foo
```

Where `foo` is the name of your role directory. Roles can be made more modula and flexible with the use of role vars.

In addition to the `meta/` and `tasks/` dirs, you can also include `files/` and `templates/` dirs for managing tasks. When copying files from your `files/` directory using the copy module, you only need to specify the filename in the `src` section of the task. The same logic applies to the template module for files in the `templates/` directory.

## Ansible Galaxy

Ansible Galaxy is a repo of Ansible roles and collections that you can include in your playbook. To add a role you can use a command like:

`ansible-galaxy role install {provider}.{role}`

Alternatively, you can  create a roles requirement file called `requirements.yml` to manage dependencies. It might look something like this:

```
---
roles:
  -  name:  {provider}.{role}
     version: {version number}

  - name: {provider}.{role}
    src: {http download path}
```

Then  you can install it via `ansible-galaxy install -r requirements.yml`
