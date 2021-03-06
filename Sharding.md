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

## What is mongoS?
The mongos tracks what data is on which shard by caching the metadata from the config servers. The mongos uses the metadata to route operations from applications and clients to the mongod instances. <br>
A mongos has no persistent state and consumes minimal system resources.Applications never connect or communicate directly with the shards.<br>
The mongos merges the data from each of the targeted shards and returns the result document. Certain query modifiers, such as sorting, are performed on each shard before mongos retrieves the results.

## Sharding Architecture
![Sharding Architecture](https://docs.mongodb.com/manual/images/sharded-cluster-production-architecture.bakedsvg.svg)

```js
/* How to add a shard to mongos? */
sh.addShard("shard1/localhost:27001")
sh.status()
```
## Shard a collection
1. Enter mongos with admin credentials
2. Analyze your collection ``` db.my_collection.findOne() ```
3. Create index for your shard keys ``` db.products.createIndex( { "sku": 1, "name": 1 } ) ```
4. Enable sharding for your database ``` sh.enableSharding("my_fav_db") ```
5. Shard collection on indexed keys ``` sh.shardCollection("my_fav_db.products", { sku: 1, name: 1 } ) ```
