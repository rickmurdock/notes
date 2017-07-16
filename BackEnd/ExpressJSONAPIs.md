# Use and verify express JSON APIs using postman and curl

### Terminology

* **Curl** (`curl`): a command line tool for URL manipulations and transfers, commonly used for making HTTP requests.

* **Postman** ([Postman](https://www.getpostman.com/)): a Graphical user interface for making HTTP requests.

#### POSTMAN FEATURES

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

### Examples

Show curl usage and options

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

### References

[Postman - Docs](https://www.getpostman.com/docs/)

---

# Return objects and arrays of data using JSON

### Terminology

* Content type: 1In responses, a `Content-Type` header tells the client what the content type of the returned content actually is.

When sending a JSON response we have to tell the client that the response we're sending is JSON in order for the client to accurately communicate and interpret the response.

### Examples

Using the Express `res.json()` method to send an object and an array of objects.

```javascript
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

```javascript
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

### References

* [MDN - Content Type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type)

* [Express - res.json](https://expressjs.com/en/api.html#res.json)

##### LESSON FOOTNOTES

1: [MDN - Content Type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type)

---

# Use routing appropriately for RESTful URL structure and HTTP verbs

Use appropriate HTTP verb for the action being performed:

* `GET`: Gets data from the server

* `POST`: Sends data to the server

* `PUT`: Update data on a server; overwrites a resource with a complete new body

* `PATCH`: Update data on a server; applies partial modifications to a resource

* `DELETE`: Deletes data from the server

### Terminology

* **HTTP verbs** also known as HTTP request methods. Verbs used by an application to indicate the desired action to be performed for a given resource.

### Examples

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

Furthermore, if the `id` of a relation is unique among other relations it is possible to eliminate the use of `/items` in the routes used to update or delete an existing relation.

* `PUT /relations/3` - Updates relation #3 for item #7

* `PATCH /relations/3` - Partially updates relation #3 for item #7

* `DELETE /relations/3` - Deletes relation #3 for item #7

---

# Describe standard REST conventions in NodeJS

### Terminology

* **spinal-case**: using a hyphen "-" to separate words

### Examples

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
