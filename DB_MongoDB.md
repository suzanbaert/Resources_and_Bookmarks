# Mongo DB

Course: [Mongo DB - M001 Basics](https://university.mongodb.com/courses/M001/about)


# General

+ Access via Shell or through Compass (GUI)
+ Atlas: the database as a service from mongodb
+ Structure: cluster > databases > collections > documents
  + Collections: group of records of very similar items
  + Database: group of collections for a specific app, or on certain topic
  + Namespace via dots: database.collection


+ Documents are JSON based
+ Add a path to mongo shell inside System environment variables


# Queries

### General

+ `show dbs`: show databases inside cluster
+ `use DBName`: go to a database
+ `show collections`: show all collections inside this database
+ `db.CollName.findOne()`: gives one document
+ `db.CollName.find({}{field:1, _id:0})`: looking at one field across documents



### Filtering data

+ `db.CollName.find({key: "value"})`
+ `db.CollName.find({key: "value", key2: "value2"})`: default AND operation, can cause issues when looking at same fields
+ `db.CollName.find({"parentkey.childkey": "value"})`: for nested fields use . and wrap inside quotes.

**Operators:**

+ `db.CollName.find({key: {$operator: $condition}})`: general syntax for operators
+ `db.CollName.find({key: {$gt: 7, $lt: 10}})`: greater than 7, less than 10


+ $gt: greater than
+ $gte: graeter than or equal
+ $lt, $lte: less than (or equal)
+ $ne: != (!! also returns documents that do not have this field)
+ $in: %in%
+ $nin: !%in%
+ $regex: adding regex inside `/str.*/`

+ $exists: whether field exists or not (but could be null)
+ {field: null}: finds if field is null or does not exist

+ $or: add an array of conditions
+ $and: add an array of conditions




**Quering arrays**
+ `db.CollName.find({key.0 : value})`: finds documents where the first element of an array is value (zero-based index).
+ operator `$all`: returns those that contain all mentioned values in its array
+ operator `$size`: returns the length of the array
+ operator `$elemMatch`: for arrays that have nested objects, find an element inside the array where something is true



### Query additions
+ `db.CollName.count({key : "value"})`: returns the number of observations
+ `db.CollName.find({key : "value"}).count()`: alternative to count
+ `db.CollName.find({key : "value"}).pretty()`: prettified JSON



### Projections

To return not the full document but only certain fields.

+ `db.CollName.find({key : "value"}, {title:1})`: includes title and document_id.
+ `db.CollName.find({key : "value"}, {title:1, _id:0})`: includes only title
+ `db.CollName.find({key : "value"}, {title:0})`: shows all fields except title.


### Creating new records

+ `db.CollName.insertOne(<json object>)`: inserts a new json document
+ `db.CollName.insertMany(<json array>)`: inserts an array of json documents

By default: ordered insert. Inserts and stops when encountering an error (so partial upload is possible).
To change to unordered insert (keep going even after an error):
+ `db.CollName.insertMany(<JSON array>, {ordered:false})`;



### Updating records

+ `db.CollName.updateOne({title: "x"}, {$set: {key: "value"}})`: updates the first value where title is x, and sets key to value. If not present it will add it, otherwise replace.

+ `db.CollName.updateMany({}, {})`: update all records where first item is true

Operators:
+ $set: insert field or replace value
+ $unset: remove
+ $inc: increment with a certain value

All update operators: [see documentation](https://docs.mongodb.com/manual/reference/operator/update/)


### Upserts:

If present, update. If not present insert.

`db.collection.updateOne({<condition>}, {$set: {}}, {upsert:true});`



### Deletes:

+ `db.collection.deleteOne({})`
+ `db.collection.deleteMany({})`



### Javascript

The mongo shell can execute Javascript --> can temporarily hold something in memory

```
tempDoc = db.collection.findOne({});
tempDoc.key
```
