# SQL: Introduction

## Installing and setting up Postgres

### Terminology

* *PostgreSQL*: a database management system. Also known sometimes as PostgreSQL.
* *background process*: a program that runs in the background. Can be managed with `brew services`.

### Examples

The following example demonstrates how to see which services are running.

```
$ brew services list
Name       Status  User       Plist
postgresql started [username] /Users/[username]/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
```

This example shows how to create a database, connect to it, and delete it.

```
$ createdb testdb
$ psql testdb
psql (9.6.3)
Type "help" for help.

testdb=# help
You are using psql, the command-line interface to PostgreSQL.
Type:  \copyright for distribution terms
       \h for help with SQL commands
       \? for help with psql commands
       \g or terminate with semicolon to execute query
       \q to quit
testdb=# \q
$ dropdb testdb
```

### References
* [brew services](https://github.com/Homebrew/homebrew-services)
* [https://www.postgresql.org/](https://www.postgresql.org/)

---

## Databases, tables, rows, and columns

### Terminology

* *database*: a storage area for tables and all other data associated with those tables. "Database" is also often used to refer to a database management system like PostgreSQL.
* *table*: a named collection of records, all with the same structure.
* *row*: a record in a table.
* *column*: a field in a table that all records will have. Columns have a name and a type.
* *primary key*: a unique id that each row in a table has. This will usually be an auto-incrementing number called `id`, although that is not a requirement of the database.
### Examples

This example shows the syntax for `CREATE TABLE`.

```
CREATE TABLE students (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(100) NULL UNIQUE,
  favorite_candy VARCHAR(100) NULL,
  graduated BOOLEAN NOT NULL DEFAULT 'f',
  cohort INTEGER NOT NULL
);
```

This example shows the syntax for `INSERT INTO`.

```
INSERT INTO students (name, favorite_candy, graduated, cohort) VALUES ('Charlie', 'Skittles', 't', 1);
INSERT INTO students (name, email, cohort) VALUES ('Harper', 'harper@example.org', 12);
INSERT INTO students (name, favorite_candy, cohort) VALUES
('Kelly', 'Milky Way', 12),
('Alexis', 'Hot Tamales', 12);
```

### References

* `CREATE TABLE` [documentation](https://www.postgresql.org/docs/current/static/sql-createtable.html)

* [PostgreSQL data types](https://www.postgresql.org/docs/current/static/datatype.html)

---

## Exporting and importing PostgreSQL databases

### Terminology

* *schema*: the structure of a database's tables and other objects

### Examples

Dumping a database:

```
pg_dump --no-owner [dbname] > dump.sql
```
