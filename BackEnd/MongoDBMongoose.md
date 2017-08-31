# Schemas and Models with Mongoose

### Terminology

* *schema* (for Mongoose): A schema defines the shape of your data to be stored in MongoDB, along with rules about that data. A Mongoose schema is different than a SQL schema in that it's an app-level definition of your data structure, not imposed by the database.

### Examples

#### CONNECTING TO MONGODB WITH MONGOOSE

Install Mongoose and Bluebird via NPM:

```
npm install mongoose@4.10.8 bluebird --save
```

NOTE: Make sure to use the version number above. Version 4.11 will not work and will cause problems.

Then in `app.js`:

```javascript
const mongoose = require('mongoose');
mongoose.Promise = require('bluebird');
// Replace "test" with your database name.
mongoose.connect('mongodb://localhost:27017/test');
```

#### CREATING A SCHEMA AND MODEL

```javascript
// models/recipe.js
const mongoose = require('mongoose');

const recipeSchema = new mongoose.Schema({
    name: { type: String, required: true, unique: true },
    prepTime: Number,
    cookTime: Number,
    ingredients: [{
        amount: { type: Number, required: true, default: 1 },
        measure: { type: String, lowercase: true, trim: true },
        ingredient: { type: String, required: true }
    }],
    steps: [String],
    source: {type: String}
})

const Recipe = mongoose.model('Recipe', recipeSchema);

module.exports = Recipe;
```

#### MAKING AN INSTANCE OF YOUR MODEL

```javascript
var recipe = new Recipe({name: "Pancakes"});
recipe.ingredients.push({ingredient: 'sugar', measure: " Tbsp"});
console.log(recipe.toObject());
// { name: 'Pancakes',
//   _id: 59553335625ccdda459e09b4,
//   steps: [],
//   ingredients:
//    [ { ingredient: 'sugar',
//        measure: 'tbsp',
//        _id: 59553335625ccdda459e09b5,
//        amount: 1 } ] }
```

### References

