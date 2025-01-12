# cisco-configuration-through-ansible 

Set up GNS3:

1. Install and configure the GNS3 virtual machine.
2. Add Cisco router images to GNS3.

3. Configure SSH on the remote router

Access the configuration mode:
```sh
conf t
```

Enable Domain Name:

```sh
ip domain-name example.com
```

Generate RSA Keys 🔑 :
```
crypto key generate rsa modulus 1024
```
Configure SSH Version:
```sh
ip ssh version 2
```
Configure VTY Lines for SSH Access:

```sh
line vty 0 4
transport input ssh
login local
exit
```
Create Local User with Privileges:

```sh
username admin privilege 15 secret yourpassword
```
Allow enable access to make configurations:

```sh
enable secret some_password
```

# Define the inventory file with the GNS3 routers.

Example Inventory File (/etc/ansible/hosts):

```sh
[example_routers]
192.168.0.13 ansible_user=miguel  ansible_password=cisco ansible_network_os=ios ansible_become_user=miguel
192.168.0.14 ansible_user=phil  ansible_password=cisco ansible_network_os=ios ansible_become_user=phil
192.168.0.15 ansible_user=michael  ansible_password=cisco ansible_network_os=ios ansible_become_user=michael
```

Example Playbook (cisco_config.yml):

```sh
- name: Configure Cisco Routers
  hosts: example_routers
  become: yes
  gather_facts: no
  connection: network_cli
  vars:
    ansible_become_method: enable
    ansible_become_password: passwordForEnablingConfiguration
  tasks:
    - name: Configure hostname
      ios_config:
        lines:
          - hostname some_hostname
```

4. Copy The SSH Key 🔑 To The Routers:

```sh
ssh-copyid hostwherethesshidwillbecopiedto@ip-address

**example**
ssh-copyid miguel@192.168.0.13
```

Running the Playbook:

```sh
ansible-playbook cisco_config.yml
```


