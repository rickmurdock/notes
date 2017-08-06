# Breaking Down the Event Loop  

The JavaScript `Event Loop` is one of the more difficult concepts to grasp in the language. Essentially, it's made up of three parts:

* Call Stack

* Event Table

* Event Queue

In this lesson, we will break down the event loop and uncover the mechanisms used to construct it.

## The Call Stack  

It's important to remember that a **JavaScript Engine** is behind the scenes of your web browser. This engine has the job of processing our JavaScript functions. JavaScript is a single-threaded language, which means that it can only process one thing at a time.

This is where the `call stack` comes into play. You can think of the call stack like Pringles potato chips (Stay with me here).

Let's say you have an empty Pringles container and you want to place some chips inside, one at a time. The first chip you place in the container will be the last chip you eat since it's located at the bottom of the stack. The same is true for the last chip you place into the container, it will be the first chip you eat.

Let's look at an example to see how this looks:

```javascript
 1 function firstPotatoChip() {
 2     console.log("Eat me First!");
 3 }
 4
 5 function secondPotatoChip() {
 6     firstPotatoChip(); // Invoked second
 7     console.log("Eat me Second!");
 8 }
 9
10 secondPotatoChip(); // Invoked first
```

Within our call stack, the `secondPotatoChip()` is placed into the container first. `firstPotatoChip()` is placed on top of the `secondPotatoChip()` since it's called within the second function.

Visualization of the call stack:

| Call Stack (Top most will be executed first) |
| --- |
| firstPotatoChip() |
| secondPotatoChip() |

Once the top function executes, it will be removed from the stack and each function below will move up.

##nThe Event Table  

Since JavaScript is single-threaded, it's possible to block the call stack by executing a function that takes too long time to complete. This issue is solved by a mechanism called **asynchronous callback functions**. These types of functions don't get executed until a later time.

The method `setTimeout()` is a great example of an asynchronous callback function. Let's view an example:

```javascript
 1 function firstPotatoChip() {
 2     console.log("Eat me First!");
 3 }
 4
 5 function secondPotatoChip() {
 6     setTimeout(firstPotatoChip, 1000); // Initialized second
 7     console.log("Eat me Second!");
 8 }
 9
10 secondPotatoChip(); // Initialized first
11
12
13 // *Results*
14 // Logged: "Eat me Second!"
15 // (After one second) Logged: "Eat me First!"
```
 
When `secondPotatoChip()` executes, the call stack will look like this:

| Call Stack (Top most will be executed first) |
| --- |
| setTimeout() |
| secondPotatoChip() |


The callback function from the setTimeout(), `firstPotatoChip()`, is placed into a table that stores it until a certain event has occurred. This table is called the event table. Think of the event table like the table that holds your potato chips before you place them into the container.

# The Event Queue  

Currently, our callback function is stored in the event table and the `setTimeout()` has been removed from the stack. Once the specific event occurs it is placed into our **event queue**.

The **event queue** is a staging area for functions waiting to be invoked and moved over to the call stack. JavaScript engines are always checking to see if the call stack is empty. If the stack is empty, it will check the event queue to see if there are any callback functions in staging. If there is, then that function will be moved to the call stack.

Assuming both `setTimeout()` and `secondPotatoChip()` have cleared, our call stack should be empty. Our `firstPotatoChip()` is placed into the event queue after its event has occurred (which is to wait for one second). It is then passed to the call stack once the call stack has cleared.

| Call Stack (Top most will be executed first) |
| --- |
| firstPotatoChip() |

Now you understand why "Eat me Second!" was logged before "Eat me First!"

## Conclusion  

To sum it up, functions are placed into the call stack and executed based on which function is on top of the stack. Asynchronous callback functions are first stored in the event table before being staged in the event queue. This process can be a daunting, but this lesson should have given you a better understanding of the event loop.

