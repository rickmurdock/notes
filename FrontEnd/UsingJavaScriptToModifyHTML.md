# Selecting DOM Nodes  

As a front-end developer, you will interact with the DOM all the time. You will add elements to the page or remove them. You will add classes and attribute data to elements. You will animate the state of elements. You will do all of these things and more. However, for you to manipulate an element, you must first select it from the page.

In this article, we will discuss several ways to select DOM nodes so that they can be manipulated.

## Get Element By Id  

The `document.getElementById()` API is common for selecting a **single element** in the DOM. That element must have and `id` attribute. This function returns a single node.

```js
// Returns a single element with an id of "main_title"
// and stores it in a variable "main"
let main = document.getElementById( "main_title" );

// Returns a single element with an id of "sidebar"
// and stores it in a variable "side"
let side = document.getElementById( "sidebar" );

// Returns a single element with an id of "category"
// and stores it in a variable "cat"
let cat = document.getElementById( "category" );
```

## Get Element By Tag  

The `document.getElementsByTagName()` API is used to select **all page elements of a certain tag type**. For instance, it can be used to select all `h1` tags or all `p` tags. This function will return an array of nodes.

```js
// Returns an array of "h1: tags
// and stores it in a variable "titles"
let titles = document.getElementsByTagName( "h1" );

// Returns an array of "p" tags
// and stores it in a variable "paragraphs"
let paragraphs = document.getElementsByTagName( "p" );

// Returns an array of "li" tags
// and stores it in a variable "listitems"
let listitems = document.getElementsByTagName( "li" );
```

## Selecting Elements By Class Name  

The `document.getElementsByClassName()` API is used to select **all page elements with a certain class attribute**. This function returns an array of nodes.

```js
// Returns an array of elements with a class of "group"
// and stores it in a variable "groups"
let groups = document.getElementsByClassName( "group" );

// Returns an array of elements with a class of "category"
// and stores it in a variable "categories"
let categories = document.getElementsByClassName( "category" );

// Returns an array of elements with a class of "element"
// and stores it in a variable "elements"
let elements = document.getElementsByClassName( "element" );
```

# Using CSS Selectors  

The following APIs allow us to use the same selectors as we use in our CSS documents. These functions take any valid CSS selector as a string and return nodes that match that CSS selector.

# Selecting a Single Node  

`document.querySelector()` allows us to select *any single element* in the DOM. This function takes any valid CSS selector and returns the first element that matches. If more than one element matches, it returns only the first. If no element matches the query, it returns `null`.

```js
// Returns the first "h1" node and stores it
// in the variable "el"
let el = document.querySelector( "h1" );

// Returns the first node with a class of "record"
// and stores it in the variable "rec"
let rec = document.querySelector( ".record" );

// Returns the first "h3" node that is a child of a "div" and stores it
// in the variable "sub"
let sub = document.querySelector( "div > h3" );
```

`element.querySelector()` allows us to select the *first descendant* of the base element on which it is invoked. If we select an element, we can *then select its descendants with `querySelector`*.

```js
// Returns the first "section" node and stores it
// in the variable "sec"
let sec = document.querySelector( "section" );

// Returns the first "h1" node that is a descendant of "sec"
// and stores it in the variable "el"
let el = sec.querySelector( "h1" );

// Returns the first node with a class of "record"
// node that is a descendant of "sec" and stores it
// in the variable "rec"
let rec = sec.querySelector( ".record" );

// Returns the first "h3" node that is a child of a "div"
// that is a descendant of "sec" and stores it
// in the variable "sub"
let sub = sec.querySelector( "div > h3" );
```

## Selecting Multiple Nodes  

`document.querySelectorAll()` allows us to select *a group of elements* in the DOM. This function *returns an array* of nodes, even if the query finds only one element.

```js
// Returns an array of "h1" nodes and stores it
// in the variable "els"
let els = document.querySelectorAll( "h1" );

// Returns an array of nodes with a class of "record"
// and stores it in the variable "recs"
let recs = document.querySelectorAll( ".record" );

// Returns an array of "h3" nodes that
// is a child of a "div" and stores it
// in the variable "subs"
let subs = document.querySelectorAll( "div > h3" );
```

