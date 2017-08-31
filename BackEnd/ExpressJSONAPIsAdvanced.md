# Filter results based on URL parameters in NodeJS  

### Terminology  

**Route Parameters**: 1Route parameters are named URL segments used to capture the values specified at their position in the URL. The named segments are prefixed with a colon and then the name (e.g. `/:your_parameter_name/`. The captured values are stored in the req.params object using the parameter names as keys (e.g. `req.params.your_parameter_name`).

**Query String**: 2a query string is the part of a uniform resource locator (URL) containing data that does not fit conveniently into a hierarchical path structure. The query string commonly includes fields added to a base URL by a Web browser or other client application, for example as part of an HTML form.

### Examples  

Using Express, Sequelize, and Body Parser with a `Todo` model defined, the following routes describe our API endpoints.

A `get()` request to the `/todos` endpoint will respond with all model instances in the todo table.

```javascript
app.get('/todos', function(req, res){
  Todo.findAll().then( function(data) {
    res.send(data);
  });
});
```

A `get()` request to the `/todos/:id` endpoint will respond with the model instance with the provided :id.

Ex `/todos/7`

```javascript
app.get('/todos/:id', function(req, res){
  id = req.query.id

  Todo.findById(id).then( function(todo) {
      res.send(todo);
  });
});
```

A `get()` request to the `/todos/complete/:bool` endpoint will respond with the model instances whose complete property value equals the :bool value provided.

Ex: `/todos/complete/true`

```javascript
app.get('/todos/complete/:bool', function(req, res){
  isComplete = req.query.bool

  Todo.findAll({
    where: {
      complete: isComplete
    }
  }).then( function(data) {
      res.send(data);
  });
});
// SELECT * FROM todo WHERE complete = true;
```

A `get()` request to the `/todos/length/:num` endpoint will respond with the model instances whose `description` property value's length equals the `:num` value provided.

Ex: `/todos/length/12`

```javascript
app.get('/todos/length/:num', function(req, res){
  num = req.query.num

  Todo.findAll({
    where: sequelize.where(sequelize.fn('char_length', sequelize.col('description')), num)
  }).then( function(data) {
      res.send(data);
  });
});
// SELECT * FROM todo WHERE char_length(description) = 12;
```

Using query parameters, a `get()` request to the `/todos/range` endpoint will respond with the model instances whose id property value is greater than or equal to the `min` value provided and whose `id` property value is less than or equal to the `max` value provided.

Ex: `GET /todos/range?min=3&max=6`

```javascript
app.get('/todos/range', function(req, res){
  const min = req.query.min;
  const max = req.query.max;

  Todo.findAll({
    where: {
      id: {
        $and: {
          $gte: min,
          $lte: max
        }
      }
    }
  }).then( function(data) {
      res.send(data);
  });
});
// SELECT * FROM todo WHERE id >= 3 AND id <= 6
```

### References  

##### Lesson Footnotes

* 1: MDN - Express Tutorial Part 4: Routes and controllers

* 2: Wikipedia- Query String

---

# Adding token based user authentication to an Express API  

### Terminology  

* `JSON Web Token`**(JWT)**: a secured base64url encoded JSON object based protocol for transmitting restricted data.

  * JSON object: made up of zero or more `name` and `value` pairs. `names` are strings and `values` are arbitrary JSON values.

### Advantages  

* The JWT contains all the information needed to identify a user.

* It eliminates the need for `session state`.

* Safe against CSRF attacks. Token is signed using secure methods such as HMAC SHA-256 or RSA.

* The same JWT can be used by several servers / domains. Single Sign On.

* The JWT can be sent with the `HTTP` request in the `body` or `header`.

### How It Works  

* Two-way protocol: `request`/`response`.

  * `response` (JWT) generated from the server.

  * The browser makes a `request` for JWT data.

  * The server generates a signed token and returns it to the browser.

* Subsequent requests: JWT can be sent with every `HTTP` request to the server in order to validate it and return secure resources.

![session vs token based diagram](./images/session-vs-token-based-diagram.png)

### Structure  

* JWT has three parts

  * `Header`.
  
  * `Payload`.
  
  * `Signature'.
  
#### Header  

* Includes the token type and hashing algorithm.

```javascript
{
  "alg": "HS256",
  "typ": "JWT"
}
```

#### Payload  

* Claims: user statements.

* 3 types of claims:

  * `public`: defined at will by the JWT user (developer).
  
  * `private`: share predefined information between parties.

  * `reversed`: predefined claims: iss, issuer; exp, expiration time; sub, subject; aud, audience.
  
* Base64Url encoded.

##### Example

```javascript
{
    "iss": "https://www.theironyard.com", // issuer
    "iat": 1497892326, // issued date
    "exp": 1529428326, // expiration
    "aud": "https://www.medium.com", // audience
    "sub": "JWT example", // subject
    "name": "John", // additional public claim
    "admin": "true" // additional public claim
}
```

#### Signature  

* Structure: `encoded header`, `encoded payload`, a `secret` and `encryption algorithm`.

##### Example

The above JWT object, along with a key and HS256 encryption would generate:

Key: qwertyuiopasdfghjklzxcvbnm123456

```javascript
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwczovL3d3dy50aGVpcm9ueWFyZC5jb20iLCJpYXQiOjE0OTc4OTIzMjYsImV4cCI6MTUyOTQyODMyNiwiYXVkIjoiaHR0cHM6Ly93d3cubWVkaXVtLmNvbSIsInN1YiI6ImV4YW1wbGUiLCJuYW1lIjoiSm9obiIsImFkbWluIjoidHJ1ZSJ9.fYSVpviIN3Igi55RtVQaygTm2b7NDlrHXP5oboTiOhI
```

Decode the token [here](https://jwt.io/#debugger)

### Generating And Verifying A JWT  

* For this demonstration we are not using views.

#### Prerequisites  

* Install `jsonwebtoken`.

```
npm install jsonwebtoken --save
```

* Install Express.

* Install Body parser.

* Connect to Database (optional)

* Install, configure, and initialize Sequelize.

  * Create a user model with user_name, password and user_email Sequelize attributes.

  *Run migration.
  
### Structure  

* JWT-auth-example

  * Controllers
  
    * example.js
    
    * user.js
    
  * Middlewares
  
    * auth.js
    
  * Models
  
    * user.js
    
  * index.js
  
  * config.js
  
  * package.json
  
config.js  

* Author a configuration file.

* Here we will store the key we use to sign the JWT. For best practices and security reasons it is best **not to commit** into the repository.

```javascript
module.exports = {
  // You may include other configuration options here.
  secret: 'supersecurepassword'
}
```

models/user.js  

* Model sample.

```javascript
var Sequelize = require('sequelize');
// Set database
var sequelize = {}; // Dev. Database
// Or production database below.
var sequelize = new Sequelize('database', 'username', 'password', {
  host: 'localhost',
  dialect: 'postgres'
});

'use strict'
module.export = function(sequelize, DataTypes){
  var User = sequelize.define( 'user', {
    id: {
      type: Sequelize.INTEGER,
      autoIncrement: true,
      primaryKey: true,
    },
    user_name: {
      type: Sequelize.STRING,
      allowNull: false,
    },
    user_email: {
      type: Sequelize.STRING,
      allowNull: false,
    },
    password: {
      type: Sequelize.STRING,
      allowNull: false,
    }

  }, {
    classMethods: {
      associate: function( models ) {
        //Associations can be defined here.
      }
    }
  });
  return User;
};
```

**generating the JWT - controllers/user.js**  

* Here a JWT object is created and sent with the request.

* In this example, the app will look for the user. If not found it will use the sequelize create() method to create one.

```javascript
var jwt = require('jsonwebtoken');
// Require the config file containing the key to sign the JWT.
var config = require('../config.js');
// Require sequelize
var Sequelize = require('sequelize');
// Set database
var sequelize = {}; // Dev. Database
// Or production database below.
var sequelize = new Sequelize('database', 'username', 'password', {
  host: 'localhost',
  dialect: 'postgres'
});
//Import user model
var User = require('./user');

module.exports = function(router){
  router.post('/login', function(req, res){
    User = function (req, res){
      User.findOne({
        where: { user_name: req.body.user_name }
      })
      .then(function (user) {
        if(!user){
          // If user does not exist, create one!
          User.create({
            user_name: req.body.user_name,
            user_email: req.body.user_email,
            password: req.body.password
          })
          .then(function(user){
            var token = jwt.sign( // Sign JWT token. Pass in claims.
              {
                user: user.id
              },
              config.secret, // Secret key.
              {
                expiresIn: 24 * 60 * 60
              });
              res.send(200, {
                'token': token,
                'userId': user.id,
                'userName': user.user_name
              });
            });
          } else {
            // If user exist, set status and return json message.
            res.status(404).json({
            error: {
              message: 'Username already exist!'
            }
          });
          }
        });
    }
  });
  return router;
}
```

**token verification - middlewares/auth.js**  

* Here we verify the JWT.

* We check for a token. If token is available, we verify.

  * If it is invalid, or it has expired, we return a 401 status code with an error message.

  * If token is not found, we redirect back to login.

```javascript
var jwt = require('jsonwebtoken');
var config = require('../config.js');

var auth = function(req, res, next) {
 var token = req.body.token || req.headers[‘x-access-token’];
  if (token) {
   jwt.verify(token, config.secret, function(err, decoded) {
      if (err) {
         console.error(‘Verification Error’, err);
         return res.status(401).send(err);
      } else {
         req.decoded = decoded;
         return next();
      }
   });
  } else {
    // Redirect back to login if token not found.
   res.redirect(401, '/')
   }
}
```

**controllers/example.js**  

* Here we create a route, which we will protect later.

```javascript
module.exports = function(router){
  router.get('/example', function(req, res){
    res.json({data: 'JWT authentication implementation!'});
  });
  return router;
}
```

**Implementation in index.js**  

```javascript
var express = require('express');
var app = express();
var router = express.Router();
var bodyParser = require('body-parser');

app.use(bodyParser.json());

// Add middleware.
app.use('/api', require('./middlewares/auth.js'));

// Protected route implementation. Available at /api/example
app.use('/api', require('./controllers/example.js')(router));

// Login router handler implementation.
app.use('/', require('./controllers/user.js')(router));

app.listen(3000);
```

**client side** 

* Manually set headers so that the middleware knows if the route is authenticated.

```javascript
headers[‘x-access-token’] = [jwt-token]
```


### Changing Default Algorithm And Signing Asynchronously  

```js
// Default algorith (HMAC SHA256)
var jwt = require('jsonwebtoken');
var token = jwt.sign({ foo: 'bar' }, 'secret');
//backdate a jwt 30 seconds
var older_token = jwt.sign({ foo: 'bar', iat: Math.floor(Date.now() / 1000) - 30 }, 'shhhhh');

// Using  SHA256
var cert = fs.readFileSync('private.key');  // get private key
var token = jwt.sign({ foo: 'bar' }, cert, { algorithm: 'RS256'});

// Signing asynchronously
jwt.sign({ foo: 'bar' }, cert, { algorithm: 'RS256' }, function(err, token) {
  console.log(token);
});
```

---

# Restricting Access To Information Based On User Role  

## Terminology  

`User authorization`: the process of granting resource access rights to specified entities based on role.

### Workflow  

* Incorporate user authentication (session or token).

* Create user model which includes a role (enum) field, i.e., `admin`, `owner`, `user`.

* Restrict information access:

  * Get user role from request.

  * Restrict resources based on user role.

## Authoring A Simple Middleware  

* For demonstration purposes.

### Example

```js
// Incorporate session / token authentication

function userRole (role) {
    return function (req, res, next) {
        if (req.session.user && req.session.user.role === role ) {
          // If it checks out, move along.
            next();
        } else {
          // If user does not checkout, set HTTP code and redirect back to home.
            res.redirect(403, '/');
        }
    }
}

app.get("/", function(req, res){
  /*
  Example:
  res.render('index', { title: 'home' });
  */
});

app.get("/post", userRole("admin"), function(req, res){
  /*
  "Create" post logic.
  Example:
  var title = req.body.title;
  Post.create({title: title}).then(function(post){
    //....
  })
  */
});

app.get("/post/:id", userRole("owner"), function(req, res){
  /*
  "View" post logic.
  Retrieve post based on id.
  Example:
  var id = req.query.id;
  var selectedPost = Post.findById(id);
  res.send(selectedSelected);
  */
});
```

## Using An Express Add-on  

* Production setting consideration:

  * Use an add-on such as Connect Roles.

### Connect Roles  

* Installation:

```
npm install connect-roles --save
```

* Usage

```js
var authentication = require('your-authentication-module-here'); // Session or token authentication.
var ConnectRoles = require('connect-roles');
var express = require('express');
var app = express();

// optional function to customize code that runs when user fails authorization.
var user = new ConnectRoles({
  failureHandler: function (req, res, action) {
    var accept = req.headers.accept || '';
    res.status(403);
    if (accept.indexOf('html') >= 0) {
      res.render('access-denied', {action: action});
    } else {
      res.send('Access Denied - You don\'t have permission to: ' + action);
    }
  }
});

app.use(authentication); // Use session or token authentication.
app.use(user.middleware());

//anonymous users can only access the home page
//returning false stops any more rules from being
//considered
user.use(function (req, action) {
  if (!req.isAuthenticated()) return action === 'access home page';
})

//Owner users can access private page, but
//they might not be the only ones so we don't return
//false if the user isn't a moderator
user.use('access private page', function (req) {
  if (req.user.role === 'owner') {
    return true;
  }
})

//admin users can access all pages
user.use(function (req) {
  if (req.user.role === 'admin') {
    return true;
  }
});

app.get('/', user.can('access home page'), function (req, res) {
  /*
  Example:
  res.render('index', { title: 'home' });
  */
});
app.get('/post/:id', user.can('access private page'), function (req, res) {
  /*
  "View" post logic.
  Retrieve post based on id.
  Example:
  var id = req.query.id;
  var selectedPost = Post.findById(id);
  res.send(selectedPost);
  */
});
app.get('/post', user.can('access admin page'), function (req, res) {
  /*
  "Create" post logic.
  Example:
  var title = req.body.title;
  Post.create({title: title}).then(function(post){
    //....
  })
  */
});

app.listen(3000);
```

* See [docs](http://documentup.com/ForbesLindesay/connect-roles#installation) for Connect Roles method details.
