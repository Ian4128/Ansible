# Ansible docs

## Ansible introduction

Ansible provides open-source automation that reduces complexity and runs everywhere. Using Ansible lets you automate virtually any task. Here are some common use cases for Ansible:
-   Eliminate repetition and simplify workflows
-   Manage and maintain system configuration
-   Continuously deploy complex softwarePerform zero-downtime rolling updates

Ansible uses simple, human-readable scripts called playbooks to automate your tasks. You declare the desired state of a local or remote system in your playbook. Ansible ensures that the system remains in that state.
As automation technology, Ansible is designed around the following principles:

#### Agent-less architecture
-    Low maintenance overhead by avoiding the installation of additional software across IT infrastructure.
#### Simplicity
-    Automation playbooks use straightforward YAML syntax for code that reads like documentation. Ansible is also decentralized, using SSH with existing OS credentials to access to remote machines.
#### Scalability and flexibility
-    Easily and quickly scale the systems you automate through a modular design that supports a large range of operating systems, cloud platforms, and network devices.
#### Idempotence and predictability
-    When the system is in the state your playbook describes Ansible does not change anything, even if the playbook runs multiple times.
<br /><br />

## Start with Ansible



### check if ssh connection is possible to all machines in inventory file
    ansible all --key-file ~/.ssh/<ansible_keyname> -i inventory -m ping

-   `--key-file <path>`: ssh private_key file.
-   `-i <filename>`: inventory file with IP addresses of all machines.
-   `-m ping`: use ping module to make a connection with the machines.
<br /><br />

### ansible.cfg file
-   `ansible.cfg` is a file with all default settings.
#### example ansible.cfg file
```
[defaults]
inventory = inventory
private_key_file = ~/.ssh/ansible
```

There is also a `ansible.cfg` in `/etc/ansible` but the one in the git repo will override this one.<br />
specifying the `--key-file` and `-i` parameters is not necessary anymore because we already declared those in the `ansible.cfg` file.
<br /><br />
### List all hosts
-   `ansible all --list-hosts` will list all hosts in the `inventory` file.

### Gathering system information of all hosts
-   `ansible all -m gather_facts`: will gather all the facts about every machine in the `inventory` file.
-   `--limit <IP-address>` parameter will only list the facts about the specified machine.
<br /><br />

## Commands
### Example sudo apt-get update
    ansible all -m apt -a update_cache=true --become --ask-become-pass
-   `-m apt`: use `apt` module.*
-   `-a update_cache=true`: argument that needs to be used, in this case it's the `update` argument. This will run the equivalent of `apt-get update` before the operation
-   `--become`: the equivalent of `sudo` on your own machine.
-   `--ask-become-pass`: this parameter will ask you the sudo password of the remote machines.
<br /><br />

### Install a package on all machines
    ansible all -m apt -a name=vim-nox --become --ask-become-pass
-   `-m apt`: use `apt` module.*
-   `-a name=<package_name>`: Give the package name `apt` needs to install. 
-   `--become`: the equivalent of `sudo` on your own machine.
-   `--ask-become-pass`: this parameter will ask you the sudo password of the remote machines.
<br /><br />

### Update existing package
    ansible all -m apt -a "name=vim-nox state=latest" --become --ask-become-pass
-   `-m apt`: use `apt` module.*
-   `-a "name=<package_name> state=latest"`: Gives the package name and state `apt` needs to install. 
-   `--become`: the equivalent of `sudo` on your own machine.
-   `--ask-become-pass`: this parameter will ask you the sudo password of the remote machines.
<br /><br />

### Update all packages
    ansible all -m apt -a "upgrade=dist" --become --ask-become-pass
-   `-m apt`: use `apt` module.*
-   `-a "upgrade=dist"`: Tells `apt` to update all packages.
-   `--become`: the equivalent of `sudo` on your own machine.
-   `--ask-become-pass`: this parameter will ask you the sudo password of the remote machines.
