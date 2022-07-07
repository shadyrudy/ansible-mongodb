# MongoDB Keyfile 
MongoDB uses a keyfile for replica set access control. You must generate a keyfile and place the keyfile in the files directory

# Create the keyfile and permission
We will first create the file mongodb-keyfile and place this file somewhere in the ansible controller, inside the /files folder. One play will transfer the file to each of the hosts. Another play will update /etc/mongod.conf with the file name.

## Creation Script
You will create the MongoDB keyfile using the following command:

```bash
openssl rand -base64 756 > <path-to-keyfile>/mongodb-keyfile
```
## Setting file permissions
Once created, the keyfile will reside on all members of the replica set at the following location:
1. /etc/mongodb-keyfile
1. Owner and group will be mongod
1. Permissions will be 400

## Updating /etc/mongod.conf
The file mongod.conf configures all of the MongoDB settings, including replication. Another play will update the mongod.conf file with the replica set name and enable keyfile authentication.

```yml
...
replication:
  replSetName: "rs0"
...
...  
security:
  keyFile: /etc/mongodb-keyfile
...
```

