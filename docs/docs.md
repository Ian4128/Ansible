# Ansible docs

### check if ssh connection is possible to all machines in inventory file
`ansible all --key-file ~/.ssh/<ansible_keyname> -i inventory -m ping`

-   `--key-file <path>`: ssh private_key file.
-   `-i <filename>`: inventory file with IP addresses of all machines.
-   `-m ping`: use ping module to make a connection with the machines.

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

### List all hosts
-   `ansible all --list-hosts` will list all hosts in the `inventory` file.

### Gathering system information of all hosts
-   `ansible all -m gather_facts`: will gather all the facts about every machine in the `inventory` file.
-   `--limit <IP-address>` parameter will only list the facts about the specified machine.