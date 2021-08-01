# Mongod
## What is a daemon?
In multitasking computer operating systems, a daemon is a computer program that runs as a background process, rather than being under the direct control of an interactive user. Traditionally, the process names of a daemon end with the letter d, for clarification that the process is, in fact, a daemon, and for differentiation between a daemon and a normal computer program. For example, syslogd is the daemon that implements the system logging facility, and sshd is a daemon that serves incoming SSH connections. <br>
Ref: [orphan vs zombie vs daemon](https://www.gmarik.info/blog/2012/orphan-vs-zombie-vs-daemon-processes/)

## What is mongod?
mongod is the main daemon process for MongoDB. It is the core server of the database handling connection requests, and most important persisting our data. mongod contains all the configuration options you can use to make our database secure, distributed and consistant. <br>
A mongod deployment may consist of more than one server, for example, our data may be distributed in a replica set or across a sharded cluster. In this case, we run a separte mongod process for each server in our cluster. <br>
When we launch mongod, we are essentially starting up a new database.But we don't interact with the mongod directly. Instead, we use a database clinet that is programmed to communicate with mongod. We issue database commands, like document inserts or update to the client, and the client takes care of communicating with mongod to execute those commands. <br>
If our deployment have multiple servers, we can configure our client to communicate with each of these mongod processes as needed. So we don't need to figure out what server to connect to ourseleves.<br>

#### mongod Default Configuration
- port: 27017
- dbpath: /data/db
- bind_ip: localhost
- auth: disabled 
```shell
# Close mongod connection via mongo shell client
use admin
db.shutdownServer()
exit
```
## mongod Configuration YML file
Ref: [Mapping between mongod command line option and configuration file](https://docs.mongodb.com/manual/reference/configuration-file-settings-command-line-options-mapping/)

## mongod Options
- mongod --dbpath <directory path> # change the default path to given one
- mongod --port <port number> # default is 27017
- mongod --auth # to enable authentication
- mongod --bind_ip 123.123.123.123 # allow clients on IP address 123.123.123.123 to access database
- mongod --bind_ip localhost,123.123.123.123 # for multiple clients

## Profiling the database
#### db.<database_method>() # database methods
- db.createUser()
- db.dropUser()
- db.renameCollection()
- db.collection.drop()
#### rs.<replica_method>() # replica set methods
#### sh.<sharded_cluster_method>() # sharded cluster methods

### MongoDB profiler
Events captured by profiler:
- CRUD Operations
- Administrative operations
- Config operations
```json
db.getProfilingLevel()
db.setProfilingLevel(1)
# {"was": 0, "slowms": 100, "sampleRate": 1, "ok": 1}
```
  
## Basic Security
<b>Authentication Mechanisms</b>
1. SCRAM (Default)
2. X.509 (Community Version)
3. LDAP(Lightweight Directory Access Protocol) (Enterprise only)
4. KERBEROS (Enterprise only)

<b>Authorization Mechanisms</b>
- Role Based Access Control, role based based access
```json
# create root super user in admin db for authentication and authorization using SCRAM
use admin
db.createUser({
  user: "root",
  pwd: "root123",
  roles : [ "root" ]
})
```
Run the below command from mongo shell to authenticate 
```
mongo --username root --password root123 --authenticationDatabase admin
```

## Role Based Access Control System
1. <b>Custom Roles</b><br>
2. <b>Built-in Roles</b>

### Built-in Roles 
1. Database User :- read | readWrite
2. Database Administration :- dbAdmin, userAdmin, dbOwner
3. Cluster Adminstration :- clusterAdmin, clusterManager, clusterMonitor, hostManager
4. Backup/Restore
5. Super User :- root
Note: All roles applied on database level

## userAdmin, dbAdmin & dbOwner
<b>userAdmin Role</b>:- This role allows user to do all operations around managing a user.
- changeCustomData 
- changePassword
- createRole
- grantRole
- viewRole
- revokeRole
- dropRole
- createUser
- dropUser
- viewUser

<b>dbAdmin</b>
- collStats
- dbHash
- killCursors
- listIndexes
- listCollections
  
<b>dbOwner</b>:- The database owner can perform any administrative action on the database. This role combines the privileges granted by the readWrite, dbAdmin and userAdmin roles.
