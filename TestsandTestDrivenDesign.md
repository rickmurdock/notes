# Test Driven Development (TDD)  

TDD Process

* Requirements form test cases

* Tests are written to describe functionality

* functionality is written and improved to pass tests

* Repeat the process

A unit usually consists of either a class or a module (group of related functions)

Test Structure

* setup: set the state for the unit or overall environment to run the test

* execution: run the unit and capture output

* validation: ensure the output passes the test

* cleanup: restore the state of the unit or environment

TDD looks like:

* simple design

  * only write tests that match requirements
  
  * only write code that passes tests
  
* modular design

  * writing smaller units that perform limited tasks can provide reusable code
  
  * units are intended to be tested independently and integrated later
  
* short development cycles

* repetition

* increased productivity

* test suite provides constant feedback

* less running of debuggers

* tests can act as documentation

TDD Limitations and Considerations:

* more time spent in early stages of development

* not reliable for user interfaces

* not reliable for working with databases

* can be difficult or limited when dependent on specific network configurations

* can be difficult to convince stakeholders that time spent on TDD is necessary and profitable

* the same developer writing tests and functionality can still allow for blindspots

* false sense of security can lead to lack of integration testing or compliance testing

* tests require additional maintenance

### Terminology  

* Test Driven Development (TDD): the process of writing tests that gauge functionality against a set of requirements, then writing the software to match the tests. Software that does not meet requirements is not allowed in the code base. The process of writing tests, programming to pass tests, and improving code is repeated indefinitely in a growing application. New features are are added through TDD. Legacy code (including tests) are refactored along the same cycle. TDD can and should be applied to applications from conception through development but it can also be used to refactor legacy applications. TDD inspires confidence in development by ensuring code is functionally sound and meets requirements.

### Examples  

A basic Chai unit test example:

```javascript
describe("multiplier", function() {
  chai.should();

  const result = multiplier(3, 5);

  result.should.be.a('number');
  result.should.equal(15);
});
```

The corresponding function:

```javascript
function multiplier(x, y) {
  return x * y;
}
```
---

# Author feature tests using Mocha and Chai for an Express app  

### Terminology  