`element.querySelectorAll()` allows us to select *a group of elements* that are descendants of the element on which it is invoked. This function returns an array of nodes. It does not return the base element.

```js
// Returns the first "section" node and stores it
// in the variable "sec"
let sec = document.querySelector( "section" );

// Returns an array of all "h1" node that are descendant of "sec"
// and stores them in the variable "els"
let els = sec.querySelectorAll( "h1" );

// Returns an array of nodes with a class of "record"
// and are descendants of "sec" and stores the array
// in the variable "recs"
let recs = sec.querySelectorAll( ".record" );

// Returns an array of "h3" nodes that are children of "div" nodes
// that are descendants of "sec" and stores them
// in the variable "sub"
let subs = sec.querySelectorAll( "div > h3" );
```

## Using Query Selectors  

Below is an example of how to use both `document` and `element` query selectors.

You are about to see a lot of code samples demonstrating how JavaScript interacts with the DOM. You will need to thoroughly review the HTML and JS files to understand what is happening. Don't skim through these samples. Read through the code found in both files. Run the Preview. Inspect the running code through the browser's console. Dig around each sample until you understand what is occurring.

Open your inspector and go to the console panel. You should see the output from the query selectors defined in `script.js`. With `index.html` open in the browser view, you can hover over the outputs in the console and see those elements highlighted in the browser view.

> Each time you run a code sample, clear the console. Keeping your console clear will keep the logging statements visible, uncluttered with older information that might cause confusion. In the upper left-hand corner (it looks like this ⍉ ) of your console panel is a small button that will clear the data from your console. While in the editor view of your code samples, and before you press the "Preview" button, clear the console.

Each selector is used multiple times in the JavaScript files included in each code sample. The JavaScript code is heavily commented to explain what is occurring. To find notes about the JavaScript code, inspect the `script.js` files and read through the comments.

### document.querySelector()  

`document.querySelector` will return *the first node* that matches the selector.

```js
 1 <!DOCTYPE html>
 2 <html>
 3     <head>
 4         <title>Introduction to the DOM</title>
 5     </head>
 6     <body>
 7         <p id="first-paragraph">This is a paragraph.</p>
 8         <p>This is a second paragraph.</p>
 9         <ul class="favorite-foods">
10             <li class="beef">Steak</li>
11             <li class="seafood">Shrimp</li>
12             <li class="poultry">Wings</li>
13        </ul>
14         <ul class="favorite-sodas">
15             <li>Dr Pepper</li>
16             <li>Coke</li>
17             <li>IBC Root Beer</li>
18         </ul>
19         <script src="script.js"></script>
20     </body>
21 </html>
``` 

### document.querySelectorAll()  

`document.querySelectorAll` will return an array of all nodes that match the selector.

```js
 1 <!DOCTYPE html>
 2 <html>
 3   <head>
 4     <title>Introduction to the DOM</title>
 5   </head>
 6   <body>
 7     <p id="first-paragraph">This is a paragraph.</p>
 8     <p>This is a second paragraph.</p>
 9     <ul class="favorite-foods">
10       <li class="beef">Steak</li>
11       <li class="seafood">Shrimp</li>
12       <li class="poultry">Wings</li>
13     </ul>
14     <ul class="favorite-sodas">
15       <li>Dr Pepper</li>
16       <li>Coke</li>
17       <li>IBC Root Beer</li>
18     </ul>
19     <script src="script.js"></script>
20   </body>
21 </html>
``` 

## Element Query Selectors  

Alternatively, we can call `querySelector` and `querySelectorAll` on **element** and the query will only look at nodes that are descendants of **element**. The following example contains several examples of how these element selectors are used to select and store references to nodes.

