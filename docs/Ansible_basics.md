# Ansible docs

### check if ssh connection is possible to all machines in inventory file
**`ansible all --key-file ~/.ssh/<ansible_keyname> -i inventory -m ping`**

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
**`ansible all -m apt -a update_cache=true --become --ask-become-pass`**
-   `-m apt`: use `apt` module.*
-   `-a update_cache=true`: argument that needs to be used, in this case it's the `update` argument. This will run the equivalent of `apt-get update` before the operation
-   `--become`: the equivalent of `sudo` on your own machine.
-   `--ask-become-pass`: this parameter will ask you the sudo password of the remote machines.

### Install a package on all machines
**`ansible all -m apt -a name=vim-nox --become --ask-become-pass`**
-   `-m apt`: use `apt` module.*
-   `-a name=<package_name>`: Give the package name `apt` needs to install. 
-   `--become`: the equivalent of `sudo` on your own machine.
-   `--ask-become-pass`: this parameter will ask you the sudo password of the remote machines.

### Update existing package
**`ansible all -m apt -a "name=vim-nox state=latest" --become --ask-become-pass`**
-   `-m apt`: use `apt` module.*
-   `-a "name=<package_name> state=latest"`: Gives the package name and state `apt` needs to install. 
-   `--become`: the equivalent of `sudo` on your own machine.
-   `--ask-become-pass`: this parameter will ask you the sudo password of the remote machines.

### Update all packages
**`ansible all -m apt -a "upgrade=dist" --become --ask-become-pass`**
-   `-m apt`: use `apt` module.*
-   `-a "upgrade=dist"`: Tells `apt` to update all packages.
-   `--become`: the equivalent of `sudo` on your own machine.
-   `--ask-become-pass`: this parameter will ask you the sudo password of the remote machines.

## The `when` statement
you will only run the task if the statement after `when:` is true.

`when: ansible_distribution == "Ubuntu"`
-   `Ansible_distribution`: this is the variable that contains the distro name of the target machine.
-   It will only execute the task if the target machine's distro is `Ubuntu`
<br /><br />

### Multiple values in the shape of an array
`when: ansible_distribution == ["Debian", "Ubuntu"]`
-   `["Debian", "Ubuntu"]` is an array of strings. in this case these are the possible values `ansible_distribution`.
-   if the target machine's distro is `Debian` or `Ubuntu` then the task will run on that machine.
<br /><br />

### Adding multiple conditions with `and`
`when: ansible_distribution == ["Debian", "Ubuntu"] and ansible_distribution_version = "8.2"`
-   The statement above will check if the distro is `Debian` or `Ubuntu` **AND** if if the distro version is "8.2" with `ansible_distribution_version = "8.2"`.
-   Both conditions have to be true to run the task.