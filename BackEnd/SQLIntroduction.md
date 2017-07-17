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

If students have not yet learned about shell redirection, this may be a good place to teach that.

Dumping a database with `DROP TABLE` statements:

```
pg_dump --no-owner --clean [dbname] > dump.sql
```

Dumping just the data:

```
pg_dump --data-only [dbname] > dump.sql
```

Dumping just the schema:

```
pg_dump --no-owner --schema-only [dbname] > dump.sql
```

Restoring any of these:

```
psql [dbname] < dump.sql
```

### References

[pg_dump documentation](https://www.postgresql.org/docs/9.6/static/app-pgdump.html)

---

## SQL Statements

### Terminology
* *SQL*: Structured Query Language. A declarative programming language for defining databases and extracting and manipulating information from them.

* *predicate*: a Boolean (true/false) statement, often used to refer to the `WHERE` clause of SQL statements

### Filtering records using WHERE clauses

* `WHERE` is an SQL filter

   - The `WHERE` clause in a `SELECT` statement describes the rows from the source tables that are used to build the result set.
   
   - The `WHERE` clause in an `UPDATE` or `DELETE` statement narrows the rows that will be affected.
   
   - It sets a series of conditions and only the rows that meet the conditions are used to build a result set.
   
   - Comparison operators can be used, such as `=`, `< >`, `<`, and `>`
   
   - `IS NULL` and `IS NOT NULL` can be used to get only rows where a column is null or not null
   
   - Boolean and parentheses operators can be used to bind multiple expressions: `AND`, `OR`

* `ORDER BY` can be used to arrange the order of the results

* `LIMIT` and `OFFSET` can be used to get a subset of the results

### Examples

```
SELECT * FROM students;
SELECT name, favorite_color FROM students;

-- Find all graduated students
SELECT name, cohort FROM students WHERE graduate = 't';
-- Find all current students
SELECT name, cohort FROM students WHERE graduate = 'f';

-- Find all the students with no email
SELECT name FROM students WHERE email IS NULL;
SELECT name, email FROM students WHERE email IS NOT NULL;

-- Find students with no favorite color and no height
SELECT name FROM students WHERE favorite_color IS NULL AND height_cm IS NULL;

-- Students from shortest to tallest
SELECT name, height_cm FROM students ORDER BY height_cm;
-- Students from tallest to shortest
SELECT name, height_cm FROM students ORDER BY height_cm DESC;
-- Get rid of the nulls
SELECT name, height_cm FROM students WHERE height_cm IS NOT NULL ORDER BY height_cm DESC;

-- 10 tallest students
SELECT name FROM students ORDER BY height_cm DESC LIMIT 10;

-- Graduate cohort 11.
UPDATE students SET graduated = 't' WHERE cohort = 11;

-- set favorite color to yellow if null
UPDATE students SET favorite_color = 'yellow' WHERE favorite_color IS NULL;

-- Delete all students without an email.
DELETE FROM students WHERE email IS NULL;

-- Remove students over 7 feet tall
DELETE FROM students WHERE height_cm >= 213.36;
```

### References

* [Intro to SQL on Khan Academy](https://www.khanacademy.org/computing/computer-programming/sql)

* [PostgreSQL SELECT documentation](https://www.postgresql.org/docs/9.6/static/sql-select.html)

* [PostgreSQL UPDATE documentation](https://www.postgresql.org/docs/9.6/static/sql-update.html)

* [PostgreSQL DELETE documentation](https://www.postgresql.org/docs/9.6/static/sql-delete.html)