```js
 1 <!DOCTYPE html>
 2 <html>
 3   <head>
 4     <title>Introduction to the DOM</title>
 5   </head>
 6   <body>
 7     <p id="first-paragraph">This is a paragraph.</p>
 8     <p>This is a second paragraph.</p>
 9     <ul class="favorite-foods">
10       <li class="beef">Steak</li>
11       <li class="seafood">Shrimp</li>
12       <li class="poultry">Wings</li>
13     </ul>
14     <ul class="favorite-sodas">
15       <li>Dr Pepper</li>
16       <li>Coke</li>
17       <li>IBC Root Beer</li>
18     </ul>
19     <script src="script.js"></script>
20   </body>
21 </html>
``` 

## Conclusion  

Selecting DOM elements is the first step needed in order to gain JavaScript access to their properties. The DOM has many functions for selecting nodes. In this article, we briefly showed `getElementByTagName`, `getElementById`, and `getElementsByClassName`. These 3 functions are extremely useful, but `querySelector( )` and `querySelectorAll( )` are more common and are more convenient because they use the same selectors as our CSS files.

This article focused on how to select and store references to DOM nodes, an essential skill for gaining access to all of the properties of DOM nodes that we will change later when building dynamic web pages.

---

# Creating, Updating, and Removing DOM Nodes  

Most modern websites and web apps involve some DOM manipulation. Some web apps depend heavily on dynamically altering content. You will need to develop a firm understanding of how to create, update, and remove DOM elements.

In this article, we will discuss DOM manipulation, including:

* **Creating New DOM Nodes** - To update a page with new content, you must create a node to contain that content.

* **Inserting New DOM Nodes into the Page** - When new nodes are created, they aren't automatically attached to the page. You have to include them by manually appending them to a parent node.

* **Deleting DOM Nodes** - Sometimes, you may need to remove a node from the page entirely.

* **Altering Properties of Nodes** - Creating and inserting nodes won't always accomplish all of your aims. You may also need to update the text of a node, or add attribute values.

## Creating New DOM Nodes  

`document.createElement()` is use to create and return new DOM nodes. This function takes a string specifying which node to create.

```js
// returns a new "h1" node and stores it
// in the el1 variable
let el1 = document.createElement( "h1" );

// returns a new "p" node and stores it
// in the el2 variable
let el2 = document.createElement( "p" );

// returns a new "div" node and stores it
// in the el3 variable
let el3 = document.createElement( "div" );
```

When nodes get created, they are not initially visible on the page. To attach them to the DOM node tree, append them to a parent node.

## Inserting New DOM Nodes into the Page  

Several methods exist for adding nodes to the page, but this article will focus on `element.appendChild()`. The `appendChild` function takes a node as its argument and appends it as the last child of the calling node.

```js
// Select the parent element from the page
let element = document.getElementById( "ul" );

// Create a new element
let newElement = document.createElement( "li" );

// Append the new element to the parent element
element.appendChild( newElement );
```

In the following example, I'll create three new `li` nodes and append them to the unordered list. I will also create text nodes for the list items and attach those as well. When you run "Preview," you will see no delay before the list items appear. The JavaScript will run *as fast as possible*, so it will look as if the `li` nodes were always there.

Look at `index.html` to see the initial state, before the `li` nodes are added.

```js
 1 <!DOCTYPE html>
 2 <html>
 3     <head>
 4         <meta charset="UTF-8">
 5         <title>Dom Manipulation</title>
 6         <link rel="stylesheet" href="style.css">
 7     </head>
 8     <body>
 9         <ul id="list"></ul>
10         <script src="script.js"></script>
11     </body>
12 </html>
 ```

# Deleting DOM Nodes  

Frequently, you will also need to remove DOM nodes. To delete a DOM node, select the node and use `element.remove()`.

In the following code sample, the HTML is written with 4 list items in the unordered list. In `script.js`, we select all four list items using document.getElementsByTagName() and then use element.remove() to delete the even list items. Because document.getElementsByTagName() returns an array, we use bracket notation to access the items and remove them from the array.

