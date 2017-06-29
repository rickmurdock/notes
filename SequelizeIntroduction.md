# Sequelize: Introduction

## Create models with Sequelize

### Terminology

* *ORM*: stands for Object-Relational Mapper. Provides a way to access a database from your program while treating your database as an object and individual rows as objects.

* *model*: an object that holds data and is responsible for saving and updating its associated database record.

* *migration*: a file that programmatically alters your database schema.

### Setting up Sequelize
Make sure you have run `npm install -g sequelize-cli`!

1. `cd` into a directory with an Express app.

2. Run `npm install sequelize pg --save`. `pg` is the library for using PostgreSQL.

3. Run `sequelize init`. This will create the `config`, `migrations`, `seeders`, and `models` directories.

4. Edit `config/config.json`. The dialect should be `"postgres"` and the username should be your local username. Change the database names to reflect the actual project.

5. Create the development database using `createdb` on the command line.

6. Run `sequelize db:migrate` to test your connection.

**Note:** At this time, `pg` gives a deprecation notice with `sequelize`. This is nothing to worry about.

### Creating a model
To create a model with Sequelize, you run `sequelize model:create` on the command line with a set of flags. As an example, to create a `User` model with a name, an email, and a bio:

> sequelize model:create --name User --attributes 'name:string email:string bio:text'

The types specified here are named differently than PostgreSQL calls them. See ["Data Types"](http://docs.sequelizejs.com/manual/tutorial/models-definition.html#data-types) to get a full list. While these are upper-cased in the documentation, they can be lower-case on the command line. Options passed to them, like `Sequelize.TEXT('tiny')`, will have to be edited in the generated migration.

Run `sequelize db:migrate` to run the migration and update your database.

### References

* [Getting started with Sequelize](http://docs.sequelizejs.com/manual/installation/getting-started.html)

* "Data types" in [Model definition](http://docs.sequelizejs.com/manual/tutorial/models-definition.html)

* [Migrations](http://docs.sequelizejs.com/manual/tutorial/migrations.html)
