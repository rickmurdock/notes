# Describe common URL structures

### Terminology

**URI**: 1 A Uniform Resource Identifier (URI) is a string of characters used to identify a resource. Such identification enables interaction with representations of the resource over a network, typically the World Wide Web, using specific protocols.

**URL**: 2 A Uniform Resource Locator (URL), colloquially termed a web address,[1] is a reference to a web resource that specifies its location on a computer network and a mechanism for retrieving it.

**URN**: 3 A Uniform Resource Name (URN) is a Uniform Resource Identifier (URI) that uses the urn scheme.

HTTP: The Hypertext Transfer Protocol (HTTP) is a set of rules used to describe ow information is passed via hyperlinks between hypertext nodes. HTTP is the foundation of data communication for the World Wide Web.

### Examples

#### Structure

* URI

* URL

* URN

#### Protocol

* HTTP

* HTTPS

#### Domain name

* Second level domain name

  * Subdomain
  
  * Host name
  
* Top level domain name

#### Extensions

* Port

* Path to a file

* Parameters

* Anchor

| protocol |	domain name	| domain name	 | domain name |	port	| path to file |	parameters |	anchor |
| --- | --- | --- | --- |  --- |  --- |  --- |  --- |  
| |second level domain name |	second level domain name	| top level domain name |  |  |  |  |				
| | subdomain	| host name	|  |  |  |  |  | 				
| https:// |	www. |	example	| .com	| :80	| /path/to/somefile.html |	?key1=value1&key2=value2 |	#somewhereInTheDocument |

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

### References

#### LESSON FOOTNOTES

* 1: [Wikipedia - Uniform Resource Identifier](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier)

* 2: [Wikipedia - Uniform Resource Locator](https://en.wikipedia.org/wiki/URL)

* 3: [Wikipedia - Uniform Resource Name](https://en.wikipedia.org/wiki/Uniform_Resource_Name)

---

# Dynamic Routes in Express

### Terminology

Route Parameters: 1Route parameters are named URL segments used to capture the values specified at their position in the URL. The named segments are prefixed with a colon and then the name (e.g. /:your_parameter_name/. The captured values are stored in the req.params object using the parameter names as keys (e.g. req.params.your_parameter_name).

Word Character: 2A "word character" within ASCII typically means a letter of the alphabet A-Z (upper or lower case), the digits 0 to 9, and the underscore.

Examples

```javascript
app.get('/users', function (req, res) { /* ... */ });

app.get('/:dynamic_route', function (req, res) {
  res.send(req.params.dynamic_route);
});
```

### References

#### LESSON FOOTNOTES

1: [MDN - Express Tutorial Part 4: Routes and controllers](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/routes)

2: [MDN - Character - Word Character](https://en.wikipedia.org/wiki/Character_(computing)#Word_character)

---

# HTML Templates with Mustache

### Terminology

**template**: a predefined component written in a programming language and compiled into HTML at runtime by a template engine. Because they are written with a programming language they can contain variables and functions, can perform text replacement, file inclusion (or transclusion), conditional evaluations, and loops.

Examples

`./views/index.mustache`

```html
<p>
  Hello, {{ userName }}!
</p>
```

`./app.js`

```javascript
// requires and environment setup ...

const mustacheExpress = require('mustache-express');
app.engine('mustache', mustacheExpress());
app.set('views', './views')
app.set('view engine', 'mustache')

app.get('/', function (req, res) {
  res.render('index', { userName: 'Sam' })
})
```

### References

* [mustache reference](https://mustache.github.io/mustache.5.html)

* [GitHub - mustache.js](https://github.com/janl/mustache.js)

* [Using template engines with Express](https://expressjs.com/en/guide/using-template-engines.html)
