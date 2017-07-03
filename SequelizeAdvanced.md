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

```
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

ADD PICS HERE

```
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

**NOTE**: The above associations would be distributed into their models' source file in associate methods, like so:

```
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

* Sequelize automatically runs validations on `create`, `update` and ``save`.

* Validations are defined in the model's attribute definitions.

* Set `validate` as follows: `validate: validation-method`.

* [See complete list of validations.](http://docs.sequelizejs.com/manual/tutorial/models-definition.html#validations)

* Validations are implemented with [validator.js](https://github.com/chriso/validator.js). See that library for documentation on individual validations.

An example:

```
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

