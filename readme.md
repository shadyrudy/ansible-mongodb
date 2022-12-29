# Install Percona MongoDB using Ansible Playbooks on Ubuntu
The following project installs the Percona distribution of MongoDB on Ubuntu using Ansible.

# Goals
The goals of this project are the following:
1. Install required packages
1. Create config files and directories
1. Install Percona's MongoDB database software
1. Start processes
1. Create the replica sets
1. Create user accounts
1. Install and start the backup process
   1. Backups managed by Percona Backup for MongoDB 

# Future Goals
1. Create a main.yml file that runs all of the playbooks in order
1. Backups via Percona Backup for MongoDB
   1. Fix permissions on the backup directory
   1. Create backup group with access to the backup directory
   1. Start the backup service as mongod
   1. Schedule backups via Cron job
1. Configure monitoring solution
1. Configure sharding

## ToDos
1. Use variables correctly for passwords
   1. Currently, the scripts use ABCABCABC as a password place holder.
   1. Use either Ansible Vault, Variable file, Hashicorp, etc  
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
### Replica Set with an Arbiter 
One of the playbooks intilizes a replica set with an Arbtier. Currently, the template looks for a specific name pattern, namely 'arb'. Hosts matching this name will be set up as an arbiter.
## ansible.conf
Ensure that your ansible.conf has the at least the following items:
1. A default inventory file location
    1. inventory=/etc/ansible/hosts 
1. become_user = {username} 
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
1. Initial commit. 
1. Added all files
1. Excluded secrets and made a generic inventory file.

## 2022-07-11
1. Added file checker to check permissions
1. Added file fixer to fix permissions
1. Added three versoins of mongod.conf
1. Modified keyfile to copy 1 version of mongod.conf
1. Modified create replicate set playbook
1. Modified percona install playbook
1. Modified software install playbook
1. Updated restart mongod script
1. Added second inventory file
1. Updated replica set init script

## 2022-07-25
1. Added install playbooks for Percona Backup for MongoDB
1. Added playbook to create PBM user and Role inside of MongoDB
1. Creates A PBM role in the MongoDB database Jinja2 template
1. Creates the PBM user in the MongoDB database Jinja2 template
1. Create the pbm-agent file via Jinja2 template
1. Create the pbm-agent.service file via Jinja2 template
1. Create the pbm_config.yaml file via Jinja2 template

## 2022-12-29
1. Updated remaining playbooks from remote_user to become_user
1. Updated replica set init script and template to include replica set name
1. Created replica set init script and template with an arbiter
1. Created script to restart MongoDB with authorization using shutdownServer()