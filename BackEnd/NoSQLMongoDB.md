# Installing and setting up MongoDB

### Terminology

* *NoSQL*: a blanket name for databases that are not relational and do not use SQL

* *MongoDB*: a document database. Stores data in a JSON-like format. Uses JavaScript for queries.

### Examples

This example shows how to connect to MongoDB, create a database, and delete it.

```
$ mongo
MongoDB shell version v3.4.4
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.4.4
> use testdb
switched to db testdb
> db
testdb
> db.dropDatabase()
{ "ok" : 1 }
```

### References

* [MongoDB docs](https://docs.mongodb.com/)

---

# MongoDB databases, collections, and documents

### Terminology

* *database*: (in MongoDB) a storage area for collections

* *collection*: a named set of documents, analogous to a table in a relational database

* *document*: a JSON-like object with keys and values, analogous to a row in a relational database

### How does MongoDB differ from PostgreSQL?

* There is no schema. You can store freeform data in the database.

* There is no easy way to join data from multiple collections, unlike in a relational database, where you can join multiple tables.

### Can I really store anything in MongoDB?

Not exactly. There is a [formal list of what you can store](https://docs.mongodb.com/manual/reference/bson-types/), but for all purposes, you can store whatever you could use in a JSON object.

---

# Importing and exporting data from MongoDB

### Vocabulary

* *JSON*: JavaScript Object Notation. A restricted version of object literals and JavaScript. Used for MongoDB exports.

* *BSON*: [Binary JSON](https://en.wikipedia.org/wiki/BSON), the format used by MongoDB internally. Can store more types than JSON.

### Importing and exporting from MongoDB

```
$ mongoimport --db databaseName --collection collectionName --file inputFile.json
2017-06-28T23:34:57.090-0400    connected to: localhost
2017-06-28T23:34:58.550-0400    imported 25359 documents

$ mongoexport --db databaseName --collection collectionName --out outputFile.json
2017-06-28T23:35:53.000-0400    connected to: localhost
2017-06-28T23:35:54.001-0400    [........................]  newdb.restaurants  0/25359  (0.0%)
2017-06-28T23:35:55.004-0400    [###############.........]  newdb.restaurants  16000/25359  (63.1%)
2017-06-28T23:35:55.452-0400    [########################]  newdb.restaurants  25359/25359  (100.0%)
2017-06-28T23:35:55.452-0400    exported 25359 records
```

---

# MongoDB Operations

### Terminology

### Examples

All the below examples were written using the provided sample database. To import the database, run:

```
curl -o primer-dataset.json https://raw.githubusercontent.com/mongodb/docs-assets/primer-dataset/primer-dataset.json
mongoimport --db newdb --collection restaurants --file primer-dataset.json
```

#### SETTING THE DEFAULT NUMBER OF RECORDS TO SHOW AT ONCE

```
> DBQuery.shellBatchSize = 4
```

#### FINDING ALL DOCUMENTS

```javascript
db.restaurants.find()
```

#### FINDING ONE DOCUMENT

```javascript
db.restaurants.findOne()
```

#### FILTERING RECORDS WHILE FINDING

Use a query filter document to filter records.

```javascript
db.restaurants.find({name: "Wendy'S"})
db.restaurants.find({cuisine: "Chinese", borough: "Brooklyn"})
db.restaurants.find({cuisine: {$in: ["Chinese", "Thai", "Vietnamese"]}})
db.restaurants.find({cuisine: {$in: ["Thai", "Vietnamese"]}})
```

See [all the MongoDB query operators](https://docs.mongodb.com/manual/reference/operator/query/#query-selectors).

You can sort by calling `.sort` on the results with an object of fields to sort by:

```javascript
db.restaurants.find({cuisine: {$in: ["Thai", "Vietnamese"]}}).sort({"name": 1})
```

`1` means to sort ascending, `-1` means to sort descending. The order of keys in the object is preserved, so you can specify multiple fields and it will sort in order.

```javascript
db.restaurants.find({cuisine: {$in: ["Thai", "Vietnamese"]}}).sort({"borough": 1, "name": 1})
```

#### FILTERING USING NESTED DOCUMENTS

You can use dot notation to search inside nested documents.

```javascript
db.restaurants.find({"address.zipcode": "11218"});
```

If you want to search for all documents based off an array value, you can reference the array like normal and you will get all records where any value in the array matches.

// Find all restaurants that have ever gotten a C score.

```javascript
db.restaurants.find({"grades.grade": "C"})
```

To get records where all values in the array match, you have to get tricky. Here's one to get all restaurants that have only ever had "A" scores:

```javascript
db.restaurants.find({"grades.grade": {$not: {$in: ["B", "C", "Z"]}}});
```

To understand how this works, step through it:

1. `$in` matches values that are in the array `["B", "C", "Z"]`. This will bring back records where any `grade` in the array `grades` returns true for this test.

2. This brings back all records where the restaurant has ever gotten a "B", "C", or "Z" (the only grades outside of "A" I saw.)

3. `$not` gives us the opposite of that -- all records that the $in didn't match.

4. So, we get all records where no `grades.grade` was "B", "C", or "Z".

You can simplify the above a little:

```javascript
db.restaurants.find({"grades.grade": {$nin: ["B", "C", "Z"]}});
```

You can reference specific elements of an array using dot notation. To find all restaurants where their last grade was an "A" (assuming that the grades are in descending order by date):

```javascript
db.restaurants.find({"grades.0.grade": "A"});
```

### Inserting documents  

When you insert a document, it will be given a unique _id unless you provide one.

```javascript
// This will insert a new document. The result object contains two values,
// `acknowledged` and `insertedId`. `insertedId` lets us look up the document
// we inserted.
var result = db.restaurants.insertOne({
  "address": {"building": "100", "street": "Fiction St", "zipcode": "00001" },
  "borough": "Yonkers", "cuisine": "Awesome",
  "grades": [
    { "date": ISODate("2017-06-01T00:00:00Z"), "grade": "A+", "score": 0 }
  ],
  "name": "Favorite Delights", "restaurant_id": "1"})
db.restaurants.findOne({"_id": result.insertedId})
```

`insertMany` can insert more than one document at a time:

```javascript
db.restaurants.insertMany([{
  "address": {"building": "100", "street": "Fiction St", "zipcode": "00001" },
  "borough": "Yonkers",
  "cuisine": "Awesome",
  "grades": [
    { "date": ISODate("2017-06-01T00:00:00Z"), "grade": "A+", "score": 0 }
  ],
  "name": "Favorite Delights",
  "restaurant_id": "1"
}, {
  "address": {"building": "101", "street": "Fiction St", "zipcode": "00001" },
  "borough": "Yonkers",
  "cuisine": "Garbage",
  "grades": [
    { "date": ISODate("2017-06-01T00:00:00Z"), "grade": "C", "score": 50 }
  ],
  "name": "Garbage Delights",
  "restaurant_id": "2"}])
```

### Creating a unique index  

You will have fields in your documents that you want to ensure are unique. To do this, you need to [create a unique index](https://docs.mongodb.com/manual/core/index-unique/#index-type-unique).

```javascript
// Ensure restaurant_id is unique.
db.restaurants.createIndex( { "restaurant_id": 1 }, { unique: true } )
```

You can see your collection's indexes like so:

```javascript
db.restaurants.getIndexes()
```

### Updating documents  

There are two main functions to update documents, `updateOne` and `updateMany`. Each of these take a filter -- like we used with `find` -- and an object made of [update operators](https://docs.mongodb.com/manual/reference/operator/update/). These operators tell us how to manipulate the document.

Some examples:

```javascript
// Add city and state to all addresses
db.restaurants.updateMany({},
  {$set: {"address.city": "New York", "address.state": "NY"}});

// Add a new review to one restaurant
// We can use new Date() because the Mongo shell uses JavaScript.
db.restaurants.updateOne({restaurant_id: "30191841"},
  {$push: {grades: {"grade": "A", "score": 7, "date": new Date()}}});

// Add a new review to one restaurant and keep them in order
db.restaurants.updateOne({restaurant_id: "30191841"},
  {$push: {grades: {
    $each: [{"grade": "A", "score": 7, "date": new Date()}],
    $sort: {"date": -1}}}})

// Fix an error in data entry across multiple documents
db.restaurants.updateMany({"address.street": "West   57 Street"},
  {$set: {"address.street": "West 57 Street"}})
```

You can also upsert documents. Upsert means to update if records are found, or insert a document if they are not found. Add a third object of options with `upsert` equal to true to do so.

```javascript
db.restaurants.updateOne(
  {restaurant_id: "99"},
  {
    $set: {name: "Spaniel's Place"},
    $push: {grades: {"grade": "A", "score": 7, "date": new Date()}}
  },
  {upsert: true}
);
```

Your search parameters must be unique for an upsert to work.

### Deleting records  

To delete records, use `deleteOne` or `deleteMany`. These take a query like `find` and `findOne`.

```javascript
// Delete restaurant id 99
db.restaurants.deleteOne({restaurant_id: "99"})

// Delete all restaurants in Manhattan which is not a real borough
db.restaurants.deleteMany({"borough": "Manhattan"})

// Delete all restaurants
db.restaurants.deleteMany({})
```

### Using MongoDB from Node  

All the commands we have run so far run inside the MongoDB shell, but you can run them from Node, too. To do so, install the Node MongoDB library:

```
npm install mongodb --save
```

The majority of method names are the same, but instead of directly returning data, they take callbacks, or if you don't provide a callback, return promises. See [the Node.js MongoDB quickstart](http://mongodb.github.io/node-mongodb-native/2.2/quick-start/quick-start/) to see it in action.

### References  

* [MongoDB shell quickstart](https://docs.mongodb.com/getting-started/shell/)
* [Node.js MongoDB quickstart](http://mongodb.github.io/node-mongodb-native/2.2/quick-start/quick-start/)
* [Query operators](https://docs.mongodb.com/manual/reference/operator/query/#query-selectors)
* [Update operators](https://docs.mongodb.com/manual/reference/operator/update/)


