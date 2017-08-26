# Cookies & Tokens  

This article is a brief discussion about cookies & tokens and is intended to provide some general information about why they are used in managing authentication. This article also discusses some of the pros and cons surrounding the use of these two technologies.

We all have come across messages such as "Remember me" and "Remember this machine". We usually see these in applications like Facebook, Instagram, Netflix, etc. Without the ability to "remember" the `state` of an application, we would have to enter our credentials and preferences every time we log into one of theses services. In order to avoid this, APIs use two different types of user authentication in order to keep `state` between the client and the server, `cookies` and `tokens`. They both serve a similar purpose, "remembering". Regardless of their similarity in purpose, `cookies` and `tokens` are inherently different.

## State  

`HTTP` protocol is `stateless`. This means that without any means of authenticating a user or application, both client and server would be clueless about each other's existence. The tried-and-true way of holding `state` has been through the use of `cookies`. In recent years `tokens` have gained popularity. They both have their advantages and disadvantages, but regardless of these, they both make maintaining `state` possible.

## Cookies  

A `cookie` is a piece of data used to hold `state` information. `Cookie` based authentication is considered `stateful` since authentication records are stored both in the server and in the client's browsers. On the back-end, a server keeps records of active sessions in a database. On the front-end, a `cookie` holds an identifier that is later used to match the server side records.

### Types of cookies  

Cookies come in three types: `session cookies`, `permanent cookies` and `secure cookies`

* A `session cookie` holds session specific data and is removed once the session ends.

* A `permanent cookie` holds session specific data and it is not removed once the session ends. It does eventually expire, but not immediately when the session ends.

* A `secure cookie` (also `http-only cookie`) is only sent using the `SSL` and `HTTPS` protocols. These are impossible to be read on client side. Effective against `XSRF` cross-site request forgery. Secure cookies are used to transmit unauthorized commands.

> A good example of `XSRF` attacks are emails claiming that "the prince of Timbuktu needs your help to transfer money to his aunt in the U.S". In this type of attack, a fraudulent cookie is use to capture sensitive information and use it maliciously.

### Example

In this example, we use a  `session cookie`.

1. You create an account on iWatchMoivies.com by entering you name, email and a password.

2. You log in using your credentials. The server processes the information, creates a user specific session, stores it in the database and places a `cookie` in the client browser.

3. Every time you login to watch an awesome kung-fu movie, the session ID stored in the `cookie` is verified against the database records.

4. When you log out the session ID is destroyed both on the server and client's browser.

### Pros  

* Tried-and-true.

* No client-side code needed for implementation.

### Cons  

* A `cookie` is sent with every request, even for requests that do not require authorization. This may decrease performance.

* Cannot be used across domains and platforms - they are domain specific.

* Extra steps are needed to protect against XSRF, i.e, `httpOnly cookie`.

* Tricky to implement with `CORS` (More on this later)

## Tokens

![session-vs-token-based-diagram.png](./images/session-vs-token-based-diagram.png)

Unlike cookies, `token` based authentication is `stateless`. Authorization records are not stored in the server. Instead, whenever an authorization request is made, it is sent with a `token`. The `token` is then verified for authenticity and are subsequently signed by the server. There are a couple of ways of sending a token, in the body of a `POST` or via `headers` through a `Bearer {JWT}`.1

### Example

1. You create an account on iInvest.com by entering you name, email and a password.

2. You log in using your credentials. The server verifies the credentials and returns a `token`, which is specifically `signed` by the server for security purposes. The token is stored in the client's browser either in `localStorage` or `sessionStorage`.

3. Every time you login to watch your money grow, the `signed token` is sent in the body of the `POST` or in the `header`, which in turn is decoded by the server.

4. When you log out, the `token` is destroyed by the client's browser.

### Pros  

* A `token` is sent with every authentication request, not *all* requests like with `cookies`.

* Can be used across multiple platforms and domains.

* The server's job is to verify and sign the `token`.

* Immune to XSRF.

* Easy to use with `CORS`.

* Compared to `cookies`, we can store any time of `meta-data` in a `token`.

### Cons  

* Client-side code needed for implementation, such as `JSON Web Token`.2

* Must be stored locally (Not stored in the server, `stateless`) using either `localStorage` or `sessionStorage`.

> Do not share sensitive information in a token. Even though the header and payload of a Token are encoded (Base64URL), and the token is signed (HMACSHA256 algorithm), it is not encrypted. In this type of situation, use `JSON Web Encryption`.3