There is also an excellent video on how all this works called, [What the heck is the event loop anyway?](https://www.youtube.com/watch?v=8aGhZQkoFbQ). We strongly suggest that you watch this video before moving on.

## Additional Resources  

* [The Event Loop - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)

---

# Using JavaScript Promises and Callbacks  

Sometimes, when we're programming an application, we want a bit of functionality to execute after something else happens.

Let's think of a scenario where that would be helpful. Consider this:

We have written an app that makes a request to another website (or database) for information. That information is pretty heavy and will take a couple of seconds to get back. We don't want our app to be held up for two seconds while we wait, but that's what can happen if the request isn't handled correctly.

Instead, we want to make our request for data and then *let the app execute uninterrupted* until the information comes back. Once the data gets back, we want to run the code that handles that data.

This process of making requests and waiting for the responses without blocking the rest of the app from running is called **asynchronous programming**.

This article introduces two concepts related to asynchronous programming:

* **Callback Functions** - Functions that are passed to other functions and run later.

* **Promises** - A proxy object that allows us to specify how to respond to the eventual success or failure of an asynchronous operation. You can think of it as: "I promise to do this if the operation fails, and I promise to do that if the operation succeeds."

This article will start by discussing callback functions and then demonstrate how to construct and use Promises.

# Callback Functions  

**In JavaScript, "callback functions" are functions passed as arguments to other functions. The callback is executed inside another function.** The idea is that the one function can make use of another and call it multiple times.

### Any Function Can Use Callbacks  

Any function can be defined to take a callback function as an argument. In JavaScript, functions can be passed around just like any other object or data, so the interpreter has no problem with a function being passed in as an argument and then being used within another function.

Let's look at a couple of examples.

### Callback Example - A Function that Calls Another Function

`calculate` is a function that takes one argument `callback`. `callback` is a function that gets executed as the last line of `calculate`. When we call `calculate`, we pass in a reference to the `print` function. Notice that we pass the *function* `print` instead of calling `print`. In other words, we are not performing a function call, like `print()`.

`print` is a function that logs whatever is passed into it along with the string "The sum is: ".

Within `calculate`, a calculation is performed with a few numbers and then stored in the `result` variable. That variable is then passed into the execution of `callback`. Remember that when we call `calculate`, we are passing it a reference to `print`, so within `calculate`, `callback` is `print`.

`print` is executed and passed `result`. It then prints the string and the value of `result`.

Note that we do not execute the `print()` function when calling the `calculate()` function. We pass `print` by reference. `print` is then called by calling the `callBack()` function inside the `calculate()` definition.

```javascript
 1 function print( num ){
 2   console.log( 'The sum is: ' + num );
 3 }
 4
 5 function calculate( callback ){
 6   let result = 4 * ( 35 / 7 );
 7   callback( result );
 8 }
 9
10 calculate( print );
```
 
In JavaScript, functions are first-class objects and can be passed by reference as an argument to a function or stored as a variable (just like any string, array or number). The use of callback functions is *really common* in JavaScript.

### Callback Example - Callbacks Can Make Your Code Simpler and More Flexible

In the following example, we define three functions:

* `printQuiet` takes one argument `thing` and prints it within the string "I whisper the number " + `thing` + "."

* `printLoud` takes one argument `thing` and prints it within the string "I SHOUT THE NUMBER " + `thing` + "!"

* `addTenToNumber` take two arguments. The first argument `num` is a number that will be added to 10. The second argument is a callback function.

With this setup, I can pass `addTenToNumber` either `printQuiet` or `printLoud` to change the output of `addTenToNumber`. I can add any number of callback functions to the mix without changing `addTenToNumber`.

```javascript
 1 function printQuiet( thing ){
 2     console.log( "I whisper the number " + thing + "." );
 3 }
 4 function printLoud( thing ){
 5     console.log( "I SHOUT THE NUMBER " + thing + "!" );
 6 }
 7 function addTenToNumber( num, callback ){
 8     let total = num + 10;
 9     callback( total );
10 }
11
12 addTenToNumber( 21, printQuiet );
13 addTenToNumber( 11, printLoud );
```

Let's add a third callback function to the mix that prints the number 10 times.

```javascript
 1 function printQuiet( thing ){
 2     console.log( "I whisper the number " + thing + "." );
 3 }
 4 function printLoud( thing ){
 5     console.log( "I SHOUT THE NUMBER " + thing + "!" );
 6 }
 7 function printTenTimes( thing ){
 8     for( let i = 0; i < 10; i++ ){
 9         console.log( "Printing " + thing + "!" );
10     }
11 }
12 function addTenToNumber( num, callback ){
13     let total = num + 10;
14     callback( total );
15 }
16
17 addTenToNumber( 21, printQuiet );
18 addTenToNumber( 11, printLoud );
19 addTenToNumber( 1, printTenTimes );
```

Using callback functions in this way can make your code much easier to maintain and can limit the amount of complex logic that you need to introduce into your code.

### Anonymous Functions as Callbacks  

In the previous example, we defined a function called `printQuiet`, `printLoud`, and `printTenTimes` to be used as callbacks for `addTenToNumber`. There is nothing wrong with that code, but it introduces three function names into the environment that don't necessarily need to be there.

Instead of defining a function with a name (`printQuiet`, `printLoud`, `printTenTimes`), we can instead create "anonymous functions". Anonymous functions are simply functions that don't have a name. It's hard to explain, so I'll show you an example.

### Callback Example - Anonymous Function Definition

In the following example, we've reworked the previous example to use anonymous functions instead of named functions. In this example, we define only one named function `addTenToNumber`.

*When we call `addTenToNumber`* we pass an anonymous function as the second argument. The functions get created *when passed into `addTenToNumber`*. They don't need a name because they only exist within the function call of `addTenToNumber`. This pattern is common if the passed function will only be used as a callback. Because it is only a callback, it doesn't need a name. It will only be called within the function to which it was passed.

In this example, `addTenToNumber` will run any callback using the callback parameter.

```javascript
 1 function addTenToNumber( num, callback ){
 2     let total = num + 10;
 3     callback( total );
 4 }
 5
 6 // Does the same as printQuiet
 7 addTenToNumber( 21, function( thing ){
 8     console.log( "I whisper the number " + thing + "." );
 9 } );
10
11 // Does the same as printLoud
12 addTenToNumber( 11, function( thing ){
13     console.log( "I SHOUT THE NUMBER " + thing + "!" );
14 } );
15
16 // Does the same as printTenTimes
17 addTenToNumber( 1, function( thing ){
18     for( let i = 0; i < 10; i++ ){
19         console.log( "Printing " + thing + "!" );
20     }
21 } );
```

### Event Listeners Use Callbacks  

Event listeners are a good illustration of methods that use callbacks. Event Listeners *exist* to respond to an event and then to execute callback functions.

### Callback Example - Event Listeners

In the following example, `myButton` has been selected from the web page. The function `handleClick` is defined to respond to the click. Then we define an event listener on `myButton`. Event listeners take two arguments: a string representing the event ("click"), and a function to call when the event occurs (`handleClick`).

`handleClick` is a callback function. By definition, it is a function that is *passed to another function as an argument* and then runs at a later time. The event listener on `myButton` runs `handleClick` whenever the "click" event occurs.

```javascript
var myButton = document.getElementById( "button" );

function handleClick(){
  console.log( "My Button was clicked" );
}
myButton.addEventListener( "click", handleClick );
```

The callback function pattern is *very* common. It's even more common for anonymous functions to be passed to an event listener. Let's rework our event listener example to use an anonymous callback function.

### Callback Example - Event Listener with an Anonymous Function

In this example, the named function `handleClick` is replaced with an anonymous function that does the same task.

```javascript
var myButton = document.getElementById( "button" );

myButton.addEventListener( "click", function(){
  console.log( "My Button was clicked" );
} );
```

You probably won't walk away from this article completely understanding callback functions, but you should at least be able to recognize them when you see them and to discuss the *idea of callback functions*.

## Promise  

A promise is a placeholder for a value that may or may not be known when the promise is created. A promise allows you to handle an asynchronous action's success or failure if/when the success or failure occurs.

Promises have several states: "pending," "fulfilled," and "rejected." A pending promise can either be fulfilled with a value or rejected with a reason (error). When a promise is either fulfilled or rejected, a handler is called by the `then()` method.

### The Promise Object  

The JavaScript `Promise` object is used for asynchronous operations. It is a constructor and should be instantiated with the `new` keyword, passing in an executor at the time of instantiation.

You may not have yet seen the `new` keyword. It is used to create new objects. `Promise` is an object that already has properties and methods attached to it. When we write `new Promise` we are telling the interpreter to create a new object that *is a Promise*. The new object is an "instance" of `Promise`, with all of the same properties and methods.

When you use `new`, the function that gets called is referred to as a "constructor," because it is being used to construct an instance of itself. In this case, `new Promise` is a constructor for `Promise` instances.

### Executor  

An executor is a callback function that is passed to the `Promise` object constructor when it gets instantiated. It has two arguments, `resolve` and `reject`.

The executor is run immediately by the `new Promise` before the `Promise` returns the created object. `resolve` and `reject` are functions to be called by the promise.

* `resolve` is called when the asynchronous operation succeeds.

* `reject` is called when the asynchronous operation fails.

The executor usually begins some asynchronous work and then calls the `resolve` or `reject` function when complete.

If an error occurs, the executor rejects the promise. In this case, the return value of the executor is ignored.

In this example, we'll simulate an asynchronous operation (usually an XHR operation or an API call) with a `setTimeout()`. `setTimeout` will wait to run a callback function. in this example, `setTimeout` waits 200 milliseconds and the runs `resolve` with the string "It Worked!"

The promise object has a then function, which takes a callback to run when the operation is successful.

The following two examples do the same thing. One uses named functions, and one uses anonymous functions.

```javascript
 1 function executor( resolve, reject ){
 2     let respond = function(){
 3         resolve( "It Worked!" );
 4     };
 5     setTimeout( respond, 200 );
 6 }
 7 function success( successMessage ){
 8   console.log( successMessage );
 9 }
10
11 //create the promise
12 let myPromise = new Promise( executor );
13
14 // Add a callback function
15 // to take the message from resolve
16 myPromise.then( success );
```

```javascript
1 let myPromise = new Promise( function( resolve, reject ){
2     setTimeout( function(){
3         resolve( "It Worked!") ;
4     }, 200 );
5 });
6
7 myPromise.then( function( successMessage ){
8    console.log( successMessage );
9 });
```

More often than not, you will see the second version, using anonymous functions, but both versions do the same thing.

## Conclusion  

It is possible to wait until an asynchronous function is executed to call another function or return a success/failure message with the JavaScript `Promise` object.

It is also possible to call a function inside another function with a callback.

Using these two techniques, we're ready to take on API calls and implement more complex functionality.

### References  

* [Promise - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

---

# Using The Fetch Library To GET & POST  

Your next cool app will depend on two things: getting and posting data.

Until recently, developers relied on `XMLHttpRequest` to do this. Even though this works, the code can be messy. They also relied on listeners and events. Adding extensive logic to this kind of request has the potential for hard-to-read code. In contrast, the `Fetch API` provides a much cleaner and simpler interface. This is a relatively new technology, so you'll want to always check the browser support required on your project before using.

## Polyfill & Isomorphic  

### Polyfill

The term *polyfill* in reference to JavaScript is a browser fall-back that allows the code you write for modern browsers to also work in older browsers.

You can keep up with the current support of `fetch` over at [caniuse.com](https://caniuse.com/#feat=fetch). Since there are some older browsers that you *might* need to support, you should be made aware of the [Fetch Polyfill](https://github.github.io/fetch/). You won't need it during this course, since you are using a modern browser (Chrome), however it's something you should be aware of.

### Isomorphic

Since Fetch is part of the `window` object and has become so popular, there is a NPM package that allows you to use the Fetch API in both the browser as well as inside of a NodeJS environment.

You can read more about [Isomorphic Fetch here](https://github.com/matthew-andrews/isomorphic-fetch).

## Using Fetch  

The `window.fetch()` method has one mandatory parameter, the "URL" from where the data will be fetched. Once a **resource** is fetched, it returns a Promise, which resolves to the **response** pertaining to the request.

The `Fetch API` also has a number of methods available to define and handle the body of the `response`.

### Simple example  

Compared to an `XMLHttpRequest`, `fetch` is much simpler to use.

#### Example

```javascript
fetch(url, options).then(
  function(response) {
    // handle HTTP response
  },
  function(error) {
    // handle network error
  }
);
```

## Request Options  

When making a `fetch` request, we can pass options ranging from `method` to `credentials`.

| Option |	Description |
| --- | --- |
| `method` |	(String) HTTP request method. **Default:** "GET" |
| `body` |	(String, body types) - HTTP request body |
| `headers` |	(Object, Headers) - Default: {} |
| `credentials` |	(String) - Authentication credentials mode. **Default:** "omit" |

### Promises  

ES6 introduced new `Promises` functionalities. One of these is `.then()`. This method replaces `callbacks` and `events`. It handles success and failure cases using two callback functions. These must be passed in as arguments. `.then()` basically says, "once we have data in the queue (`promise`), do x, y, and z with it."

#### Example

```javascript
let url = "https://www.theironyard.com";

fetch(url).then(processStatus);
```

When a resource is **fetched**, it returns a **promise**. One very important thing about **promises** is that *no matter* the response from the server (including 400), they will be *resolved successfully*. This means that we must do something with the **response** we get back through the **promise**.

#### Example

In the example below, we deal with the response by using a conditional statement. If it returns an error, it returns. If successful, we process the data. Focus on the conditional statement.

```javascript
fetch("https://api.github.com/users/theironyard")
  // Data is fetched and we get a promise.
  .then(
    // The promise returns a response from the server.
    function(response) {
      // We process the response accordingly.
      if (response.status !== 200) {
        console.log(response.status);
        return;
      }
      response.json().then(function(data) {
        console.log("Here is the data:", data);
      });
    }
  )
  .catch(function(err) {
    console.log("Fetch Error :-S", err);
  });
```

### We got a response. now what?  

It is important to note that the `response` we get back is not JSON, but an object. This means that we can use certain methods to manipulate the data we get back.

There are a handful of methods you can use, but the one that is most commonly used is `.json()` method. This method resolves the response with an object literal containing the JSON data.

#### Example

```javascript
response.json().then(function(data) {
  // Do something with your JSON.
  // For example, a 'for' loop.
});
```

### GET  

`GET` is the default `fetch` method. There is no need to declare this in the request.

#### Example 1

In this simple `GET` request we `fetch` some cool data from NASA.

```javascript
fetch(
  "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo"
)
  .then(function(response) {
    if (response.status !== 200) {
      console.log(response.status);
      return;
    }
    response.json().then(function(data) {
      console.log("test", response.url);
    });
  })
  .catch(function(err) {
    console.log("Fetch Error :-S", err);
  });
```

*Run the example above in Codepen and checkout the console.*

#### Example 2

The example below is a bit more complex. Notice that in the conditional statement we modify the `header` request.

> We can modify header requests using the following methods: `append`, `has`, `get`, `set`, and `delete`.

```javascript
function fetchGET(url) {
  fetch(url)
    .then(function(response) {
      if (
        response.headers.get("content-type").indexOf("application/json") !== -1
      ) {
        // checking response header
        //.json() parses the response.
        return response.json();
      } else {
        throw new TypeError(
          'Response from "' + url + '" has unexpected "content-type"'
        );
      }
    })
    .then(function(data) {
      console.log('JSON from "' + url + '" parsed successfully!');
      console.log(data);
    })
    .catch(function(error) {
      console.error(error.message);
    });
}

fetchGET("https://api.github.com/users/theironyard");
```

*Run the example above in Codepen and check out the console.*

### POST  

A `fetch POST` request is fairly easy to set up. Simply provide the "URL" and declare the POST method. In most cases, you will also have to provide a `header` so that the request is successfully processed by the server.

#### Example

The following is a basic example of a `POST` request using `fetch`.

```javascript
fetch(url, {
  method: "post",
  headers: {
    "Content-type": "application/x-www-form-urlencoded; charset=UTF-8"
  },
  body: "body content goes here"
})
  .then(json)
  .then(function(data) {
    console.log("Request succeeded with JSON response", data);
  })
  .catch(function(error) {
    console.log("Request failed", error);
  });
```

#### Example 2

The following is a more complex example. Here we execute the `submitGist()` function to post data to the Github Gist API.

```javascript
function createGist(opts) {
  console.log("Posting request to GitHub API...");
  fetch("https://api.github.com/gists", {
    method: "post",
    body: JSON.stringify(opts)
  })
    .then(function(response) {
      return response.json();
    })
    .then(function(data) {
      let url = data.html_url;
      console.log("Created Gist:", url);
      alert(url);
    });
}
function submitGist() {
  let content = "today is hot";
  if (content) {
    createGist({
      description: "Fetch API Post example",
      public: true,
      files: {
        "test.js": {
          content: content
        }
      }
    });
  } else {
    console.log("Please enter in content to POST to a new Gist.");
  }
}
submitGist();
```

## Conclusion  

The `Fetch API` is a new technology that simplifies HTTP requests. It offers a powerful and yet uncomplicated process over `XMLHttpRequests`.