* *MochaJS*: [MochaJS](https://mochajs.org/) is a testing framework for NodeJS and client side JavaScript applications. MochaJS provides asynchronous testing with reporting.

* *assert*: [assert](https://nodejs.org/api/assert.html) is a built-in Node library for asserting truth in tests.

* *ChaiJS*: [ChaiJS](http://chaijs.com/) is an assertion library for BDD and TDD that uses different syntax than assert.

* *Supertest*: [Supertest](https://github.com/visionmedia/supertest) is a library that allows to you make and test web requests and responses. Uses Superagent methods as well as its own .expect method.

### Examples  

```javascript
// app.js
const express = require('express');
const app = express();

app.get('/hello', function (req, res) {
  res.json({"hello": "world"})
})

if (require.main === "module") {
  app.listen(3000, function () {
      console.log('Express running on http://localhost:3000/.')
  });
}

module.exports = app;
```

```javascript
// test/hello_test.js
const request = require("supertest");
const assert = require("assert");
const app = require("../app");

describe("GET /hello", function () {
  it("should return successfully", function (done) {
    request(app)
      .get("/hello")
      .expect(200)
      .expect("Content-Type", "application/json; charset=utf-8")
      .expect(function (res) {
        assert.equal(res.body['hello'], "world");
      })
      .end(done);
  })
})
```

### Mocha.js  

Mocha is a library for writing tests. It must be paired with an assertion library like assert or Chai.

By default, Mocha runs all `.js` files in your `test/` directory.

Mocha uses two functions, `describe` and `it`, to define tests. The important thing to remember about these is that you do not have to require them. They are made available to your tests by Mocha.

```javascript
// test/sample_test.js
const assert = require("assert");

describe("A string", function () {
  it("can be uppercased", function () {
    assert.equal("Hello".toUpperCase(), "HELLO");
  });

  it("can be lowercased", function () {
    assert.equal("Hello".toLowerCase(), "hello");
  })
});
```

```
$ mocha

  A string
    ✓ can be uppercased
    ✓ can be lowercased

  2 passing (12ms)
```

`describe` is used to set a context for tests. It can be nested, with multiple `describe` calls inside a parent. `it` is used to write individual test cases.

You can test simple functions and methods this way.

### Testing controller functions  

In an Express app, you often want to test the functions that handle your routes -- your controller functions. To do this, install the library `supertest`. This library lets you make HTTP requests and run tests against the results. In addition, you will need to make your `app` object available. To do that, add `module.exports = app;` to the bottom of your `app.js` file (or whatever you have called the file where your Express app is created.)

You will also want to change your code that calls `app.listen` to look like the following:

```javascript
if (require.main === "module") {
  app.listen(3000, function () {
      console.log('Express running on http://localhost:3000/.')
  });
}
```

`require` is an object available in your module. When your module is run as a script `(node app.js)`, `require.main` will equal "module" and the `app.listen` code will be run. When your module is required from another module -- like we will do with the tests -- the `app.listen` code will not be run.

To test your controller functions, you will need to require `supertest` and learn about how Mocha handles asynchronous code. Let's look at an app and a controller test and then see how they work.

```javascript
// app.js
const express = require('express');
const app = express();

app.get('/hello', function (req, res) {
  res.json({"hello": "world"})
})

if (require.main === "module") {
  app.listen(3000, function () {
      console.log('Express running on http://localhost:3000/.')
  });
}

module.exports = app;
```

```javascript
// test/hello_test.js
const request = require("supertest");
const assert = require("assert");
const app = require("../app");

describe("GET /hello", function () {
  it("should return successfully", function (done) {
    request(app)
      .get("/hello")
      .expect(200)
      .expect("Content-Type", "application/json; charset=utf-8")
      .expect(function (res) {
        assert.equal(res.body['hello'], "world");
      })
      .end(done);
  })
})
```

The first line to notice is the line:

```javascript
it("should return successfully", function (done) {
  // ...
})
```

`done` is a function that it will pass to your test that will tell Mocha that your test is done when called. If you pass an argument to done, it will count that as a failed test.

The next line, request(app), sets up Supertest to make requests against your app. This handles running the app for you, so you don't have to know the address and port of the web application.

`.get("/hello")` makes a `GET` web request to `/hello`, as you might expect. This returns a promise that we can then run tests against.

`.expect` is a Supertest method that is very flexible and allows you to test multiple features of the response from the server. See the API documentation to learn all the ways in which .expect can be called. We are calling it three ways here:

```javascript
request(app)
  .get("/hello")
  // Checks to see if the status code is 200.
  .expect(200)
  // Checks to see if the header "Content-Type" is equal to the value here.
  .expect("Content-Type", "application/json; charset=utf-8")
  // Runs the function, passing in the response.
  // Assertions defined in the function.
  .expect(function (res) {
    assert.equal(res.body['hello'], "world");
  })
  .end(done);
```

Lastly, we call `.end(done)`. To understand this line, let's give `.end` a function we define:

```javascript
request(app)
  .get("/hello")
  // tests ...
  .end(function (err, res) {
    if (err) {
      done(err);
    } else {
      done();
    }
  });
```

If we have an error, we pass it to `done`, otherwise we do not. By using the shorter statement, `.end(done)`, we do a similar thing. The `err` and `res` are passed directly to `done`. If `err` is null, which it will be if there is no error, then that's great! If `err` is not null, then an error is passed to `done` and that lets us know about the error.

### npm test  

Our `package.json` file has a section we have not explored until now. The `scripts` object can contain terminal commands that we need to run frequently, including a command to test our code. Change your `package.json` to have the following `scripts` object:

```javascript
{
  "private": true,
  "dependencies": {
    // list of your dependencies
  },
  "scripts": {
    "test": "mocha"
  }
}
```

Now run `npm test`. Mocha should run. This gives us an easy-to-remember standard way to run tests for any Node project we come across, no matter what tools or configuration those tests may need.

---

# Author unit tests for Express models  

### Configuration  

To test more complicated applications -- ones with databases, for example -- we will require more set up. This has two steps. First, you will need to configure your application differently depending on how you are running it. A common way to do this is to set the `NODE_ENV` environment variable to a value like `development`, `production`, or `test` and then use that value to look up a configuration object. If you've looked at the `models/index.js` file that Sequelize creates, then you may have seen this.

To do this yourself, first create a file called `config.json`. Here is one for an app that uses MongoDB.

```javascript
// config.json
{
  "development": {
    "mongoURL": "mongodb://localhost:27017/coolapp_development"
  },
  "test": {
    "mongoURL": "mongodb://localhost:27017/coolapp_test"
  }
}
```

Then in app.js, we would write code like this:

```javascript
const express = require('express');
const mongoose = require('mongoose');
const app = express();

// Default to the "development" environment.
const nodeEnv = process.env.NODE_ENV || "development";
const config = require("./config.json")[nodeEnv];

mongoose.connect(config.mongoURL);
```

You can set up any configuration variables you need to in this way. Note that to make sure your tests use the test configuration, you will have to run `NODE_ENV=test mocha`. Change your `test` script in `package.json` to run this:

```javascript
{
  "scripts": {
    "test": "NODE_ENV=test mocha"
  }
}
```

There is a `config` [library you can install via NPM](https://www.npmjs.com/package/config) that takes this pattern and extends it. You may want to look at it if you need more support than this provides.

### Test hooks  

Mocha provides four hooks for us to use in our tests.

```javascript
describe('hooks', function() {
  before(function() {
    // runs before all tests in this block
  });

  after(function() {
    // runs after all tests in this block
  });

  beforeEach(function() {
    // runs before each test in this block
  });

  afterEach(function() {
    // runs after each test in this block
  });

  // test cases
});
```

These hooks can be used to set up data we need for our tests, clear out databases, or anything else we might need. They apply both inside the same `describe` function they are in and in any nested `describe` functions. They can even be defined outside of a `describe`. In that case, they will apply to all tests in the module. They also provide a `done` function in the case we have asynchronous actions we want to take.

For example, we may want to connect to MongoDB before our tests, and clear out a collection before each test in order to make sure we have a clean setup. Here is an example:

```javascript
const assert = require('assert');
const mongoose = require('mongoose');
const config = require("../config")[process.env.NODE_ENV || 'test'];
const Recipe = require("../models/recipe");

before("connect to Mongo", function (done) {
  mongoose.connect(config.mongoURL).then(done);
});

after("drop database", function (done) {
  mongoose.connection.dropDatabase(done);
})

describe("Recipe", function () {
    beforeEach("delete all recipes", function (done) {
        Recipe.deleteMany({}).then(() => done()).catch(done);
    });

    it("can be created", function (done) {
        Recipe.create({name: "Pancakes"}, function (err, recipe) {
            if (err) {
                done(err)
            } else {
                assert(recipe.id);
                assert(recipe.name == "Pancakes");
                done();
            }
        });
    });
})
```

### Why worry about this?  

You want to make sure your tests are *deterministic* -- that is, that when you run them, you get the same results each time. In the above code, it's assumed that the `name` field on recipes is unique. In this case, the test would fail if we didn't delete all recipes before each test and had multiple tests making a recipe with the name "Pancakes." Having a fresh database for each test is imperative to ensure your tests are deterministic.

---

# Naming Conventions and Class/Function Length Best Practices  

Terminology  

* **function scope**: Functions create a pocket of scope. Nested functions create internal pockets of scope which can access outer functions' scopes and global scopes. Outer scopes cannot access inner functions' scopes.

* **block scope**: Encapsulating scope in contexts other than functions; `if` statements and `for` loops, for example.

* **let**: The `let` keyword includes the variable declaration in the scope of any block it's contained in (usually a `{ .. }` pair). `let` implicitly uses any block's scope for its variable declaration.

* **const**: Like `let`, `const` uses block scope, however, `const` has a value which is fixed. Attempts to change a `const` declared variable will result in an error.

### Variables  

* Variables should be declared with `const` or `let` before they are used.

  * `var` is declared with function scope.
  
  * `const` and `let` are declared with block scope.
  
  * Use `let` and `const` in order isolate scope to containing blocks.
  
  * Use `let` if a variable's value will change.
  
  * Use `const` to declare variables whose values will not change (preferred method).
  
* Make variable names descriptive.

* Never use abbreviations in variables.

* Use camel case for two word variables.

* Avoid global variables.

* Avoid changing the type of a variable.

Functions  

* Functions should be declared before they are used.

 * Inner functions should be stored in a variable. Doing so makes it easier to read scope.
 
* Always use a verb in function names.

* Function names should be descriptive of their actions performed.

* Never use abbreviations in function names.

* Use camel case for two word function names.

* Each function/method should do only one thing.

### Characters in Naming  

* Use any letter of the alphabet, uppercase or lowercase.

* Use any number, 0 - 9.

* Do not use backslash `\`.

* Most variables start with a lowercase letter.

* Constructors start with an uppercase letter.

### Whitespace  

* The word `function` should be followed with one space.

* Apply one space after a keyword when followed by a left parentheses `(`.

 * Functions are the exception to this rule.
 
* The second parentheses `)` following a function, before an opening brace `{` should be followed by a space.

* Follow a comma `,` with one space.

* Within a `for` statement, follow a semicolon `;` with one space.

* A semicolon `;` at the end of a statement should be followed by a line break.

* When using a plus symbol `+` to concatenate values, include a space before and after the plus `+`.

### Line Breaks  

* Nested statements should be indented.

* Avoid excessively long lines.

* When breaking a line, do so after a left brace `{`, left bracket `[`, left parentheses `(`, comma `,`, or before a period `.`, question mark `?`, or colon `:`.

* Try to include just one statement per line.

* When a function statement is broken onto multiple lines, the closing brace `}` should be on the same level of indentation as the beginning of the function declaration.

* When an array is broken onto multiple lines, the closing bracket `]` should be on the same level of indentation as the beginning of the array.

* When an object is broken onto multiple lines, the closing brace `}` should be on the same level of indentation as the beginning of the object.

### Comments  

* Use comments sparingly.

* Using descriptive keywords and writing smaller statements will provide clarity to the reader, requiring no comments.

* When writing a comment make sure it is describing something that may not be immediately recognizable.

### Other  

* Use `===` whenever possible to ensure matching data types and to avoid type conversion. Avoid `==` value matching only.

* In general, make classes and objects as short as possible.

  * Use prototypal inheritance to distribute properties and methods.
  
  * Try to isolate values and functionality to small reusable components or modules.
  
* Don't repeat yourself (DRY).

### Examples  

```javascript
const bookList = [
  {
    title: 'Turtles All the Way Down',
    author: 'John Green'
  },
  {
    title: 'Camino Island',
    author: 'John Grisham'
  },
  {
    title: 'Wonder',
    author: 'R. J. Palacio'
  }
]

function makeSentence(object) {
  const sentence = 'Read ' + object.title + ', by ' + object.author + '.';
  console.log(sentence);
}

// call makeSentence with every object in bookList
for (let i = 0; i < bookList.length; i++) {
  makeSentence(bookList[i]);
}
```

> The use of the variable `i` obviously ins't very descriptive here. In this case `i` is a commonly known and widely used variable to describe an `iterator`. Another common one character variable you may encounter is `e` in place of `event`.