```
 1 <!DOCTYPE html>
 2 <html>
 3     <head>
 4         <meta charset="UTF-8">
 5         <title>Dom Manipulation</title>
 6         <link rel="stylesheet" href="style.css">
 7     </head>
 8     <body>
 9         <ul id="list">
10             <li>This is the first list item text.</li>
11             <li>Text for the second item.</li>
12             <li>Third item text.</li>
13             <li>Item number four.</li>
14         </ul>
15         <script src="script.js"></script>
16     </body>
17 </html>
``` 

# Manipulating HTML content  

## element.innerHTML  

`element.innerHTML` - This property provides a reference to the child HTML of an element. You should treat this as a "read only" property. If if you need to change the HTML contents of a node, you should delete the content and then replace it with a new node of content.

```html
 1 <!DOCTYPE html>
 2 <html>
 3     <head>
 4         <meta charset="UTF-8">
 5         <title>Dom Manipulation</title>
 6         <link rel="stylesheet" href="style.css">
 7     </head>
 8     <body>
 9         <ul id="list">
10             <li>This is the first list item text.</li>
11             <li>Text for the second item.</li>
12         </ul>
13         <script src="script.js"></script>
14     </body>
15 </html>
``` 

### element.textContent

`element.textContent` - This property references the textual content of an HTML tag as text, removing any HTML tags. You can also change the text of an element. For example, if you want to change the text of an element with the id 'selector', we can do this:

```js
let element = document.querySelector('selector');
element.textContent = "I have changed the text!";
```

This property is used to retrieve or alter content on an element. In contrast to innerHTML, textContent will not allow HTML content.

```js
let element = document.querySelector('selector');

//create the element
let listItem = document.createElement('li');

// Add text content to the newly created 'li' item.
// Compared to innerHTML, notice how no style was added.
listItem.textContent = 'SevenUp';
```

# Manipulating Classes and Attributes  

### element.classList Object  

```js
element.classList - A powerful object for manipulating classes. It offers robust options, from adding multiple classes, to toggling classes.

let element = document.querySelector('selector');

//adds the class
element.classList.add('highlighted');

//adds multiple classes
element.classList.add('highlighted', 'important');

//removes the class
element.classList.remove('highlighted');

//adds it back since it no longer has it
element.classList.toggle('highlighted');

//removes it since it was just added.
element.classList.toggle('highlighted');
```

> `classList.contains()`: verifies if an element’s list of classes contains a specific class.

## className Property  

If supporting older browsers, we can use the `className` property on each element. Whatever string we assign to the property will be the new CSS class for the element. This method is not as robust as the `classList` method, but it does allow us to append classes. In the example below we add a class, and in the second, we append to it.

```js
// Lets assume that "element" already has a class of "first"
let element = document.querySelector('selector');

// Append a space and a new class to the className string
element.className += " highlighted";

// A whitespace character `" "` is used between the opening quote and the class name.
// The updated class is now "first highlighted"
```

A problem with this property is that you may accidentally overwrite any existing class names if you aren't careful.

```js
let element = document.querySelector('selector');
element.className = 'highlighted';

// This property holds a string, and you can overwrite it by setting it
// to a new string. Whatever the className use to be, it
// has been updated to "highlighted"
```

You can get around this type of problem in a couple of ways. We can save the current className in a variable, append our new classes to it, and then reset the className property to the new augmented string (which includes the original classes). You can also use external libraries like jQuery to give you a better API that does work in older browsers.

## Setting and Changing Attributes  

Use `element.setAttribute()` to either change an existing attribute or add one. This method accepts two parameters, a `name` and a `value`. There will be times when you will either want to set or change an attribute. For example, we might want to update where the 'next' button of a form takes us by changing the `href` of its anchor tag based on the value of a 'select option'. In the example below, we update the existing `href` value from our HTML example to take us to The Iron Yard's website.

```js
// select an element with a class of "link"
let link = document.querySelector('.link');

// give link an "href" attribute value of "http://www.theironyard.com"
link.setAttribute("href", "http://www.theironyard.com");
```

## Manipulating Other Common Properties of Elements  

