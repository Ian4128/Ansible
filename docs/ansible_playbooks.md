## Creating an Ansible playbook
### Example playbook
```
#install_apache.yaml
- hosts: all
  become: true
  tasks:

  - name: install apache2 package
    apt:
      name: apache2
```
### Playbook explained
Indentation is very important in `.yaml.` files. every Tab needs to be 2 spaces.<br />
We'll divide the codeblock in two pieces:
<br /><br />
```
- hosts: all
  become: true
  tasks:
```

-   `hosts: all`: Run the playbook on all hosts.
-   `become: true`: the equivalent of `sudo` on your own machine.
-   `tasks:`: declare the tasks you want to run.
<br /><br />
```
  - name: install apache2 package
    apt:
      name: apache2
```

-   `name: <name>`: give a name to the task.
-   `apt:`: Give the module that needs to be used.
-   `name: apache2`: Name of the package you want to install. In this case it's `apache2`.
<br /><br />
### Run a playbook
**`ansible-playbook --ask-become-pass install_apache.yaml`**

-   `--ask-become-pass`: this parameter will ask you the sudo password of the remote machines.
-   `install_apache.yaml`: The name of the playbook you want to run.