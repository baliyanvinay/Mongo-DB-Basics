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

## What is the primary key in a document?
- "_id" is an unique identifier for a document in a collection.
- "_id" required in every MongoDB document
- ObjectId() is the default value for the "_id" field unless otherwise specified

## Explain Projection in MQL
Projection is expilicit mongo query to control the fields returned by the find() query.
0s and 1s can't be fixed in a single projection except when you want to exclude the "_id" field.
```json
# Syntax of projection
# 1 - include the field | 0 - exclude the field
db.collection.find({<filter_query>}, {<projection_query>})
```

## Useful MongoDB MSQL queries
- db.collection.findOne() # get a random document
- db.collection.insert({}) # insert a document
- db.collection.insert([ { "test": 1 }, { "test": 2 }, { "test": 3 } ]) # insert 3 documents
- db.collection.insert([{ "_id": 1, "test": 1 },{ "_id": 1, "test": 2 },{ "_id": 3, "test": 3 }],{ "ordered": false }) # to insert not in ordered fashion
- db.collection.updateMany({ "city": "HUDSON" }, { "$inc": { "pop": 10 } }) # increment pop by 10 for all cities named HUDSON
- db.collection.updateOne({ "zip": "12534" }, { "$set": { "population": 17630 } })
- db.collection.drop() # drop a collection
- db.collection.deletemany({"city":"ALABAMA"}) # delete all documents where city is ALABAMA
- db.collection.deteleone({"_id": 12}) # use id to delete to make sure only one document is removed
- db.collection.find({"age": {"$gte": 18} }) # get all documents where age is greater than or equals to 18
- db.collection.find({"tripduration": {"$lte": 70},"usertype": "Customer" })
- db.collection.find({"$and": [{"age": 12}, {"name": "John"}] })
- db.collection.find({"$not": {"name": "John"}}) # where name is not John
- db.collection.find({<array_field>: {"$size": <size_of_array>}}) # all documents where with exactly the given length.
- db.collection.find({<array_field>: {"$all": <array_in_square_brackets>}}) # all documents where array field contains all elements mentioned 
- db.collection.find({<array_field>: {"$elemMatch": {<field_in_array>: <value_to_match>} }}) # when to check elements of an array field

## Sample Queries
```json
db.companies.find(
{"$or": 
    [
	{"$and": [{"founded_year": 2004}, {"$or": [{"category_code": "web"}, {"category_code": "social"}] }]},
	{"$and": [{"founded_month": 10}, {"$or": [{"category_code": "web"}, {"category_code": "social"}] }]}	
    ]
}
).count()

# Different implementation same result
db.companies.find(
{"$and": 
    [
	{"$or": [{"founded_month": 2}, {"founded_year": 2004}]}, 
	{"$or": [{"category_code": "web"}, {"category_code": "social"}]}
    ]
}).count()
```
```json
# Where trip started and ended at same stattion and duration of trip is more than 1200 seconds
db.trips.find(
{"$expr": 
    {"$and": 
        [
         {"$gt": ["$tripduration", 1200]},
         {"$eq": ["$end station id", "$start station id"]}
        ]
    }
}
).count()
```
```json
# where twitter_username is same as permalink for a document
db.companies.find({"$expr": {"$eq": ["$twitter_username", "$permalink"]}}).count()
```
```json
# All Properties where amenities contains 20 items and does include atleast all the elements listed below
db.listingsAndReviews.find(
    { "amenities": 
	{
	    "$size": 20,
	    "$all": [ "Internet", "Wifi", "Kitchen",
                      "Washer", "Dryer", "Essentials",
                      "Hangers", "Hair dryer", "Iron"
                    ]
	}
    }
).pretty()
```
```json
# Review size is exactly 50 and acoomadates more than 6 people
db.listingsAndReviews.find(
    {"reviews" : {"$size": 50}, "accommodates": {"$gt": 6}}
).pretty()
```
```json
# Where Offices are located in Seattle City | Offices field is an array
db.companies.find({"offices": {"$elemMatch":{"city": "Seattle"}}}).count()
```
MongoDB Playground: https://mongoplayground.net/
