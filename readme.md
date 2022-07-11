# Install Percona MongoDB using Ansible Playbooks on Ubuntu
The following project installs the Percona distribution of MongoDB on Ubuntu using Ansible.

# Goals
The goals of this project are the following:
1. Install required packages
1. Create config files and directories
1. Start processes
1. Create the replica sets
1. Create user accounts

# Future Goals
1. Configure backup solution
1. Configure monitoring solution
1. Configure sharding

## ToDos
1. Update the install playbook to use proper package management instead of wget
1. Update the restart playbook to use the Ansible service module
1. Fix the official MongoDB install playbook, which is currently failing. 

# Calling the Ansible Playbooks
You can call the playbooks using the following syntax
```bash
ansible-playbook -i inventory.txt -c ssh software.yml
```
The parameters are the following:
1. -i inventory.txt
    1. The hosts where the playbook will execute
1. -c ssh
    1. Use SSH to connect to the targeted hosts
    1. Specify keyfile locations in /etc/ansible/ansible.cfg
1. software.yml
    1. The requested playbook       

## inventory.txt
Ensure that your servers are correct in the inventory file.
These server names are used in the **hosts** section of the playbooks. The default inventory list is specified in the ansible.conf file 
Or, you can specify the inventory file using the **-i** option in the command line. 
### Python install location - inventory.txt
You can specify the Pyton install location if the playbooks complain. Simply update server name entries in the **inventory.txt** file with the following information
```bash
[dbservers]
10.1.1.1 ansible_python_interpreter=/usr/bin/python3
```

## ansible.conf
Ensure that your ansible.conf has the at least the following items:
1. A default inventory file location
    1. inventory=/etc/ansible/hosts 
1. remote_user = {username} 
1. SSH key location
    1. private_key_file=/home/ubuntu/documents/mykey.pem

**Note:** You can specify any of the above using different command line options, such as **-i** for to specify a different inventory file. 

The ansible.conf file is usually located in the directory /etc/ansible/

# Project Requirements
To run the playbooks, you'll need to install the following packages.

1. Ansible
1. Python3
1. Pymongo - via PIP
1. MongoDB Ansible Gallery Package
   1. https://galaxy.ansible.com/community/mongodb

# Version History
## 2022-07-07
Initial commit. 
Added all files
Excluded secrets and made a generic inventory file.

## 2022-07-11
Added file checker to check permissions
Added file fixer to fix permissions
Added three versoins of mongod.conf
Modified keyfile to copy 1 version of mongod.conf
Modified create replicate set playbook
Modified percona install playbook
Modified software install playbook
Updated restart mongod script
Added second inventory file
Updated replica set init script