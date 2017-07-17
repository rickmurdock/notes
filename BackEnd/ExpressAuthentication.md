# Express Authentication

#### BASIC AUTHENTICATION

* Client requests secure resource

* Server requests username and password

* Client sends username and password

* Server returns requested resource

#### SESSION BASED AUTHENTICATION

* Client authenticates by providing credentials via HTTP request.

* The server provides a `session_id` via HTTP response.

  - `session_id` is an identifier associated with a user account.
  
  - `session_id` can be stored in a cookie.
  
  - `session_id` is stateful.
  
* `session_id` is attached to to subsequent outgoing requests.

* Sessions can be limited to certain time periods

* Security concerns

* Scalability concerns

* Excessive memory usage

### Terminology

* *Authentication*: After becoming a subscriber, the user receives an authenticator e.g., a token and credentials, such as a user name. He or she is then permitted to perform online transactions within an authenticated session with a relying party, where they must provide proof that he or she possesses one or more authenticators

* *HTTP cookies*: An HTTP cookie (web cookie, browser cookie) is a small piece of data that a server sends to the user's web browser, that may store it and send it back together with the next request to the same server.

### Examples

![Server Client Image](https://github.com/rickmurdock/notes/blob/master/imageFiles/ServerClient.png)

### References

* [Wikipedia - Authentication](https://en.wikipedia.org/wiki/Authentication)

* [HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)

---

# Author a website with user authentication workflow

Use the npm package express-session to set up user authentication.

### Terminology

* *Authentication*: After becoming a subscriber, the user receives an authenticator e.g., a token and credentials, such as a user name. He or she is then permitted to perform online transactions within an authenticated session with a relying party, where they must provide proof that he or she possesses one or more authenticators

* *HTTP cookies*: An HTTP cookie (web cookie, browser cookie) is a small piece of data that a server sends to the user's web browser, that may store it and send it back together with the next request to the same server.

### Examples

```javascript
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

### References

* [NPM - express-session](https://www.npmjs.com/package/express-session)

* [Wikipedia - Authentication](https://en.wikipedia.org/wiki/Authentication)

* [HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)
