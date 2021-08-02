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
