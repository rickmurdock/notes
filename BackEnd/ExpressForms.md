# Form Validation

### Terminology

* `body-parser`: middleware that parses incoming request bodies.

   * Stores a form's input in a JavaScript object.
   
   * Accessible through `req.body`.
   
   * For the following middlewares, the request `Content-Type` header must match the `type` option.
   
     * `bodyParser.json(options)`: parses `json`.
      
     * `bodyParser.raw(options)`: parses all bodies as `buffer`.
      
     * `bodyParser.text(options)`: parses all bodies as a `string`.
      
     * `bodyParser.urlencoded(options)`: parses `urlencoded` bodies, only.
     
   * See docs.
   
* express-validator: middleware for node-validator (string validators and sanitizers)

  * Validates input passed by the browser.
  
  * Configuration: expressValidator must come after bodyParser (see example).
  
  * Validations (see docs for complete list):
  
    * `req.checkBody()`: only checks `req.body`.
    
    * `req.checkQuery()`: only checks `req.query`.
    
    * `req.checkParam()`: only checks `req.params`.
    
  * Validators:
  
    * `isEmpty()`: validates if string has length of zero.
    
    * `isEmail()`: validates if string is an email.
    
    * `equals()`: validates string agains a string.
    
    * See all [validator](https://github.com/chriso/validator.js) for a complete list.
    
  * See docs.
  
> String validator, only.

### Body-parser

#### BODY-PARSER INSTALLATION

```
$ npm install body-parser --save
```

#### BODY-PARSER IMPLEMENTATION

```javascript
var express = require('express');
var bodyParser = require('body-parser');

// Create app
var app = express();

// Set app to use bodyParser()` middleware.
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));

app.get('/', function(req, res){
  // Set 'action' to '/'
  var html = '<form action="/" method="post">' +
             '<h1>User Name</h1>' +
             '<p>Enter your email</p>' +
             '<input type="text" name="email" placeholder="email address" />' +
             '<button type="submit">Submit</button>' +
         '</form>';         
  res.send(html);
});

// Receives data from form (action='/')
// 'req.body' now contains form data.
app.post('/', function(req, res){
  var email = req.body.email;
  var html = '<p>Your user name is: </p>' + email;
  res.send(html);
});
app.listen(3000);
```

### Express-validator

#### EXPRESS-VALIDATOR INSTALLATION

```
$ npm install express-validator --save
```

#### BODYPARSER AND EXPRESS-VALIDATOR IMPLEMENTATION

```javascript
var express = require('express');
var bodyParser = require('body-parser');
var expressValidator = require('express-validator');

// Create app
var app = express();

// Set app to use bodyParser()` middleware.
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
//'extended: false' parses strings and arrays.
//'extended: true' parses nested objects
//'expressValidator' must come after 'bodyParser', since data must be parsed first!
app.use(expressValidator());

app.get('/', function(req, res){
  // Set 'action' to '/'

  var html = '<form action="/" method="post">' +
             '<h1>User Name</h1>' +
             '<p>Enter a username</p>' +
             '<input type="text" name="user" placeholder="user name" />' +
             '<button type="submit">Submit</button>' +
        '</form>';
  res.send(html);
});

// Receives data from form (action='/')
// 'req.body' now contains form data.
app.post('/', function(req, res){
  //Call req.checkBody function.
    //Pass inputs to validate.
    //Tell middleware which validators to apply (chain one or more).
    req.checkBody("user", "You must enter a username!").notEmpty();

    var errors = req.validationErrors();
    if (errors) {
      // Render validation error messages
      var html = errors;
      res.send(html);
    } else {
      var user = req.body.user;
      var html = '<p>Your user name is: </p>' + user;
      res.send(html);
    }
  });
app.listen(3000);
```

#### TRY IT!

express-form-validation.zip (8 KB)

---

# Returning Appropriate HTTP Response Codes

### Terminology

HTTP response codes categories:

* 200s: Success

* 300s: Redirection

* 400s: Client error

* 500s: Server error

More information on each request:

* HTTP Status Cats API

* HTTP Statuses

### Most Common HTTP Status Codes
* 200 - Response was OK

* 201 - Created Successfully (Forms)

* 301 - Redirect Temporarily

* 302 - Redirect Permanently

* 400 - Problem with the Request (generic)

* 401 - Need to Sign In

* 403 - Unauthorized

* 404 - Not Found

* 422 - Problem with the content. (Errors)

* 500 - Server Error (Problem with Code)

* 503 - Server is unavailable (Problem with Service)

### Why Use HTTP Status Codes

When browsing a website, our browsers use HTTP statuses behind the scenes. For example, if you visit a page that has moved, the web app may response with a 301 response (MOVED), telling the browser the new location. Cat | Details

Our browser will transparently grab the "location" from the response and move us to the new location.

Alternatively, if the response is a 200, our browser knows everything went ok with the response and to parse the response and display our HTML.

# Examples

By default, express will add a status of 200 when we render, and a 302 when we redirect.

But what if we want to prevent a user from accessing a particular route in our application? We'll want to return a 403 ("forbidden").

```javascript
const express = require('express');
const path = require('path');
const mustacheExpress = require('mustache-express');
const app = express();
app.engine('mustache', mustacheExpress());
app.set('view engine', 'mustache')
app.use(express.static(path.join(__dirname, 'public')));