Once you have references to the DOM elements you want to work with, there are some useful properties available. The exact set of properties varies depending on what interfaces the element implements. For example, all of the HTML elements implement `HTMLElement`, `Element`, `ParentNode`, and `Node`. However, you may have non-HTML elements that do not implement `ParentNode` and therefore don't have the `children` property. Beyond that different elements will have additional properties specific to that element. For example, `HTMLImageElement` has a `src` property.

## ID Property  

The `id` property can be used to access or change an element's ID. This example prints the id for each span tag in the document to the console.

```html
 // index.html
 1 <!DOCTYPE html>
 2 <html>
 3    <head>
 4         <title>Hello World</title>
 5     </head>
 6     <body>
 7         <h1>This is a headline</h1>
 8         <p><a href="#" onclick="printSpans()">Print Span Tags to Console</a></p>
 9         <p>Here is <span id="foo">a paragraph  of</span> text.</p>
10         <p>This is a different paragraph <span id="bar">with some inline content</span>.</p>
11         <div>
12             This is some <span id="baz">inline</span> content not contained in a paragraph.
13         </div>
14         <script src="script.js"></script>
15     </body>
16 </html>
``` 
 
## Title Property  

HTML elements have a `title` attribute that controls the tooltip displayed when you hover over the element. This example uses the `querySelectorAll()` method to select all of the different tags in the HTML body and sets the title tag to be the name of the tag.

**Try hovering over any element to see what tag it is.))

```html
// index.html
 1 <html>
 2     <head>
 3         <title>Hello World</title>
 4     </head>
 5     <body>
 6         <h1>This is a headline</h1>
 7         <p><a href="#">Toggle Span Tags</a></p>
 8         <p>Here is <span>a paragraph  of</span> text.</p>
 9         <p>This is a different paragraph <span>with some inline content</span>.</p>
10         <div>
11             This is some <span>inline</span> content not contained in a paragraph.
12         </div>
13         <script src="scripts.js"></script>
14     </body>
15 </html>
 ```

## tagName

The `tagName` property returns the name of the tag. For example, if the tag is a `<div>`, then the element's `tagName` is "DIV". Note that the value from `tagName` is always uppercase. You can see this in action in the `title` example above.

```js
let divs = getElementsByTagName( "div" );

// This will log "div"
console.log( divs[ 0 ].tagName );
```

## children

The children property returns an array of the children of an element. This property is read-only. If you want to add children to an element, the best way is to use its append() or prepend() methods. This example lists the children of the document's body tag.

```
// index.html
 1 <!DOCTYPE html>
 2  <html>
 3     <head>
 4         <title>Hello World</title>
 5     </head>
 6     <body>
 7         <h1>This is a headline</h1>
 8         <p>Here is <span>a paragraph  of</span> text.</p>
 9         <p>This is a different paragraph <span>with some inline content</span>.</p>
10         <div>
11             This is some <span>inline</span> content not contained in a paragraph.
12         </div>
13         <p>One more paragraph for good measure.</p>
14         <script src="scripts.js"></script>
15     </body>
16 </html>
``` 

## parentNode  

An HTML document is a hierarchy of nested tags. When working with the DOM, we can see the parent node of an element using its read-only `parentNode` property.

This example finds the node with an id of "child" in the document and prints out its `parentNode`.

```html
// index.html
 1 <!DOCTYPE html>
 2 <html>
 3     <head>
 4         <title>Hello World</title>
 5     </head>
 6     <body>
 7         <div id="parent">
 8             <p id="child">This is the child</p>
 9         </div>
10         <script src="scripts.js"></script>
11     </body>
12 </html>
``` 

## Conclusion  

JavaScript makes it possible to create, update, and remove DOM Nodes, enabling us to create dynamic, flexible, and powerful web content.

---

# Creating Event Listeners  

An `event listener` allows a developer to respond interactively to events that occur on the DOM. In this lesson, you will learn how to respond to a couple of events using "event listeners" and how to respond to those events with callback functions.

## Selecting a DOM Element  

