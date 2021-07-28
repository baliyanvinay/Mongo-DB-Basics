## What is a MongoDB database?
MongoDB is a NoSQL document database which prefer not using the traditional approach of storing data into tables across rows and columns. MangoDB stores data in BSON format which is binary format of JSON to increase speed, space and flexiblity. BSON also added some non-native JSON data types like dates. long, floats. <br>
NoSQL databases can be
- key:value pair database like Redis and DynamoDB 
- Graph database like Cassandra and neo4j
- Column oriented database like MariaDB
- Document based databased like MongoDB

## Basic MongoDB Terms in comparison to RDBS
| Term | Related SQL Term | Description |
| :-------------:|:-------------:| :-------------:| 
| Database | Database ||
| Collection | Table | A collection would contain many documents |
| Document | Row | A structure to organize & store data in field value pair |
| Field | Column | a unique identifier for a datapoint |

## Other MongoDB Terms
Cluster - group of servers that store your data. <br>
Replica Set - a few connected machines that store the same data to ensure that if something happens to one of the machines the data will remain intact. Comes from the word replicate - to copy something. <br>
Instance - a single machine locally or in the cloud, running a certain software, in our case it is the MongoDB database. <br>

## What is MongoDB Atlas?
Atlas is a database as a service offered by MongoDB which provides cloud database storage solution.<br>
Note: MongoDB provides a free Atlas cluster with 3 server replica set with 512MB of storage that never expire.

## Importing and Exporting data in MongoDB
### JSON Import/Export
- mongoimport: mongoimport --uri "<Atlas_Cluster_URI>" --drop=<json_file>.json
- mongoexport: mongoexport --uri "<Atlas_Cluster_URI>" --collection=<collection_name> --out=<json_file>.json

### BSON Import/Export
- mongorestore: mongorestore --uri "<Atlas_Cluster_URI>" --drop <BSON_dump>
- mongodump: mongodump --uri "<Atlas_Cluster_URI>"

MongoDB Playground: https://mongoplayground.net/