// Routes
app.get('/', function(req, res){
  res.render('index');
});

// If non-authorized user visits to '/dashboard', 403 and render 403.mustache in the errors directory
app.get('/dashboard', function(req, res){
  res.status(403);
  res.render("errors/403")
});


<div class='callout-download'>
<p>Download and run <code>npm install</code> and then <code>node http-codes.js</code>. When you visit <code>/dashboard</code>, it will render a 403 error.</p>

<ul>
<li><a href="https://tiy-learn-content.s3.amazonaws.com/c5740929-express-http-codes.zip">express-http-codes.zip</a> (9 KB)</li>
</ul>
</div>
```
---

# Receiving Uploaded Files Using Busboy

### Terminology

* busboy: module used to parse incoming HTML form data. Simplifies file upload process.

* pipe(): method used to read data from a 'file' and write to a destination writable stream.

* fs: module used to perform file system related operations on a machine, such as reading files, creating files, deleting files, updating files, and renaming files.
  
  * js. createWriteStream(): method used to create a writable data stream.

* Node.js OS module: module used for obtaining operating system related information.
  
  * The os.tmpdir(): method that returns the OS's default directory for temporary files in a string.

### Installation

```
npm install busboy --save
```

### Setup

#### INCLUDE

```javascript
  var http = require('http'), //If using HTTP server and client
  var path = require('path'),
  var os = require('os'),
  var fs = require('fs');

  var Busboy = require('busboy');
 ```
 
#### SAVING A FILE

Instantiate `busboy`:

```javascript
var busboy = new Busboy({ headers: req.headers });
```

Configure `busboy` to save upload file:

* You must include five parameters in the `busboy` middleware function: (fieldname, file, filename, encoding, mimetype)

```javascript
busboy.on('file', function(fieldname, file, filename, encoding, mimetype) {
    // Set a temporary directory to save incoming file.
     var saveTo = path.join(os.tmpDir(), path.basename(fieldname));
     // Or
     // Set project directory to save incoming file.
     var saveTo = path.join('./public/uploads/', path.basename(filename));
     // Save the incoming file.
     file.pipe(fs.createWriteStream(saveTo));
   });
```

#### FINISHING THE UPLOAD

```javascript
busboy.on('finish', function() {
  console.log('Upload complete');
  // Send back HTTP response.
  res.writeHead(200, { 'Connection': 'close' });
  // End response.
  res.end("File saved!");
});
```

#### IMPLEMENTATION

In this example we do not use the `HTTP` module and we use `views` and `routes`. We also perform a couple of checks before the file is uploaded:

* If no file is selected and the file is not a 'pdf' we send back a 500 code and we end the response.

* We also use fs-extra, which adds file system methods that aren't included in the native fs module and adds promise support to the fs methods.

```javascript
var express = require('express');
var path = require('path');
var index = require('./routes/index');
var bodyParser = require('body-parser');
var mustache = require('mustache-express');
var fs = require('fs-extra');
var Busboy = require('busboy');
var app = express();
app.engine('mustache', mustache());
app.set('view engine', 'mustache');
app.use(bodyParser.urlencoded({ extended: false }));
app.use(express.static(path.join(__dirname, 'public')));

app.use('/', function(req, res){
  res.render("index");
});

// Accept POST request on '/upload'.
app.post('/upload', function (req, res) {
  var busboy = new Busboy({ headers: req.headers });
  busboy.on('file', function(fieldname, file, filename, encoding, mimetype) {
    var saveTo = path.join('./public/uploads/', path.basename(filename));
    file.pipe(fs.createWriteStream(saveTo));
  });
  busboy.on('finish', function() {
    res.writeHead(200, { 'Connection': 'close' });
    res.end("Uploaded file to: /uploads");
  });
  //Parse HTTP-POST upload
  return req.pipe(busboy);
});

app.listen(3000, function () {
  console.log('Successfully started node application!')
})
```

> Remember to set the enctype in your HTML Form to "multipart/form-data"

```html
<form action="/upload" method="POST" enctype="multipart/form-data">
  <input type="file" name="file">
  <input type="submit" value="Upload file">
</form>
```
