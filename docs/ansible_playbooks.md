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
### Playbook explained
Indentation is very important in `.yaml.` files. every Tab needs to be 2 spaces.<br />
We'll divide the codeblock in two pieces:
<br /><br />
```
- hosts: all
  become: true
  tasks:state: latest
```

-   `hosts: all`: Run the playbook on all hosts.
-   `become: true`: the equivalent of `sudo` on your own machine.
-   `tasks:`: declare the tasks you want to run.
<br /><br />

```
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

### Run a playbook
**`ansible-playbook --ask-become-pass install_apache.yaml`**

-   `--ask-become-pass`: this parameter will ask you the sudo password of the remote machines.
-   `install_apache.yaml`: The name of the playbook you want to run.
<br /><br />

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
<br /><br />

### Remove package
-   `state: absent`: Will remove the given package(s) from all machines. In this case `apache2` will be removed