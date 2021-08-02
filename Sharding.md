## Database Scaling
<b>Vertical Scaling</b>: Upgrade to RAM, disk space and CPU to handle increased data needs. This could be very expensive as hardware will come to a limit. <br>
<b>Horizontal Scaling</b>: More machines are added in this case where each machine can be used to store data.

## Sharding
In sharding, data is divided into many pieces and can be stored at as many as servers one like. Together all these shards make up sharded cluster. <br>
Each shard is deployed as replica set in order to grauntee high availablity. This enables fault tolenance for each piece of data(i.e., shard)<br>
<b>Note</b>: MongoS routes the queries/data to the correct shard using a config server. To maintain availablity of this server, replication is used. 
