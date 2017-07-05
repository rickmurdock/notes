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