## Conclusion  

APIs use two different types of user authentication in order to keep `state` between the client and the server, `cookies` and `tokens`. Regardless of their similarity in purpose, `cookies` and `tokens` are inherently different. When trying to decide which one to use, we must take performance, security, and usage flexibility into consideration. And remember, the next time the prince of Timbuktu sends you an email, delete it.

#### Lesson Footnotes

* 1: [JSON Web Token](https://jwt.io/)

* 2: [JSON Web Token](https://jwt.io/)

* 3: [JSON Web Encryption](https://tools.ietf.org/html/rfc7516)

---

# Understanding CORS  

Web apps depend on many resources in order to function. While some apps get all their resources from their own domain, other apps depend on third-party web services in order to function. One of your projects, for example, might depend on resources from a third-party. If there were no evil hackers in the world, then sharing resources between domains would be a breeze, but that is not the case. `CORS` was developed to make it easier and safer to share web services between domains.

## Why CORS?  

Security is a major concern in modern web development. To protect applications from dangerous scripts, all browsers implement something called the **Same-Origin Policy**. The Same-Origin Policy allows one web page to access data from another web page so long as both pages have the same origin (basically, they are on the same domain).

The trouble with this valuable policy is that it makes communicating data between pages that don't share the same origin rather difficult. Even though document contents are exposed through the `responseText` property when performing a local XMLHttpRequest, the Same-Origin Policy does not allow the data to be transferred if the data is requested from a page that has a different origin (Cross-Origin Request).

`CORS` (Cross-Origin Resource Sharing) keep things safe while allowing data to be shared between domains. Incidentally, `CORS` is also compatible with both `XMLHttpRequest` and `Fetch APIs`.

## How does CORS work?  

The first thing that we must point out about `CORS` is that it does not require any special coding from the front-end developer. In order for `CORS` to work, it must be enabled in the server's configuration. This is a job for a back-end developer. The server configuration is set to allow `CORS` by putting a particular value in the `Access-Control-Allow-Origin` header. When the header is properly configured, making a cross-origin `request` is just like making any other type of `request`. The other requirement is that the browser must support `CORS`.

There are certain methods that must be included in the `header` when performing a `CORS request`. These methods will also determine whether we are doing a `simple request` or a `preflight`.

## A Simple Request  

A simple request is one in which the `header` contains the following methods in the `header`:

* `GET`

* `HEAD`

* `POST`

`contentType` values allowed:

* `application/x-www-form-urlencoded`

* `multipart/form-data`

* `text/plain`

### Example

```javascript
let handler;
var xhr = new XMLHttpRequest();
var url = 'https://url-to-resource';

xhr.open('GET', url, true);
xhr.onreadystatechange = handler;
xhr.send();
```

Note, that we are using the `XMLHttpRequest()` here but you could easily just use the `fetch` library.

## Pre-flight  

More complicated requests (e.g. security sensitive requests) use something called `preflighting`. This methods validates a `request` before it is executed. This type of request contains the following methods in the `header`:

* `PUT`

* `DELETE`

* `CONNECT`

* `OPTIONS`

* `TRACE`

* `PATCH`

Or, the `contentType` includes:

* `application/x-www-form-urlencoded`

* `multipart/form-data`

* `text/plain`

Finally, it is considered a `preflight request` if the `headers` are set automatically by the browser and they include:

* `Accept`

* `Accept-Language`

* `Content-Language`

* `Content-Type (but note the additional requirements below)`

* `DPR`

* `Downlink`

* `Save-Data`

* `Viewport-Width`

* `Width`

> If `cookies` are received as part of the `response`, they are discarded. If these are a requirement, set the `withCredentials` property to `true` on the request before the `send()` method.

### Example

The following is an example of a `preflight` request, because it uses a `Content-Type` of `application/xml`.

```javascript
var xhr = new XMLHttpRequest();

xhr.open( 'POST', 'url-to-resource', true );
xhr.withCredentials = true;
xhr.setRequestHeader( 'Z-CUSTOM', 'shareme' );
xhr.setRequestHeader( 'Content-Type', 'application/xml' );
xhr.onreadystatechange = handler;
xhr.send(body);
```

## Conclusion  

While keeping things safe is important, so is sharing resource amongst different domains. This is specially true in open-source projects where we depend on third party services. `CORS` enables us to easily share and distribute such resources while keeping things safe. It also helps keep apps lighter and less complicated since not all resources must be developed, maintained and stored in the same domain.
