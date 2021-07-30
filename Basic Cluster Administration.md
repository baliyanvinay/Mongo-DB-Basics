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
