# Structure of an Express app

### Vocabulary

* module: a JavaScript source file that defines `module.exports`

* `require`: a Node function to require other files or libraries. Comes in two types:

  * `require("library_name")` -- requires a core library or library installed via npm
  
  * `require("./library_name")` -- requires a file you have written in this project
  
* MVC: stands for Model View Controller. These are the three parts of this way of building software. The MVC architecture is used widely in web development.
The model controls access to data and ways to manipulate that data.
  
  * The view presents an interface to the user based off data.
  
  * The controller facilitates communication between models and views and handles user input.
  
  * In Express, models can be anything we choose to handle our data. Views are our templates or functions that format the response to the user. Controllers are our routes -- specifically, they're the functions we connect to routes. We could put those functions in a separate file and separate our routes and controllers.

### Examples

Example model in its own file:

```javascript
// models/user.js

const users = [
  {username: 'alexis', email: 'alexis@gmail.com'},
  {username: 'landry', email: 'landry@hotmail.com'}
  // ...
]

function getUser(username) {
  return users.find(function (user) {
    return user.username == username
  });
}

module.exports = {
  find: getUser,
  all: users
}

// app.js
const User = require('./models/user');
console.log(User.find('landry'));
console.log(User.all);
```

Example file layout for an Express application:

This example comes from a fictional blogging application.

```
project_directory/
|- package.json
|- app.js
|- routes/        <---- routes/controllers
|   |- users.js
|   \- posts.js
|- models/        <---- models
|   |- user.js
|   |- post.js
|   \- comment.js
|- views/         <---- templates
|   |- login.mustache
|   |- register.mustache
|   |- index.mustache
|   |- layout.mustache
|   \- ...
\- public/        <---- static files, could also be called "static"
    |- app.css
    \- ...
```

---

# Express routing

### Vocabulary

* *router*: a self-contained collection of routes and middleware

* *web request*: the message sent from your browser (or another program) that tells your web server to send back data. Contains a HTTP verb (GET, POST, etc), a URL, headers, and possibly a body.

* *web response*: the data sent back from your web server. Contains headers, a status code, and a body.

### Examples

Using `express.Router` for an admin section:

```javascript
// routes/admin.js
const express = require('express');
const router = express.Router();

router.use(function adminAuth (req, res) {
  // code for authorizing admins
});

router.get('/users', function (req, res) {
  res.render("users", {});
});

module.exports = router;

// app.js
const express = require('express');
const adminRouter = require('./routes/admin');

const app = express();

app.use('/admin', adminRouter);
```

Using multiple route handlers for a Twitter clone:

```javascript
const getUser = function (req, res, next) {
  const userId = req.params.userId;
  const user = User.get(userId);

  if (user) {
    req.user = user;
    next();
  } else {
    res.status(404).render("404");
  }
}

app.get('/:userId/timeline', getUser, function (req, res) {
  const tweets = req.user.tweets;
  res.render("timeline", {user: req.user, tweets: tweets});
});

app.get('/:userId/profile', getUser, function (req, res) {
  res.render("profile", {user: req.user});
});

app.get('/popular', function (req, res) {
  const tweets = getPopularTweets();
  res.render("popular", {tweets: tweets});
});
```

### Resources

* [Express.js Routing](https://expressjs.com/en/guide/routing.html)

* [Express.js API Docs on express.Router](https://expressjs.com/en/4x/api.html#router)

* [Express Routing - Advanced Techniques](http://jilles.me/express-routing-advanced-techniques/)

---

# Understanding middleware

### Terminology

* *middleware*: a function used with Express that takes a request, response, and `next`, which is a function to call to pass control to the next middleware or route handler in the chain. The concept of middleware is used in other web frameworks and software.

### Examples

An example of logging requests with a middleware:

```javascript
app.use(function (req, res, next) {
    console.log("Request at", new Date());
    console.log("URL:", req.url);
    console.log("Query:", req.query, "\n");
    next();
})
```

An example of setting a header. The `Server` header is the name of the server.

```javascript
const setServerName = function (name) {
  return function (req, res, next) {
      res.header("Server", name);
      next();
  }
}

app.use(setServerName("Dynamo 1000"));
```

---

# Output logs using the `morgan` package

Morgan is an HTTP request logger middleware for node.js.

The `morgan()` function accepts 2 arguments: `format` and `options`.

`format` can be a predefined string name, a string of a format string, or a function that will produce a log entry.

The `options` object accepts a number of properties.

### Terminology

### Examples

The following example demonstrates using the `'combined'` format (standard for Apache servers):

```javascript
var express = require('express')
var morgan = require('morgan')

var app = express()

app.use(morgan('combined'))

app.get('/', function (req, res) {
  res.send('hello, world!')
})
```

This example demonstrates using a conditional statement to check whether the app is running in the `'dev'` environment or in `'production'`. While in production morgan is using the `'common'` format and passing an `options` object. The `options` object, in this case, is set to skip logging if the response status code is less than `400` and stream logs to an external file: (__dirname + '/../morgan.log'). While in development morgan uses the `'dev'` format.

```javascript
var express = require('express')
var morgan = require('morgan')

var app = express()

if (app.get('env') == 'production') {
  app.use(morgan('common', {
    skip: function(req, res) {
      return res.statusCode < 400
    },
    stream: __dirname + '/../morgan.log'
  }));
} else {
  app.use(morgan('dev'));
}
```

### References

[GitHub - Express, Morgan](https://github.com/expressjs/morgan)