One of the methods available to us on the DOM object is `getElementById()`. We use this method to return a reference to an HTML element whose `id` we pass to it.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Selecting a DOM Element</title>
</head>
<body>
    <h2 id="header">Page Header!</h2>
    <script>
        // "header" is the name of our element's id
        var header = document.getElementById("header");
        console.log("Header: " + header); // Will log "<h2 id="header">Page Header!</h2>"
    </script>
</body>
</html>
```

Since our variable `header` is a reference to a DOM element, it adopts the methods available to other DOM elements. One of the methods available to use is the `addEventListener()` method.

## Event Listeners  

The `addEventListener()` method allows us to add a DOM event to our elements. These events will notify our code base that certain interactions have taken place.

Let's check out how to place an event listener onto a DOM element:

```
// index.html
 1 <!DOCTYPE html>
 2 <html>
 3     <head>
 4         <title>Selecting a DOM Element</title>
 5     </head>
 6     <body>
 7         <h2 id="header">Click Me and I'll change!</h2>
 8         <script src="index.js"></script>
 9     </body>
10 </html>
```

> Be sure to read the comments in the index.js file!

When adding an event listener, we need to pass in an `event type` and a `call back` function. In our example above, we are declaring the event type to be a `click` event. We are then passing in a `callback` function called ourCallBack(). This will execute when we trigger the event.

The `click` event type is one of many available to us.1 As an example, here is the same code but instead of "clicking" the text, we can "hover" it using the "mouseover" event.

```html
// index.html
 1 <!DOCTYPE html>
 2 <html>
 3     <head>
 4         <title>Selecting a DOM Element</title>
 5     </head>
 6     <body>
 7         <h2 id="header">Hover me and I'll change!</h2>
 8         <script src="index.js"></script>
 9     </body>
10 </html>
``` 

Some other common events are "mouseenter", "mouseleave", "mouseup", "mousedown", "blur", "focus", "load", "scroll", "keyup", "keydown", and "keypress". These events are only a few of *dozens* available through the browser. You won't want for events. However, you might need functionality that isn't available from the standard event types.

That's when we would want to create a `custom event` type.

## Custom Events  

This custom event discussion is somewhat advanced. It's certainly worth knowing. Evented programming environments like the browser are much easier to handle, if you embrace events and build your logic around them. However, they can be a little difficult to grasp. This foray into custom events is cursory and exploratory. Don't sweat it if you don't get everything the first time through.

When creating custom events, you will create a custom `event name` and have access to trigger this custom event. Once created, the syntax for linking your event to a node element is the same as before.

To create a custom event, we will need to use `JavaScript's CustomEvent API`. Let's first create the event:

```js
var customEvent = new CustomEvent("awesomeEvent");
```

The `CustomEvent Constructor` allows us to create a new custom event with relative ease. When creating a custom event, our first parameter takes in our custom event name. The second parameter is an object with customization options, which can be used to specify options the browser will use (like `bubbles` and `cancelable` below) or data that the browser will ignore that we can use (like `awesomeLevel` below).

```js
var customEvent = new CustomEvent("awesomeEvent", {
    detail: {
        awesomeLevel: "over 9000!"
    },
    bubbles: true,
    cancelable: false
});
```

Now that we've further customized our event, let's see how we can trigger our custom events.

```js
// Grab the reference of a node element
var headerElement = document.getElementById("header");

// Create your custom event
var customEvent = new CustomEvent("awesomeEvent", {
    detail: {
        awesomeLevel: "over 9000!"
    },
    bubbles: true,
    cancelable: false
});

// Trigger the event with the dispatchEvent() method.
headerElement.dispatchEvent(customEvent);
```

We can trigger our custom events and pass our custom data using the dispatchEvent() method.

## Additional Resources  

#### Lesson Footnotes

