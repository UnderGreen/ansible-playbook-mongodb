###Testing repo for [greendayonfire.mongodb](https://github.com/UnderGreen/ansible-role-mongodb) Ansible role.

An example how to use my role.  
It's included replica set configuration and one simple node without authorization.  
Execute **vagrant up --provider=virtualbox**. It will install 4 nodes and you can see my role in action.

**mongodb1** - mongodb master node (it's will be master after init)  
**mongodb2** - replica set member  
**arbiter** - replica set member with arbiter role  
**mongodb-wa** - node with mongodb without auth and replication
