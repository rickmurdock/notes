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

```
sequelize model:create --name User --attributes 'name:string email:string bio:text'
```

The types specified here are named differently than PostgreSQL calls them. See ["Data Types"](http://docs.sequelizejs.com/manual/tutorial/models-definition.html#data-types) to get a full list. While these are upper-cased in the documentation, they can be lower-case on the command line. Options passed to them, like `Sequelize.TEXT('tiny')`, will have to be edited in the generated migration.

Run `sequelize db:migrate` to run the migration and update your database.

### References

* [Getting started with Sequelize](http://docs.sequelizejs.com/manual/installation/getting-started.html)

* "Data types" in [Model definition](http://docs.sequelizejs.com/manual/tutorial/models-definition.html)

* [Migrations](http://docs.sequelizejs.com/manual/tutorial/migrations.html)

---

## Use Sequelize models

### Terminology


* *promise*: an alternative to callbacks for handling asynchronous code. See [Why Promises?](http://bluebirdjs.com/docs/why-promises.html)

### Examples

##### REQUIRING MODELS

To require a model, `const models = require("./models")` and use the `models` object to access the model, like so:

```javascript
const models = require("./models");
models.User.findOne().then(function (user) {
  console.log(user);
})
```

If you try to require the model file directly (like `require("./models/user")`), you will get a function, not your model class.

##### BUILDING AND SAVING MODEL INSTANCES

To build an unsaved instance:

```javascript
const todo = models.Todo.build({
  title: 'Finish writing learning objective',
  description: 'Sequelize has a lot of concepts to learn',
  deadline: new Date()
});
```

To save the instance, call `save`. If you want to do something with the saved instance, you will have to use `.then` on the returned value from `save`.

````javascript
todo.save().then(function (newTodo) {
  console.log(newTodo.id);
})
````

`todo` will be updated, but it is asynchronous, so there isn't a guarantee when that will happen. `save()` returns a promise.

`create` will build and save in one step. It is really easy to make a mistake with `create`, though: it returns a promise, not a model instance.

```javascript
// BAD
const todo = models.Todo.create({
  title: 'Finish writing learning objective',
  description: 'Sequelize has a lot of concepts to learn',
  deadline: new Date()
});
console.log(todo);

// output
// Promise {
//   _bitField: 67108864,
//   _fulfillmentHandler0: undefined,
// ...
```

##### QUERYING FOR MODELS

###### FINDING BY ATTRIBUTES USING FINDONE()

* Returns the first instance that matches the `where` clause.

* Returns `null` if not found.

```javascript
User.findOne({
  where: {
    username: 'kerry'
  }
}).then(function (user) {
  //Code here
});
```

###### FINDING BY ID USING FINDBYID()

* Returns only one instance.

* Returns null if not found.

* In this example, we use an id of 1234.

```javascript
User.findById(1234).then(function (user) {
  //Code here
})
```

###### FINDING OR CREATING AN INSTANCE USING FINDORCREATE()

* `spread`: spreads an array of values to parameters. Only used with `findOrCreate()`. Works like `then`, but makes it easier to work with multiple parameters.

```javascript
User.findOrCreate({
  where: {
    username: 'brody'
  },
  defaults: {
    email: 'brody@email.com'
  }
}).spread(function (user, created) {
  console.log(user.id, created);
});
```

###### FINDING MULTIPLE INSTANCES USING FINDALL()

* Returns *all* instances.

```javascript
User.findAll().then(function (users) {
  // code here
})
```

###### SEARCH USING SPECIFIC ATTRIBUTES

* Returns *all* instances that match the `where` clause.

```javascript
User.findAll({
  where: {
    user_name: 'Dan',
  }
}).then(function (users) {
  // code here
});
```

###### SEARCH USING A RANGE

* Returns an array containing instances specified in the range.

```javascript
User.findAll({
  where: {
    id: [1234, 1256, 1345]
  },
}).then(function (users) {
  // code here
});
```

###### GET A COUNT

```javascript
User.count({
  where: { state: 'NC' }
}).then(function (count) {
  console.log(count);
})
```

##### LIMITING, OFFSETTING, AND ORDERING

###### LIMIT

```javascript
// Limit the results to 20.
User.findAll({ limit: 20 })
```

###### OFFSET

```javascript
// Steps over the first 5 elements.
User.findAll({ offset: 5 })
```

###### OFFSET AND LIMIT

```javascript
// Step over the first 15 elements, and limits it to 5
User.findAll({ offset: 15, limit: 5 })
```

###### ORDER

```javascript
User.findAll({order: ['startDate']})
// Returns ORDER BY startDate

User.findAll({order: [['startDate', 'DESC']]})
// Returns ORDER BY startDate DESC
```

##### UPDATING AND DESTROYING INSTANCES

###### UPDATING AN INSTANCE USING UPDATE()

```javascript
User.update({
  email: 'awesomewinter@email.com',
  age: 34
}, {
  where: {
    user_name: 'winter',
  }
}).then(function (user){
  // Code here.
  // Do something after updating instance.
})
```

###### DELETING AN INSTANCE USING DESTROY()

```javascript
User.destroy({
  where: {
    user_name: 'ryan'
  }
}).then(function(){
  // Code here.
  // Do something after destroying instance.
});
```

---

## Express/Sequelize CRUD Operations

### Terminology

* *CRUD*: stands for create, read, update, and delete -- the four major actions you take on data through most web applications

### Examples
Here is an example set of routes for a bookmarking application that show a list of links; let you add, edit, and delete links; and track the number of clicks on each link. You can find the entire application with discrete commits to see how it was built at [world-of-links](https://github.com/tiycnd/world-of-links).

```javascript
// index of all links
router.get("/links", function (req, res) {
    models.Link.findAll().then(function (links) {
        res.render("index", {
            links: links
        });
    });
});

// create form for link
router.get("/links/create", function (req, res) {
    res.render("form");
})

// create action for link
router.post("/links", function (req, res) {
    req.checkBody("title", "You must include a title.").notEmpty();
    req.checkBody("url", "Your URL is invalid.").isURL();

    const linkData = {
        title: req.body.title,
        url: req.body.url,
        descr: req.body.descr
    };

    req.getValidationResult().then(function (result) {
        if (result.isEmpty()) {
            models.Link.create(linkData).then(function (link) {
                res.redirect("/");
            });
        } else {
            const link = models.Link.build(linkData);
            const errors = result.mapped();
            res.render("form", {
                errors: errors,
                link: link
            })
        }
    })
});

// view link
router.get("/links/:linkId", function (req, res) {
    models.Link.findById(req.params.linkId).then(function (link) {
        if (link) {
            link.clicks += 1;
            link.save().then(function () {
                res.redirect(link.url);
            });
        } else {
            res.status(404).send('Not found.');
        }
    });
});

// edit form for link
router.get("/links/:linkId/edit", function (req, res) {
    models.Link.findById(req.params.linkId).then(function (link) {
        if (link) {
            res.render("form", {
                link: link,
                action: "/links/" + link.id,
                buttonText: "Update link"
            });
        } else {
            res.status(404).send('Not found.');
        }
    })
})

// edit action for link
router.post("/links/:linkId", function (req, res) {
    req.checkBody("title", "You must include a title.").notEmpty();
    req.checkBody("url", "Your URL is invalid.").isURL();

    const linkData = {
        title: req.body.title,
        url: req.body.url,
        descr: req.body.descr
    };

    models.Link.findById(req.params.linkId).then(function (link) {
        if (link) {
            req.getValidationResult().then(function (result) {
                if (result.isEmpty()) {
                    link.update(linkData).then(function (newLink) {
                        res.redirect("/");
                    });
                } else {
                    const errors = result.mapped();
                    res.render("form", {
                        link: linkData,
                        errors: errors,
                        action: "/links/" + link.id,
                        buttonText: "Update link"
                    });
                }
            });
        } else {
            res.status(404).send('Not found.');
        }
    })
});

// delete action for link
router.post("/links/:linkId/delete", function (req, res) {
    models.Link.findById(req.params.linkId).then(function (link) {
        if (link) {
            link.destroy().then(function () {
                res.redirect("/");
            })
        } else {
            res.status(404).send('Not found.');
        }
    })
});
```

### References

[Sequelize Tutorial - Instances](http://docs.sequelizejs.com/manual/tutorial/instances.html)

