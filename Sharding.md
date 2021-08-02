## Database Scaling
<b>Vertical Scaling</b>: Upgrade to RAM, disk space and CPU to handle increased data needs. This could be very expensive as hardware will come to a limit. <br>
<b>Horizontal Scaling</b>: More machines are added in this case where each machine can be used to store data.

## Sharding
In sharding, data is divided into many pieces and can be stored at as many as servers one like. Together all these shards make up sharded cluster. <br>
Each shard is deployed as replica set in order to grauntee high availablity. This enables fault tolenance for each piece of data(i.e., shard)<br>
<b>Note</b>: MongoS routes the queries/data to the correct shard using a config server. To maintain availablity of this server, replication is used. 

## When to shard
- Check if it is economically viable to scale up, when you hit a dead end with vertical scaling and the performance is not as much as costs involved.  
- Horizontal scaling also provide parallel performance simultaneously for smooth operations.
- Increasing the size of your disks most likely will imply eventual increasing the size of your RAM, which brings added costs or other bottlenecks to your system.<br>In such a scenario sharding, the parallelization of your workload across shards, might be way more beneficial for your application and budget than the waterfall of potential expensive upgrades.<br>
- A general rule of thumb indicates that individual servers should contain 2-5TB of data.
