# Building an inventory
Inventories organize managed nodes in centralized files that provide Ansible with system information and network locations. Using an inventory file, Ansible can manage a large number of hosts with a single command.

## setting up an inventory file
this can be done using an `inventory.ini` or `inventory.yaml` file. `.yaml` files are better for maintaining larger inventories.

```
#inventory.ini

[myhosts]
127.0.0.1
```
<br />

#### Verify your inventory
    ansible-inventory -i inventory.ini --list


#### ping the `myhosts` group in your inventory
    ansible myhosts -m ping -i inventory.ini
<br />

### Inventory in INI or YAML format
Creating an inventory in `YAML` format becomes a sensible option as the number of managed nodes increases. For example, the following is an equivalent of the `inventory.ini` that declares unique names for managed nodes and uses the `ansible_host` field:


```
myhosts:
  hosts:
    my_host_01:
      ansible_host: 192.0.2.50
    my_host_02:
      ansible_host: 192.0.2.51
    my_host_03:
      ansible_host: 192.0.2.52
```
<br />

### Using metagroups
Create a metagroup that organizes multiple groups in your inventory with the following syntax:

```
metagroupname:
  children:
```
<br />

The following inventory illustrates a basic structure for a data center. This example inventory contains a `network` metagroup that includes all network devices and a `datacenter` metagroup that includes the `network` group and all webservers.
```
leafs:
  hosts:
    leaf01:
      ansible_host: 192.0.2.100
    leaf02:
      ansible_host: 192.0.2.110

spines:
  hosts:
    spine01:
      ansible_host: 192.0.2.120
    spine02:
      ansible_host: 192.0.2.130

network:
  children:
    leafs:
    spines:

webservers:
  hosts:
    webserver01:
      ansible_host: 192.0.2.140
    webserver02:
      ansible_host: 192.0.2.150

datacenter:
  children:
    network:
    webservers:
```
<br />

Variables set values for managed nodes, such as the IP address, FQDN, operating system, and SSH user, so you do not need to pass them when running Ansible commands.

#### Variables can apply to specific hosts.
```
webservers:
  hosts:
    webserver01:
      ansible_host: 192.0.2.140
      http_port: 80
    webserver02:
      ansible_host: 192.0.2.150
      http_port: 443
```
<br />

#### Variables can also apply to all hosts in a group.
```
webservers:
  hosts:
    webserver01:
      ansible_host: 192.0.2.140
      http_port: 80
    webserver02:
      ansible_host: 192.0.2.150
      http_port: 443
  vars:
    ansible_user: my_server_user
```