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

