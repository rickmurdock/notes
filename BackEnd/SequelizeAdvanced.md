# Associations: belongsTo, hasOne, hasMany, and belongsToMany

### Terminology

* *associations*: relationships between models.

* *source model*: the model defining an association

* *target model*: the model to which an association is being defined

* *foreign key*: a database column that contains references to another table

* *target key*: a database column that a foreign key references.

* Sequelize associations:

  * `belongsTo`: creates an association in which the foreign key for the relationship exists on the source model. Establishes a one-to-one or many-to-one relationship.

  * `hasMany`: creates an association in which the foreign key for the relationship exists on the target model. Establishes a one-to-many relationship.

  * `hasOne`: creates an association in which the foreign key for the relationship exists on the target model. Establishes a one-to-one relationship.

  * `belongsToMany`: creates an association in which there are two foreign keys on a third table. Establishes a many-to-many relationship.

### Examples

##### A MIGRATION ADDING A FOREIGN KEY

```javascript
// migrations/20170625173808-add-user-id-to-todo.js
'use strict';

module.exports = {
  up: function (queryInterface, Sequelize) {
    return queryInterface.addColumn(
      'Todos',
      'userId',
      {
        type: Sequelize.INTEGER,
        allowNull: false,
        references: {
          model: 'Users',
          key: 'id'
        }
      }
    )
  },

  down: function (queryInterface, Sequelize) {
    return queryInterface.removeColumn('Todos', 'userId');
  }
};
```

### Tables for examples  

The "users" table:

| id |	username |
| --- | --- |
| 1	| alexis |
| 2	| river |
| 3	| dorian |

The "todos" table:

| id |  userId |	text |
| --- | --- | :--- |
| 1	|	1 | Do homework |
| 2	| 1	| Feed cat |
| 3	| 3	| Plan vacation |
 
The "authors" table:

| id	| name	| userId |
| --- | :--- | --- |
| 1	| Alexis Tseng |	1
| 2	| River Whitaker |	2
| 3	| Dorian Ramirez |	3

The "books" table:

| id	| title |
| --- | :--- |
| 1	| Vacation Planning for Type A Personalities |
| 2| 	An Exploration of the Netherworld |

The "authors_books" table:

| id | authorId	| bookId |
| --- | --- | --- |
| 1	| 1	| 1 |
| 2 | 2 | 1 |
| 3 |	3 |	2 |
| 4	| 1	| 2 |


```javascript
const User = this.sequelize.define('User', {/* attributes */});
const Todo  = this.sequelize.define('Todo', {/* attributes */});
const Author = this.sequelize.define('Author', {/* attributes */});
const Book  = this.sequelize.define('Book', {/* attributes */});

User.hasOne(Author, {foreignKey: 'userId'});
User.hasMany(Todo, {foreignKey: 'userId'});
Todo.belongsTo(User, {foreignKey: 'userId'});
Author.belongsTo(User, {foreignKey: 'userId'});
Author.belongsToMany(Book, {
  through: 'authors_books', foreignKey: 'authorId', otherKey: 'bookId'
});
Book.belongsToMany(Author, {
  through: 'authors_books', foreignKey: 'bookId', otherKey: 'authorId'
});
```

**NOTE**: The above associations would be distributed into their models' source file in`associate` methods, like so:

```javascript
// models/todo.js
'use strict';
module.exports = function(sequelize, DataTypes) {
  var Todo = sequelize.define('Todo', {
    body: {
      text: DataTypes.STRING,
      allowNull: false
    }
  }, {});

  Todo.associate = function (models) {
    Todo.belongsTo(models.User, {foreignKey: 'userId'});
  }
  return Todo;
};
```

---

# Author Model Class Methods, Attributes, And Instance methods

### Model validation

* Sequelize automatically runs validations on `create`, `update` and `save`.

* Validations are defined in the model's attribute definitions.

* Set `validate` as follows: `validate: validation-method`.

