# Ports & IP Addresses

### Terminology

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

    * 1024 - 49151: registered ports. Assigned by [**IANA**](https://www.iana.org/) to an entity. i.e., 1020 Quicktime, 23399 Skype, 2375 Docker REST API.
    
    * 49152 - 65536: private/dynamic ports. Cannot be registered with IANA.

  * **Transport layer**: specify source and destination port number in the header.
  
  * **Protocol**: interaction rules used by endpoints in order to communicate.
  
    * `TCP` (Transmission Control Protocol): connection-based transmission of data.

      * Dependent on successful connection between endpoints.
      
      * Data is sent and received in sequential order.
      
* **IP/Port relationship**: the `IP address` is used to locate a particular device. The `port` is used to access a network service on that device.

> * All communication on the internet is from IP address to IP address.
> * A router assigns both 'private' and 'public' IP addresses. 'Public' addresses can be accessed over the internet. 'Private' addresses are only accessible from within the device's network.

### Example

In this example we run a local server on a local network, accessing port 3000.

```
192.168.2.1:3000
```

> Consider mentioning how to make a server available over the internet via port 80 and the global public ip address. This would entail opening port 80 on the router, setting up port forwarding, running the server on port 80 (proxies to port 3000), etc.

---

# Starting A Node Application

### Terminology

* Node.js: open-source and cross-platform JavaScript runtime used to build network application.

  * Provides JavaScript modules for web application development.
  
  * It is fast and never buffers data.
  
  * Scalable.

* Express.js: open-source Node.js framework designed to build web applications and API's.

### Pre-requisite

* Install node:

Check node version:

```
  $ node -v
```

Install node: `brew install node`

```
  $ brew install node
```

### Application Setup

* `mkdir`: express-app.

* `cd into` 'express-app' directory.

* `npm init`: accept all default options.

* `npm install`, then:

  * Add `express.js` dependency to package.json
  
```
$ npm install express --save
```

### Author Application

* `touch`: express-app.js

* Then:

```javascript
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

Simply call 'node' followed by the app's name in terminal.

```
$ node express-app.js
```

* Open browser to: localhost:3000

starting-node-app.jpeg

### Try it!
Starting Node.zip (424 KB)

---

# Configure Node to Serve Static Files in a Directory

### Terminology

 * express.static(): built-in middleware function used to configure the location of files to be served by an express.js application.

* paths:

  * Relative path:
  
```javascript
  app.use(express.static('public'))
```

* Adding a virtual path:

  * Specify mount point.
  
  * Virtual path prefix (non-existent in the file system). i.e., '/files'
  
```javascript
  app.use('/static', express.static('public'))
```

* Absolute path:

  * Safer when the app runs from a different directory.
  
```javascript
  app.use('/files', express.static(path.join(__dirname, 'public')))
```

### Example 1

File: ironyardlogo.png

Directory: serve-static-files/public

Configuration:

Relative to the static directory.

```javascript
app.use(express.static('public'))
```

Accessing file on port 3000:

http://localhost:3000/ironyardlogo.png

Example 2

File: video.mp4

Directory: serve-static-files/public

Configuration:

* Absolute path:

```javascript
app.use('/files', express.static(path.join(__dirname, 'public')))
```

Accessing file on port 3000:

http://localhost:3000/files/video.mp4

MULTIPLE DIRECTORIES

To add multiple directories, simply add them in the order you would like express.jsto search for a particular file.

```javascript
// Relative to the static directories
app.use(express.static('public'))
app.use(express.static('utilities'))

// Absolute path to the directories
app.use('/files', express.static(path.join(__dirname, 'public')))
app.use('/utilities', express.static(path.join(__dirname, 'utilities')))
```

The order of the static files determines how Express searches for a file.
Try it!
This sample does not contain the 'utilities' directory as per the example above.

serve-static-files.zip (2 MB)

---

# Authoring A Trivial Node App

### A Fill Murray Image App

* `mkdir`: trivial
* `cd` into 'trivial' directory.
* `npm init`: accept all default options.
* `npm install`, then:

  * Add `express web server` dependency: `npm install express --save`

* `mkdir`: 'files'

  * Add images files.
  
* `touch`: index.html

  * Add content.
  
* `touch`: 'trivial-app.js'

  * configure `express.static` to use a relative path.

```javascript
    app.use('/files', express.static('files'));
```
    
* configure `app.get()` to listen on root ('/').

* configure `app.get()` to serve `index.html` from `root`.

```javascript
//Listening on root
app.get('/', function (req, res) {
  //serve 'index.html'
  res.sendFile(path.join(__dirname + '/index.html'));
  //__dirname: resolves to  project folder.
})
```

#### IMPLEMENTATION

**trivial-app.js:**

```javascript
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

index.html:

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

### Start application

```
node trivial-app.js
```
* Open browser to: `localhost:3000`

### Try it!
