# cisco-configuration-through-ansible

Set up GNS3:

Install and configure the GNS3 virtual machine.
Add Cisco router images to GNS3.
Create an Ansible Playbook:

Configure SSH on the remote router

Enable Domain Name:

```sh
ip domain-name example.com
Generate RSA Keys:
```

```
crypto key generate rsa modulus 1024
Configure SSH Version:
```

```sh
ip ssh version 2
Configure VTY Lines for SSH Access:
```

```sh
line vty 0 4
transport input ssh
login local
exit
Create Local User with Privileges:
```

```sh
username admin privilege 15 secret yourpassword
```

Define the inventory file with the GNS3 routers.

Write a playbook to configure the routers.
Example Inventory File (inventory.yml):

```sh
all:
  hosts:
    router1:
      ansible_host: 192.168.56.101
    router2:
      ansible_host: 192.168.56.102

```
Example Playbook (cisco_config.yml):

```sh
- name: Configure Cisco Routers
  hosts: all
  gather_facts: no
  tasks:
    - name: Configure hostname
      ios_config:
        lines:
          - hostname {{ inventory_hostname }}
```

Running the Playbook:

```sh
ansible-playbook -i inventory.yml cisco_config.yml
```
