```
# mongoDB srv URI string
mongodb+srv://<user_name>:<user_password>@<host_name>/<authentication_database>
```

## MQL with normal queries and pipelines queries
#### Limiting data
```python
limited_cursor = movies.find(
	{"directors": "Sam Raimi"},
	{"_id": 0, "title": 1, "cast":1}
).limit(2)

pipeline = [
	{"$match": {"directors": "Sam Raimi"} },
	{"$project": {"_id": 0, "title": 1, "cast": 1} },
	{"$limit": 2}	
]
movies.aggregate(pipeline)
```
#### Sorting data
```python
from pymongo import DESCENDING, ASCENDING
sorted_cursor = movies.find(
	{"directors": "Sam Raimi"},
	{"_id": 0, "year": 1, "title": 1, "cast": 1}
).sort("year", ASCENDING)

pipeline = [
	{"$match": {"directors": "Sam Raimi"}},
	{"$project": {"_id": 0, "year": 1, "title": 1, "cast": 1} },
	{"$sort": {"year": ASCENDING}}
]
movies.aggregate(pipeline)
```
#### sorting with multiple values 
```python
# sort method takes a list of tuples 
sorted_cursor = movies.find(
	{"directors": "Sam Raimi"},
	{"_id": 0, "year": 1, "title": 1, "cast": 1}
).sort([("year": ASCENDING),("title": ASCENDING)])

pipeline = [
	{"$match": {"directors": "Sam Raimi"}},
	{"$project": {"_id": 0, "year": 1, "title": 1, "cast": 1} },
	{"$sort": {"year": ASCENDING, "title": ASCENDING}}
]
movies.aggregate(pipeline)
```
#### Counting result documents
```python
# count the number of movies directed by given director
pipeline = [
	{"$match": {"directors": "Sam Raimi"}},
	{"$project": {"_id": 0, "year": 1, "title": 1, "cast": 1} },
	{"$count": "num_movies"}
]
# Note: No cursor method is available | count depcieated in cursor methods
```

#### Skipping documents
```python
movies.find(
	{"directors": "Sam Raimi"},
	{"_id": 0, "title": 1, "cast": 1}
).sort("year", ASCENDING).skip(10) 

pipeline = [
	{"$match": {"directors": "Sam Raimi"}},
	{"$project": {"_id": 0, "year": 1, "title": 1, "cast": 1} },
	{"$sort": {"year": ASCENDING} },
	{"$skip": 10}
]
movies.aggregate(pipeline)
```
## Pagination using limit and skip
```python
db.movies.find().sort("year", ASCENDING).skip(page*20).limit(20)
```