* [Mongoose.js](http://mongoosejs.com/)

* [MDN Express Tutorial - Part 3 - Using Mongoose](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/mongoose)

---

# Create, query, update, and delete models with Mongoose

### Examples

Creating an unsaved model:

```javascript
const recipe = new Recipe({name: "Pancakes", source: "Grandma"});
recipe.save()
  .then(function () {
    // actions to take on success
  })
  .catch(function () {
    // handle error
  })
```

Creating a model and saving in one command:

```javascript
Recipe.create({name: "Pancakes"})
  .then(handleSuccess)
  .catch(handleError);
```

Finding one record:

```javascript
Recipe.findOne({name: "Pancakes"})
  .then(handleSuccess)
  .catch(handleError);
```

Finding multiple records:

```javascript
Recipe.find({cookTime: {$gt: 15, $lt: 60}})
  .then(handleSuccess)
  .catch(handleError);
```

Complex queries:

```javascript
Recipe.find({source: "Grandma"})
  .where('cookTime').lt('30') // only cookTimes < 30
  .where({ingredients: {
    $lt: {$size: 5}}}) // only recipes with less than 5 ingredients
  .limit(10) // only 10 recipes
  .skip(5) // skip the first five
  .sort("-cookTime") // sort by cookTime descending
  .select("name cookTime") // only return name and cookTime
```

Updating one model:

```javascript
Recipe.updateOne({source: "Grandma"},
  {$push: {steps: "Call Grandma and tell her how it was."}})
```

Updating multiple models:

```javascript
Recipe.updateMany({source: "Grandma"},
  {$push: {steps: "Call Grandma and tell her how it was."}})
```  
  
Deleting one model:

```javascript
Recipe.deleteOne({name: "Green Bean Casserole"})
```

Deleting multiple models:

```javascript
Recipe.deleteOne({prepTime: {$gt: 60}})
```

### References

* [Mongoose docs](http://mongoosejs.com/docs/guide.html)

---

# Validating models with Mongoose

### MONGOOSE VALIDATION

* Used to validate model instances

* Defined in the SchemaType

* Middleware construct

  * runs before save with a `pre('save')` hook on every schema
  
* Can be called manually

  * doc.validate(callback)
  
  * doc.validateSync()
  
* Can be customized


### BUILT-IN VALIDATORS

* `required` properties are checked before saving. Their values must be present.

* `min` values can be required of `Number` property types

* `max` values can be required of `Number` property types

* `enum` values can be required of `String` property types

* `match` values can be required of `String` property types

* `maxLength` values can be required of `String` property types

* `minLength` values can be required of `String` property types

### CUSTOM VALIDATORS

* Can be added to a model property with the validate property

* The validate property expects either a function value or an array containing a function and an error message

* The validate function accepts the value of the model property as it's argument, allowing you to validate the value of the model property and return a boolean value

> `unique` is a helper, not a validator

### Examples

```javascript
const recipeSchema = new mongoose.Schema({
  // Using the `required` validator
  name: { type: String, required: true, unique: true },
  prepTime: {
    type: Number,
    // Using the `min` validator
    min: [1, 'Some prep time must be considered']
  },
  cookTime: {
    type: Number,
    // Using a custom validator and message
    validate: [ function(val) {
        // Get the last three characters of the value
        const lastThreeChars = val.substr(val.length - 3);
        return lastThreeChars === 'min'
      // Custom error message
      }, 'No good, {PATH} should be in minutes ("min")'
    ]
  },
  ingredients: [{
     amount: { type: Number, required: true, default: 1 },
     measure: { type: String, lowercase: true, trim: true },
     ingredient: { type: String, required: true }
  }],
  steps: [String],
  source: {type: String}
})

const Recipe = mongoose.model('Recipe', recipeSchema);


// This recipe has no name and no prepTime
var recipe = new Recipe();

recipe.save(function(error) {
  // The errors returned after a failed validation
  // contain an `errors` object containing the failed properties
  assert.equal(error.errors.name.message, 'Path `name` is required.');
  assert.equal(error.errors.prepTime.message, 'Some prep time must be considered');
});
```

> `{PATH}` is replaced with the invalid document path

### References

[Mongoose - Validation](http://mongoosejs.com/docs/validation.html)

---

# Extending Sequelize models

### Terminology

* *virtual field*: This appears to be a field and can even be set if configured to, but is not stored in the database, and cannot be used in queries.

* *instance method*: A method available on all individual model instances.

* *static method*: A method available on the model class.

* *query helper method*: A method available when chaining query methods.

### Examples

#### RECIPE SCHEMA USED THROUGHOUT LESSON

```javascript
const recipeSchema = new mongoose.Schema({
    name: { type: String, required: true, unique: true },
    prepTime: Number,
    cookTime: Number,
    ingredients: [{
        amount: { type: Number, required: true, default: 1 },
        measure: { type: String, lowercase: true, trim: true },
        ingredient: { type: String, required: true }
    }],
    steps: [String],
    source: {type: String}
})

const Recipe = mongoose.model(recipeSchema);
```

### VIRTUAL FIELDS

```javascript
recipeSchema.virtual('totalTime').get(function () {
  return (this.prepTime || 0) + (this.cookTime || 0);
});

// later on, with a recipe called "pancakes"...

pancakes.prepTime = 10;
pancakes.cookTime = 20;
console.log(pancakes.totalTime);
// => 30
```

You can even have a virtual field that you can set:

```javascript
recipeSchema.virtual('allSteps')
    .get(function () {
        return this.steps.join("\n");
    })
    .set(function (val) {
        this.steps = val.trim().split("\n");
    });

// later...
var recipe = new Recipe();
console.log(recipe.allSteps);
// => ""
var steps = `
Grease a pan.
Place batter on pan.
Cook until golden brown.
`;
recipe.allSteps = steps;
console.log(recipe.allSteps);
// => Grease a pan.
// => Place batter on pan.
// => Cook until golden brown.
console.log(steps[2]);
// => Cook until golden brown.
```

### INSTANCE METHODS

```javascript
recipeSchema.methods.findRecipesFromSameSource = function (callback) {
  return this.model('Recipe').find({
    source: this.source,
    _id: {$ne: this._id}
  }, callback);
}

// later...
grandmasPancakes.findRecipesFromSameSource()
  .then(function (recipes) {
    console.log(recipes)
  })
  .catch(handleError);
  
```

### STATIC METHODS

```javascript
recipeSchema.statics.findByMaxIngredients = function (maxIngredients, callback) {
    return this.find({ingredients: {$lte: {$size: maxIngredients}}});
};

// later...

Recipe.findByMaxIngredients(3).then(handleSuccess).catch(handleError);
```

### QUERY METHODS

```javascript
recipeSchema.query.maxIngredients = function (maxIngredients, callback) {
    return this.where({ingredients: {$lte: {$size: maxIngredients}}});
};

// later...

Recipe.find({cookTime: {$lte: 30}})
  .maxIngredients(3)
  .then(handleSuccess)
  .catch(handleError);
```  
  
References

* [Mongoose schema docs](http://mongoosejs.com/docs/guide.html)
