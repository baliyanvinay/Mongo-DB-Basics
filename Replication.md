## What is Replication?
Replication is the concept of maintaining multiple copies of data. This is to make sure data is available in events of downtime & failures. <br>
In mongoDB, a replica set defines this functionality with one primary node and other nodes as secondary node. Each node remains consistent with each. In case of primary node going down, one of secondary node gets elevated to primary node. This is decided based on a vote amoung nodes.  

## Replica Sets
Replica sets are group of mongods that shares copies of same data between them. Everytime client communicates with a replica set, operation is performed with primary node and then synced with secondary nodes.

![Replica Set](https://docs.mongodb.com/manual/images/replica-set-read-write-operations-primary.bakedsvg.svg)

Replica set can reach upto 50 nodes, but only 7 nodes can take part in election to primary node selection. 
### Delayed Node in Replica Set
Delayed Nodes have delay in their replication process is to allow resilence in data. For example, if the delay is 1 hour, then any error drop collection operation can be reverted from delayed node within 1 hour window.

## Replica Set setup
To create 3 node replica set, below steps could be followed:- 
1. create three mongod configuration files with same keyFile and replicaSetName and different ports. 
2. start a mongod process with one of the conf files ``` mongod -f mongod_01.conf ```
3. enter db using mongo shell ``` mongo --port 270001 ```
4. initiate replicate set ``` rs.initiate() ``` # this will make the current db as PRIMARY node
5. add an admin user to handle replication operations in the admin db ``` use admin ```
6. quit() the mongo shell and start other mongod processes
7. utilize the newly created admin user to enter primary db again ``` mongo admin -u admin -p password --port 270001 ```
8. add other nodes using ``` rs.add({ host: "<host_ip: port>" }) ``` as SECONDARY nodes
9. check status of replica set and quit ``` rs.status ```

## Replication Configuration
- JSON object that defines the configuration options of our replica set
- can be configured manually from the shell
- there are set of mongo shell replication helper methods that make it easier to manage
```json
{
	"_id": "replica_set_name",
	"version": 12,
	"members": [
		{
			"_id": "0812301",
			"host": "127.0.0.1:270001",
			"arbiteronly": false,
			"hidden": false, 
			"priority": 1, 
			"slaveDelay": 3000, 
		},
	],
}
```
Note: priority must be 0 when hidden is true |  1000<=priority>=0 | 0 means node can't be primary ever

## Replica Set Commands
- Replica status ```rs.status() ```
- Role of a node ``` rs.isMaster() ```
- Info about mongo server ``` db.serverStatus()['repl'] ```
- Operation log ``` rs.printReplicationInfo() ```

```
# Connecting to a replica set
mongo --host replica_Set_name/localhost:27001 -u admin -p pass
```