* 1: [Event References - MDN](https://developer.mozilla.org/en-US/docs/Web/Events)

---

# Running Code at Intervals or After a Delay  

JavaScript gives us some useful methods to delay and execute a set of code in intervals. We use these methods to delay a function from being executed or cause another to execute until stopped by a user's input. In this lesson, we will learn how to use the `setTimeout()` and `setInterval()` methods. We will also learn about their canceling counterparts: `clearTimeout()` and `clearInterval()`.

## Setting a Timeout  

The `setTimeout()` method allows you to set a time to wait until running a specific function. When we call `setTimeout()`, it generates a `TimeoutId`. A TimeoutId is used to cancel a `setTimeout()` by passing it to the `clearTimeout()` method.

### Usage  

The `setTimeout()` method will take a function as the first parameter. It then requires the amount to delay as a number (calculated in milliseconds) as the second parameter.

The basic syntax for setTimeout is as follows.

```js
setTimeout( [function], [time in milliseconds] );
```

The function takes two parameters. The first is the function that will be called when the timer runs out. This could be an anonymous function (a function with no name). The second parameter is the number of milliseconds (1000 milliseconds is one second) to wait before calling the function passed in the first parameter.

Let's look at an example `setTimeout()` method:

```js
1 setTimeout(function() {
2     console.log("This was delayed by one second!");
3 }, 1000);
```
 
You can read the above code as "Wait one second and then log 'This was delayed by one second'".

Now that we've simplified the syntax, we can pass in a function for our first parameter and set our delay to be one second (1000 milliseconds).

### clearTimeout()  

Whenever we call `setTimeout`, it returns a TimeoutId. We can store this id in a variable. This id is useful because it can be used to stop the timer and prevent the function from running. To cancel the execution of `setTimeout()`, we will call the `clearTimeout()` method and pass in our TimeoutId:

```js
1 let timeoutId = setTimeout( function() {
2     console.log( "This was delayed by one second!" );
3 }, 1000 );
4
5 clearTimeout( timeoutId );
6 
7 // Nothing will be logged because the timeout is canceled immediately.
```

## Setting an Interval  

The `setInterval()` method works in a different manner then the `setTimeout()` method. Instead of delaying the execution of a function, the `setInterval()` method will run a function at a given interval until canceled. The `setInterval()` generates an `IntervalId` like setTimeout() generates a `TimeoutId`.

### Usage  

We write the `setInterval()` method like our `setTimeout()` method by passing in a function as its first parameter. For the second parameter, we pass in a number (in milliseconds) that represents how often we'd like the function to execute.

Let's see what it looks like:

```js
setInterval(function() {
    console.log("This is executed every one second!");
}, 1000);
```

### clearInterval()  


To cancel a `setInterval()` we will want to use the `clearInterval()` method. Assuming we have a variable named `id` that has a IntervalId assigned to it, this is how we would cancel a `setInterval()`:

```js
clearInterval(id);
```

### Example of Executing Code Periodically  

```html
// index.html
<html>
    <head>
        <title>Set Intervals</title>
    </head>
    <body>
        <p id="container">Hello, World!</p>
        <input type="button" onclick="beginChange()" value="Start Changing"/>
        <input type="button" onclick="cancelChange()" value="Cancel Changing"/>
        <script src="index.js"></script>
    </body>
</html>
```

```js
// script.js
// Setting container equal to the html element with the id equal to "container"
var container = document.getElementById("container");
var id;

// Since "setInterval()" will generate an IntervalId,
// we will want to assign our variable "id" to
// the execution of setInterval().
function beginChange() {
    id = setInterval(changeSize, 1000); // "id" now equals the IntervalId generated by the setInterval().
}

// Our cancelChange() function will call the clearInterval().
function cancelChange() {
    clearInterval(id); // The clearInterval() function requires a IntervalId.
}

// This function takes our element with an id of "container" and changes it's border style.
function changeSize() {
    container.style.border = container.style.border === "3px solid red" ? "3px dotted blue" : "3px solid red";
}
```

> Take your time with this runnable program. There are comments in the index.js file to give a better explanation of the process, and a "Browser View" to see the code running.

## Conclusion  

Both `setTimeout()` and `setInterval()` have many practical uses when working on the web. As a developer, you might need to use them to delay or repeat the execution of functions. You should now be able to place these new methods in your "web developer's tool box".

## Additional Resources  

* [setTimeout - MDN](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout)
* [setInterval - MDN](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setInterval)
