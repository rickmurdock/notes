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

[MongoDB docs](https://docs.mongodb.com/)

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

```
db.restaurants.find()
```

#### FINDING ONE DOCUMENT

```
db.restaurants.findOne()
```

#### FILTERING RECORDS WHILE FINDING

Use a query filter document to filter records.

```
db.restaurants.find({name: "Wendy'S"})
db.restaurants.find({cuisine: "Chinese", borough: "Brooklyn"})
db.restaurants.find({cuisine: {$in: ["Chinese", "Thai", "Vietnamese"]}})
db.restaurants.find({cuisine: {$in: ["Thai", "Vietnamese"]}})
```

See [all the MongoDB query operators](https://docs.mongodb.com/manual/reference/operator/query/#query-selectors).

You can sort by calling `.sort` on the results with an object of fields to sort by:

```
db.restaurants.find({cuisine: {$in: ["Thai", "Vietnamese"]}}).sort({"name": 1})
```

`1` means to sort ascending, `-1` means to sort descending. The order of keys in the object is preserved, so you can specify multiple fields and it will sort in order.

```
db.restaurants.find({cuisine: {$in: ["Thai", "Vietnamese"]}}).sort({"borough": 1, "name": 1})
```

#### FILTERING USING NESTED DOCUMENTS

You can use dot notation to search inside nested documents.

```
db.restaurants.find({"address.zipcode": "11218"});
```

If you want to search for all documents based off an array value, you can reference the array like normal and you will get all records where any value in the array matches.

// Find all restaurants that have ever gotten a C score.

```
db.restaurants.find({"grades.grade": "C"})
```

To get records where all values in the array match, you have to get tricky. Here's one to get all restaurants that have only ever had "A" scores:

```
db.restaurants.find({"grades.grade": {$not: {$in: ["B", "C", "Z"]}}});
```




