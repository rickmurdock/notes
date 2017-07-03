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

```
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

```
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

```
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