* [See complete list of validations.](http://docs.sequelizejs.com/manual/tutorial/models-definition.html#validations)

* Validations are implemented with [validator.js](https://github.com/chriso/validator.js). See that library for documentation on individual validations.

An example:

```javascript
var User = sequelize.define('user', {
  username: {
    type: Sequelize.STRING,
    unique: true,
    validate: {
      // Only allows letters.
      isAlpha: true
    }
  },
  email: {
    type: Sequelize.STRING,
    validate: {
      // Checks for email format (person@example.org)
      isEmail: true
    }
  }
});
```

### Handling validation and other errors

When an error occurs in a Sequelize method that returns a promise (like `create` or `save`), you have to use a `catch` method to handle it. An example:

```javascript
// invalid username
User.create({username: "!!!", email: "a@example.org"}).then(function (user) {
  console.log(user);
}).catch(function (err) {
  console.log("Error", err);
})
// will log the error
```

You can handle specific types of errors by passing the error type as the first argument to `catch`.

```javascript
User.create({username: "me", email: "me@example.org"})

// creating a second one will cause a UniqueConstraintError
User.create({username: "me", email: "woot@example.org"}).then(function (user) {
  console.log(user);
}).catch(Sequelize.UniqueConstraintError, function (err) {
  console.log("Username not unique!");
}).catch(Sequelize.ValidationError, function (err) {
  console.log("Not valid!", err);
}).catch(function (err) {
  // handle all other errors
  console.log("Oh no!", err);
})
```

### Bulk methods

##### CREATE INSTANCES IN BULK

Use the `bulkCreate()` method to create multiple instances at once. An example:

```jsx
User.bulkCreate([{
    username: 'Emerson',
    role: 'admin',
    isActive: false
}, {
    username: 'Grey',
    role: 'user',
    isActive: true
}, {
    username: 'Keelan',
    role: 'admin',
    isActive: true
}]).then(function() {
    return User.findAll();
}).then(function(users){
    console.log(users) // Returns an array of user objects.
});
```

### Validating bulk insertions  

* Set `validate` attribute to `true` as a second option of the `createBulk()` method.

* The `validate` attribute must also be set in the model.

An example:

```jsz
// Define model.
// Set validation.
User.bulkCreate([{
    username: 'john.smith',
    email: 'john.smith@email.com'
}, {
    username: 'mary.connor',
    email: 'mary.connor'
}], {
    validate: true
}).catch(function(errors) {
    console.log(errors);
})
```

### Update instances in bulk  

* Use the update() in conjunction with an attribute(s) to be updated and a where clause to update many instances at once.

```jsx
User.update({
  { isActive: 'true'}, // Set attribute value for update.
  { where: { user_role: 'admin'}}
}).spread(function (affectedCount, affectedRows) {
  // Returns two values in an array.
  return User.findAll();
}).then(function (users) {
  // Do something with users
})
```

### Delete instances in bulk  

Use the `destroy()` in conjunction with the `where` clause to delete many instances at once.

```jsx
User.destroy({
  where: {
    user_role: 'user'
  },
  truncate: true // Ignores the where clause and truncates the table.
}).then(function (affectedRows) {
  return User.findAll();
}).then(function (users) {
  // Do something with users.
});
```

## Defining Instance Methods  

* Instance methods add functionality to models.

* To add them, we assign them to the model's prototype.

* In this example we include a `fullEmail` function so that it is available in all instances of the model.

```jsx
'use strict';
module.exports = function(sequelize, DataTypes){
  var User = sequelize.define('user', {
    username: Sequelize.STRING,
    email: Sequelize.STRING
  }, {});
  User.prototype.fullEmail = function () {
    return `${this.username} <${this.email}>`;
  }
  return User;
};
```

* Use `this` to gain access to the method.

```jsx
User.create({
  username: 'cadence',
  email: 'cadence@example.org'
}).then(function(user){
  console.log(user.fullEmail()); //Prints 'cadence <cadence@example.org>'
});
```
