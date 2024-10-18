## Creating an Ansible playbook
### Example playbook
```
#install_apache.yaml

- hosts: all
  become: true
  tasks:

  - name: update repository index
    apt:
      update_cache: yes

  - name: install apache2 package
    apt:
      name: apache2
      state: latest
```
<br />

### Playbook explained
Indentation is very important in `.yaml.` files. every Tab needs to be 2 spaces.<br />
We'll divide the codeblock in two pieces:
<br /><br />
```
#install_apache.yaml

- hosts: all
  become: true
  tasks:state: latest
```

-   `hosts: all`: Run the playbook on all hosts.
-   `become: true`: the equivalent of `sudo` on your own machine.
-   `tasks:`: declare the tasks you want to run.
<br /><br />

```
#install_apache.yaml

  - name: update repository index
    apt:
      update_cache: yes

  - name: install apache2 package
    apt:
      name: apache2
      state: latest
```

-   `name: <name>`: give a name to the task.
-   `apt:`: Give the module that needs to be used.
-   `update_cache: yes`: execute the equivalent of `sudo apt-get update`
-   `name: apache2`: Name of the package you want to install. In this case it's `apache2`.
-   `state: latest`: This will make sure that everytime the playbook runs the latest package will be installed.
<br /><br />

**You can add as much tasks as you like!**
<br /><br />

### Adding multiple packages to install in one task
```
#install_apache.yaml

      name:
        - apache2
        - libapache-mod-php
```

-   `name:` stays on the same location
-   packages can be added by typing them each on a separate line starting with `-`

<br /><br />

### Run a playbook
**`ansible-playbook --ask-become-pass install_apache.yaml`**

-   `--ask-become-pass`: this parameter will ask you the sudo password of the remote machines.
-   `install_apache.yaml`: The name of the playbook you want to run.
<br /><br /><br />

## Removing packages
### Example playbook
```
#remove_apache.yaml

---

- hosts: all
  become: true
  tasks:

  - name: install apache2 package
    apt:
      name: apache2
      state: absent
```
<br />

### Remove package
-   `state: absent`: Will remove the given package(s) from all machines. In this case `apache2` will be removed
<br /><br />

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