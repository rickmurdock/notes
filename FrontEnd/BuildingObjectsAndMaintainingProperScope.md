# Date and Math Global Objects  

The `Math` and `Date` global objects are great tools for you to use as you progress as a developer. In this lesson, we are going to take a nose-dive into each so you can master their uses.

## Date  

The `Date` can be used to generate an object that represents a particular date or time, or a length of time since some previous date. Each instance created using the `Date` object is calculated based upon the number of milliseconds that have occurred since 1 January, 1970 UTC.

> We won't go into detail here. If you're interested in learning why this date is important, check out Unix Time.

### The Date Constructor  

The `Date` object is a constructor, so you can call it by initializing the `new` keyword and calling on the date object. You can instantiate a `Date` object in four different ways.

```js
new Date();
new Date(value);
new Date(dateString);
new Date(year, month[, date[, hours[, minutes[, seconds[, milliseconds]]]]]);
```

* **value**: This `integer` represents the number of milliseconds since 1 January 1970 00:00:00 UTC.

* **dateString**; This `string` represents a date. See `Date.parse` for valid formats for this string.

* [**year, month, date, hours, minutes, seconds, milliseconds**]: These `integers` correspond to the namespace they represent.

> Remember, every parameter we pass into the `Date` object is an integer except `dateString` (Ex: "November 6, 1987").

### Creating Date Objects  

We've seen how to initialize the `Date` object, now let's create some dates!

The first example is a simple usage of the `Date` constructor:

```js
// Example #1
var date = new Date();

console.log("New Date: ", date); // current date and time in Unix time
```

Now let's pass in an integer that will represent the number of milliseconds since 1 January 1970 00:00:00 UTC:

```js
// Example #2
var date = new Date(563200000000);

console.log("New Date: ", date); // 1987-11-06T12:26:40.000Z
```

As you can see, the logged date is `1987-11-06T12:26:40.000Z`. Let's try initializing the `Date` with a string:

```js
// Example #3
var date = new Date("November 6, 1987");

console.log("New Date: ", date); // Fri Nov 06 1987 00:00:00 GMT-0500 (EST)
```

Finally, let's pass in integers for the year, month, and day into our `Date` object:

```js
// Example #4
var date = new Date(1987, 11, 6);

console.log("New Date: ", date); // 1987-12-06T00:00:00.000Z
```

Did you notice the logged date?: `1987-12-06T00:00:00.000Z`

The month integer takes a value for month from `0 - 11`, with `0` being equal to January. When we passed in the month as `11`, we were actually passing in the month for December. Take note of this or you might find yourself a month early or behind!

### Adopting Methods from the Date Object  

All instances of `Date` inherit the methods from `Date.prototype`. These methods are useful for returning information about your created dates. You'll be able to edit and view the created date object using these methods, so make sure you head over to the **Additional Resources** section below to view all the methods available to you.

## Math  

The `Math` object functions differently than our `Date` object. Unlike the `Date` object, the `Math` object isn't initialized through a constructor, but by accessing the properties and methods located on the object itself. The methods located on the `Math` object, as you might expect, relate to common mathematical functions and equations.

### Examples  

Let's jump straight into some common methods you will use:

### Math.pow()  

```js
// Returns base to the exponent power, that is, base^exponent.
Math.pow(6, 2); // 36
Math.pow(3, 3); // 27
Math.pow(-7, 2); // 49 (squares are positive)
Math.pow(-7, 3); // -343
```

### Math.round()  

```js
// Returns the value of a number rounded to the nearest integer.
Math.round(25.49); // 25
Math.round(20.5);  // 21
Math.round(19);  // 19
Math.round(-25.5);  // -25
Math.round(-25.51); // -26
```

### Math.ceil()  

```js
// Returns the smallest integer greater than or equal to a number.
Math.ceil(.89); // 1
Math.ceil(5); // 5
Math.ceil(20.001); // 21
Math.ceil(-.98); // 0
Math.ceil(-10.678); // 10
```

### Math.floor()  

```js
// Returns the largest integer less than or equal to a number.
Math.floor(25.46); //  25
Math.floor(25.99); //  25
Math.floor(-25.46); //  26
Math.floor(-25.99); //  26
```

### Math.sqrt()  

```js
// Returns the positive square root of a number.
Math.sqrt(9); // 3
Math.sqrt(1); // 1
Math.sqrt(0); // 0
```

> Make sure to visit the Math - MDN link to view all methods available on the `Math` object.

## Conclusion  

Your `Math` and `Date` global object usage will be a common occurrence throughout your coding career. If you take the time to look through their methods and properties, you will be equipped to tackle issues and provide better solutions when dealing with date, time, and mathematics.

## Additional Resources  

* [The Date Object- MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)
* [The Math Object- MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)

---

# Math and Concatenation Operators  

The math and concatenation operators in JavaScript are some interesting and surprisingly entertaining tools. They are used to implement simple arithmetic like `2 + 2` or used to join together two strings into one, like `"Hello, " + "World!"`. This lesson will have you using both operators to do some pretty useful (and not useful) things.

Computers excel at arithmetic. After all, that's what they're designed to do! JavaScript comes with a standard compliment of mathematical operators.

Operators are special bits of syntax that take action on operands. For example, we use arithmetic operators to perform mathematical operations such as addition, multiplication, subtraction, and division.

### The Plus Operator  

**Operator**: `+`

**Syntax**: `<firstValue> + <secondValue>`

**Returns**: The sum of the two values.

The plus operator can be use for addition and concatenation. Concatenation means to take two values and return a string by adding them together using the `+` operator. Let's view a chart to see what type of interaction takes place when "adding" two different types:

| Type One | | Type Two |	Addition |	Concatenation	| Return Value |
| :---: | --- | :---: | :---: | :---: | :---: |
| Number	| `+` |	Number |	x	|	| Number |
| Boolean |	+	| Number| 	x	| |	Number |
| Boolean |	`+`	| Boolean	| x	|	| Number |
| Number |	+	| String	|	| x |	String |
| String |	`+`	| String	|	| x |	String |
| Boolean |	+ | String	|	| x |	String |

As we can see from this table, using the `+` operator with two different types of values can return very different results.

### Examples: The Plus Operator

Now, let's look at a practical example to better understand the power of concatenation. This should also reinforce the importance of using the `+` operator, accurately.

```js
// Our variables:
var one = 1;
var two = 2;
var three = "3";
var four = "four";
var five = "five";

// Inside the console.log(), insert two variables of your choice and use the `+` operator to either return a sum or concatenate.

// This will log a Number of 3:
console.log( one + two );

// This will log a string "fourfive":
console.log( four + five );


// This will log a string "3five":
console.log( three + five );
```

The main thing to keep top of mind is that if the "+" operator is used with numbers it performs addition. If it is used with strings it performs concatenation.

### The Minus Operator  

**Operator**: `-`

**Syntax**: `<firstValue> - <secondValue>`

**Returns**: The difference of the two values.

```js
console.log(9 - 5);
console.log(0 - 1873902369);
console.log(3292379.368 - 5.000001);
```

### The Multiplication Operator  

**Operator**: `*`

**Syntax**: `<firstValue> * <secondValue>`

**Returns**: The product of the two values.

The multiplication operator multiplies the value to the left of the `*` symbol by the value on the right and returns the result.

```js
console.log(9 * 5);
console.log(0.2571 * 1896725);
console.log(0.0000000001 * 7);
```

> The `*` operator will return a number if used with a number and a "number as a string" e.g. `3 + "3`". Even though JavaScript will allow this sort of thing, avoid it if you are able. You should only be doing math with numbers.

### The Division Operator  

**Operator**: `/`

**Syntax**: `<firstValue> / <secondValue>`

**Returns**: The quotient of the two values.

The division operator divides the value to the left of the `/` symbol by the value on the right and returns the result.

```js
console.log(9 / 3);
console.log(0.96342801 / 123);
console.log(16129849702 / -1234);
```

> The `/` operator will return a number if used with a number and a "number as a string" e.g. `3 - "2"`. Again, good to know, but don't do it. This type of operation can be the source of bugs that are extremely difficult to chase down.

### Remainder operator  

**Operator**: `%`

**Syntax**: `<a value> % <another value>`

**Returns**: The remainder left when dividing the two values.

Remember long division from elementary school?

![Long division - the bane of 4th graders everywhere.](./images/dec-long-division.gif)

The remainder operator, often called the *modulo operator* or *mod*, returns the remainder left over when dividing the value to the left of the `%` symbol by the value to the right.

Imagine you run a breakfast restaurant that makes three-egg omelets and have 116 eggs. You want to know how many eggs will be left over if you sell all the omelets you can make.

```js
console.log( 116 % 3 );
```

This tells us there will be two eggs left over once we've made all of our three-egg omelets.

Here are a few more examples to explore:

```js
console.log(9 % 3);
console.log(14 % 5);
console.log(16129849702 % -1234);

console.log( 0.14 % 0.03 );
```

## Order of Operations  

Do you remember [PEMDAS](https://www.mathsisfun.com/operation-order-pemdas.html) from math class? PEMDAS is an acronym used to remember the order that operations must be performed in a mathematical expression. JavaScript follows the same rules for arithmetic. This would be:

* Parenthesis

* Exponents

* Multiplication (and Modulo) and Division (left to right)

* Addition and Subtraction (left to right)

Using this, we know that `42 - 8 * 34 = -230` because the `*` multiplication operator has higher precedence than the - subtraction operator. `8 * 34 = 272` and `42 - 272 = -230`. If we didn't follow the correct order of operations, we might have incorrectly found the answer to be `1156`.

You may remember a mnemonic from school: **P**lease **E**xcuse **M**y **D**ear **A**unt **S**ally.

### Parenthesis  

Just like in math, you can use parenthesis in JavaScript to explicitly indicate order of operations. JavaScript treats each set of parenthesis as a separate expression. JavaScript evaluates parenthesis starting from the innermost pair, and works outward. Each set of parenthesis is replaced with its evaluated value before the outer set of parenthesis are evaluated.

Imagine we have this expression in JavaScript:

```js
(42 - (8 + 24)) * ((7 + (4 + 3)) / 2)
```

JavaScript would step through this like so:

1. `(42 - (8 + 24)) * ((7 + (4 + 3)) / 2)`

2. `(42 - 32) * ((7 + 7) / 2)`

3. `10 * (14 / 2)`

4. `10 * 7`

5. `70`

### Arithmetic Operator Precedence  

Let's take a look at an example expression and determine how JavaScript would evaluate it. This is the same expression as before, just without parenthesis:

```js
42 - 8 + 24 * 7 + 4 + 3 / 2
```

JavaScript starts by looking for multiplication, division and remainder operators. Since JavaScript reads left to right, the first operator found is a `*` multiplication operator. The value immediately to the left of the `*` operator is 24 and to the right is 7. JavaScript will evaluate that first, like so:

```js
42 - 8 + (24 * 7) + 4 + 3 / 2
```

... and evaluates to:

```js
42 - 8 + 168 + 4 + 3 / 2
```

Continuing to the right, JavaScript next finds the `/` division operator. The value to the left of the `/` operator is 3 and to the right is 2. JavaScript will perform that expression next, like so:

```js
42 - 8 + 168 + 4 + (3 / 2)
```

... and evaluates to:

```js
42 - 8 + 168 + 4 + 1.5
```

Having completed evaluating multiplication, division and remainder operators, JavaScript starts looking for addition and subtraction. Again, this is done left to right. JavaScript will first find the `-` operator followed by an `+` operator followed by two more.

```js
(42 - 8) + 168 + 4 + 1.5
```

... and evaluates to:

```js
34 + 168 + 4 + 1.5
```

The pattern continues.

```js
34 + 168 + 4 + 1.5
```

... becomes ...

```js
(34 + 168) + 4 + 1.5
```

... becomes ...

```js
202 + 4 + 1.5
```
... etc...

```js
(202 + 4) + 1.5
```

... etc...

```js
206 + 1.5
```

At last we reach the final expression and evaluate that to 207.5! Therefore, the original expression...

```js
console.log( 42 - 8 + 24 * 7 + 4 + 3 / 2 );
```

... is logically the same as this expression with parenthesis:

```js
console.log( ( ( ( 42 - 8 ) + ( 24 * 7 ) ) + 4 ) + ( 3 / 2 ) );
```
 
My advice to you is to always make judicious use of parenthesis. Be explicit in your expressions. This not only helps reduce difficult to debug errors, it also makes your code easier for humans to understand.

## Conclusion  

JavaScript has some pretty unique functionality with its arithmetic operators. It's vitally important to understand what they "return" when used with different type of values. Now that you have a better appreciation of JavaScript arithmetic operators, watch this [educational and down-right hilarious video](https://www.destroyallsoftware.com/talks/wat)!

## Additional Resources  

* [Arthimic Operators - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators)

---

# The Global Scope and Compartmentalized Code  

In JavaScript it is important that you write effective code that serves its purpose but does not override global variables upon execution. We'll examine how to write specific compartmentalized code that that doesn't make changes to the entire project or pollute the global scope.

Proper isolation of variables is key, especially as your code becomes longer and more complex. Let's dial the microscope back two notches and first examine the `window` object, as well as the term "scope". We'll then take a look at some ways to construct function expressions that are self-contained, and therefore do not affect the global scope.

## The Window Object  

The `window` object represents a browser's window. It is the petri dish that all global items grow inside of, including:

* Global JavaScript objects

* Global functions

* Global variables

> Global variables are properties of the window object whereas global functions are methods of the window. The `window.document` property points to the DOM document loaded in that window.

## Scope  

Scope refers to the accessibility of functions and variables within a part of code during runtime, essentially determining the visibility of variables in code. There are two types of scope: `global scope` and `local scope`.

A variable that's been defined *outside* of a function is considered to be in the `global scope`, while a variable defined *inside* of a function is in the `local scope`. In other words, **all** variables in a JavaScript project are in the `global scope` unless they're defined inside a function.

```js
var pet = 'dog';
```

The variable "pet" is in the global scope, and may be used within any other function. For example:

```js
var pet = 'dog';

function printPet() {

  var otherPet = "cat"; 

  console.log(pet);
}

printPet();
```

...logs "dog" because it was defined in the global scope.

The variable `otherPet` is defined **inside** the function, which means that its within the function scope of `printPet` and cannot be accessed except by the function or other items inside the function. Lets modify our previous code sample to see how access to `otherPet` is handled.

```js
function printPet() {

  var otherPet = "cat";
}

// This line of code can't reach `otherPet` because
// `otherPet` is scoped to the function
// This log function is globally scoped and can't see into the function
console.log( otherPet );
```

You should see a reference error because the log statement and the variable are in different scopes and the log statement can't see into the function. So, how can we access that inner scoped data? Lets adjust our code one more time to see what we can do.

```js
function printPet() {

  var otherPet = "cat";
  return otherPet;
}

// This line of code still can't reach the inner variable
// but since the function call returns the value, then we can 
// indirectly access internally scoped data via the function call
console.log( printPet() );
```

This last example illustrates a really important concept: Protecting data from outside influences. The variable `otherPet` is protected. Nothing can get to it to change it. We can call the function, but all that does is return the value stored in the variable, we don't have to worry about some other programmer accidentally creating a variable called otherPet that overwrites ours.

So, lets extend that concept a bit farther. What if we put our whole program inside a function? Then everything we need will be protected in a single function, which we can call to get the program started. Lets see what that might look like:

```js
//We define a function to hold all of our code
function runApp(){

  // Put all of the good stuff in this function,
  // Data, functions, algorithms, etc.
  var importantData = {
    things : 1498623,
    count : 23928375,
    stuff : 9235
  };

  var formatStrings = function( str ){ /* code */ };

  /* More code */
}

/* Call the function to get it running */
runApp();
```

So, lets consider what we've done. All of our code is protected inside of a function. Cool. We can modify the contents of runApp till our hearts are content without worrying that someone might overwrite `importantData` or `formatString()`. Now, lets talk about the down sides.

1. `runApp()` is a named function that can accidentally be accessed by other applications or programmers.

2. The `runApp()` function is in the Global scope. Now, instead of individual pieces being in danger, our entire app is in danger.

3. We have to explicitly call `runApp()` to get the ball rolling.

How can we have our cake and eat it too? How can we protect our data, prevent pollution of the global scope, and guarantee that other programs and programmers can't accidentally upset our code?

## Immediately-Invoked Function Expressions  

An Immediately-invoked-function-expression, or an IIFE (pronounced like "iffy") is essentially exactly what it sounds like: a function used in JavaScript that runs right after being defined. They allow you to isolate a script environment and they don't pollute the global scope by overriding global variables.

The syntax may look a little strange, but its not so bad when you break it down. Essentially, an IIFE is an anonymous function, which is awesome because its unnamed. That checks off the first issue we had with our previous example. The anonymous function is wrapped in parenthesis, with another set of parenthesis before the semi colon. These parenthesis tell the anonymous function to run immediately.

The basic template for an IIFE is as such:

```js
(function () {
    // main content
})();
```

Any variable declared inside that IIFE is safe from being unintentionally accessed elsewhere in your code.

```js
(function () {

  var ice = 'cream';
  console.log(ice);

})(); //logs "cream"

console.log(ice); //logs ReferenceError: ice is not defined
```

You can see from the above example, the variable "ice" is contained within the IIFE so the second attempt to log "cream" returns an error because it is outside of the expression.

We can even test how we've limited the scope by using the same variable name and console logging inside and outside of the IIFE. Its not a good practice to duplicate variables in this way, but its useful here to demonstrate the behavior of IIFEs.

```js
var ice = 'cream';

(function() {
  var ice = 'skate'

  console.log(ice); // -> This logs skate
})();

console.log(ice); // -> This logs cream
```

### Passing Arguments into an IIFE  

Another benefit of immediately invoked expressions is that arguments can be passed through them as follows:

```js
var ice = 'ice';

(function (innerIce) {
    console.log(innerIce);
})(ice);
```

This logs ice, as it's been passed through the IIFE. Notice that the `ice` variable has been passed into the last set of parenthesis. That value is then passed through to the anonymous function. The anonymous function receives it as a variable called `innerIce` and logs it.

## What to do with this knowledge?  

Now that you know how to write IIFEs, you should try to write all of your JavaScript code in IIFEs.

```js
// No code out here

(function (){
    // All my code goes in this
    // anonymous function
})();
```

## Conclusion  

When asked what the greatest challenge of writing code is, a coder will often tell you that it's coming up with names; names for variables, functions, DIV's, and much more. As a piece of code grows, more names are required and that creates a higher risk of a single piece of code throwing off the entire project. If you're not careful, a name may be accidentally used in more than one space within the global scope. The compartmentalization of code that Immediately-invoked function expressions facilitates is a great way to prevent this error.

### References  

* [Window - MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window)

* [Scope - MDN](https://developer.mozilla.org/en-US/docs/Glossary/Scope)

* [Properly Isolate your Variables in JavaScript](http://www.nicoespeon.com/en/2013/05/properly-isolate-variables-in-JavaScript/)

---

# Hoisting Behavior In JavaScript  

Variable and function hoisting is an interesting (sometimes odd) behavior that takes place in JavaScript. It is very important to understand this mechanism when declaring new variables and functions to help elevate `TypeErrors`.

In this lesson, we will go over what happens when our JavaScript functions and variables are parsed and how hoisting can create confusion.

## Scoping In JavaScript  

`Scope` is an important concept in the JavaScript language and has a direct relationship with hoisting. A scope is like a bubble that has `blocked` code from anything outside of it. When you create a new function in JavaScript, you create a new `scope`.

Why is this important?

## Variable Hoisting  

When the JavaScript interpreter executes your code, it will hoist all functions and variables to the top of their containing scope.

Lets pretend that you've written this little snippet of code. It makes sense to you and when you call the function, it does what you want. Unfortunately, what you write is not exactly what the JavaScript interpreter sees, when it runs this code:

```js
function foo() {
    bar();
    var x = 1;
}
```

The interpreter sees your code and notices that you declared a variable after you called `bar()`. The interpreter doesn't like that as a best practice and it rewrites your code to be as follows, before it runs the code:

```js
function foo() {
    var x;
    bar();
    x = 1;
}
```

As you can see, the variable declaration was pushed to the top of the scope. In this example, the function `foo()` is the scope. This is called hoisting. Most of the time, it won't get in the way of your code. However, it does imply a very good practice with data: You should just go ahead and declare your variables at the top of their scope. JavaScript is going to do it anyway, so you should at least make it obvious what is occurring.

**Also realize that the assignment portion of the variable declaration was not hoisted.** In our example above, only the name `var x;` was hoisted, `x = 1;` was not hoisted.

## Function Declaration Hoisting  

Though variable names are hoisted to the top, it is important to note that functions behave different. If you define a function within another function's scope, entire function body will be hoisted.

Let's check out an example of hoisting with a function declaration:

```js
function foo(){
    // This code will run even though the function declaration
    // happens later in the code. Why?
    bar();

    function bar(){}
}
foo();
```

Again, the interpreter sees the function declaration following the function call and decides to rewrite things before executing the code. It hoist the entire `bar()` declaration to the top of the scope.

```js
function foo(){

    // this function declaration has been 
    // hoisted to the top of foo()
    function bar(){}

    bar();
}

foo();
```

Okay, declarations get completely hoisted, but what about function expressions? If you assign a function to a variable, the variable name will behave similarly to a normal variable.

## Function Expression Hoisting  

First, notice that the bar function is actually an anonymous function assigned to a variable `bar`. This is called a function expression. We can still call the function in the same way i.e. `bar()`, but this alternate way of creating named functions changes how hoisting is managed. Lets look at the first block of code. If this code were run, it would throw a reference error. Hoisting is still happening, but function expression act like variable hoisting. The declaration is hoisted, but not the assignment.

```js
function foo(){
    // This code will throw an error. Why?
    // I thought functions got hoisted?
    bar();

    var bar = function(){};
}

foo();
```   
 
The following snippet demonstrates what the interpreter will hoist in this case. It has hoisted `var bar;` but not `bar = function(){};` This means that when bar() is called, it has yet to be assigned a value.

```js
function foo(){

    // this function declaration has been 
    // hoisted to the top of foo()
    var bar;

    bar();

    bar = function(){};
}

foo();
```

> Whether you choose to use function declarations or expressions its clear that this behavior (hoisting) can be a clear source of confusion. **The lesson to take away: Always define your functions before using them.**

## Conclusion  

This lesson has given you the knowledge to master the incredible mechanism that is `hoisting` in JavaScript. If you use common practice and declare your variables at the top of a given `scope`, you will find that your hoisting errors will be at a minimum.

## Additional Resources  

* [Hoisting - MDN](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)

---

# Using Bracket and Dot Notation to Access Object Literals  

Everything in JavaScript is an object. Functions are objects. Arrays are objects. Numbers, strings, etc. are all objects. When you do anything in JavaScript, you are manipulating objects.

The idea of objects can be confusing to most folks, so lets take a second to break it down conceptually.

Imagine that you are trying to represent your cell phone. Lets say, an iPhone. You want to describe it to people. Lets just imagine some of the things that we might say about the phone, and lets also try to be organized about how we describe the phone. We are going to describe two categories of information about your phone.

1. **We'll describe what your phone "is".** Meaning, what it looks like, what descriptors we can apply to it e.g. color, durability, size, weight.

2. **We'll describe what it "does".** What behaviors does it have? What commands can we give it? What does it do on its own? e.g. Make call, click home button, increase/decrease volume.

We could go into great detail about the phone, but lets just stick to a few key details:

The phone "is":

* black

* 5oz

* rectangular

* minimalist

* etc.

The phone "does":

* make calls

* increase volume

* decrease volume

* go back to home screen

* turn on

* turn off

* etc.

This is great. We have a good list of descriptors and behaviors about the phone. Now, how can we represent this phone as code? **The answer: an object.** There are several ways to create objects in JavaScript, but we'll model our phone as an Object Literal, since thats what we'll discuss in this lesson. Below, you'll see the data rewritten as a JavaScript object. In this lesson, we'll talk a lot about how to access data from objects, but it pays for you to see what they are for.

```js
var iPhone = {
  color : "black",
  weight : 5,
  shape : "rectangular",
  style : "minimalist",

  makeCalls : function(){},
  changeValume : function( amount ){},
  goToHomeScreen : function(){},
  on : function(){},
  off : function(){}
};

console.log( iPhone );
```

In a lot of ways, this object literal is the same thing as our list above. The formatting and syntax is a little different, but in essence its the same. The items that our phone "is" became data. The items that our phone "does" became functions (sometimes called 'methods' when part of an object). The object (everything in between the curly braces) is the object. The object is the encapsulation of all of this data and behavior into one thing, accessible via the variable `iPhone`.

Now that we've talked in general about object literals, lets dig in a little deeper.

## Object Literals  

"Object literal" really just refers to the way that a specific object is written. It requires a specific syntax that differentiates it from other objects like arrays and strings. Code blocks containing object literal syntax help cut down on the number of global variables declared, which means cleaner code and less chance for error.

Lets start as simply as possible. We'll define an Object with no members and talk about a couple ways to create new members, and access and edit those member. The following code sample declares a variable demo that holds an empty object. Objects are declared using the curly braces. All members of the object are listed as comma separated, key/value pairs. In this case we have no members, so theres nothing between the curly braces.

```js
var demo = {};
```

So, how do we alter this object to add members? We have two options "dot notation" and "bracket notation". They do exactly the same thing, but lets look at examples of both. We'll start again with our empty object

```js
var demo = {};
console.log( demo );

// Adding a member with dot notation
demo.firstName = "Markus";
console.log( demo );

// Adding a member with bracket notation
demo[ "lastName" ] = "Jackson";
console.log( demo );
```

Run the code and take a look at the output. `console.log( demo )` is called three times over the course of this code snippet and each time it returns a different result.

The first time it is called it returns the empty brackets. Those empty brackets simply mean that the object has no members.

The second time console.log is called, the object has a new member `firstName` and it has been set with a string value "Marcus". This logging follows the dot notation statement. `demo.firstName = "Markus";` means "access the firstName member of the demo object and assign it a value of 'Markus'". If that member (`firstName`) doesn't exist, then it is created.

The third time that `console.log` is called, the object has yet another member `lastName` with a value of 'Jackson'. This new development follows the bracket notation statement. `demo[ "lastName" ] = "Jackson";` can be read as "access the lastName member of the demo object and assign in a value of 'Jackson'. The syntax is a little different, but the result is the same. A new member was created (because it didn't already exist) and was assigned a string "Jackson". One notable difference between dot notation and bracket notation is that bracket notation requires that member being accessed is in string form, e.g. `demo[ "lastName" ]`

Notice that the two members (`firstName` and `lastName`) are separated from their values by a colon and that the member/value pairs are separated from one another by a comma.

if we wanted to create this object without having to add elements to it manually we could do so like this. I'll throw in a couple other code samples for you to puzzle out before moving on to a much more detailed discussion of bracket notation and dot notation.

```js
var demo = {
  firstName : 'Marcus',
  lastName : 'Jackson'
};


// Accessing the firstName property of demo via dot notation
console.log( 1, demo.firstName );
// Accessing the firstName property of demo via bracket notation
console.log( 2, demo[ "firstName" ] );

// Accessing the lastName property of demo via bracket notation
console.log( 3, demo.lastName );
// Accessing the lastName property of demo via bracket notation
console.log( 4, demo[ "lastName" ] );

// Altering the lastName property via dot notation
demo.lastName += " III";
// Altering the firstName property via dot notation
demo[ "firstName" ] = "Darius";

// Logging the concatenation of the first and last names
console.log( 5, demo[ "firstName" ] + " " + demo[ "lastName" ] );

// Creating two new members of the demo object, `age` and `hobbies`
demo.age = 35;
// object properties can hold any data, including arrays, other objects, and functions
demo.hobbies = [ "golf", "swimming", "reading" ];

// Logging the whole demo object to show how it has been changed
console.log( 6, demo );

// Logging the last item in the hobbies property
console.log( 7, demo.hobbies[ 2 ] );
```

One last example to demonstrate that objects can hold any data. The 'this' keyword is simply the way that an object refers to itself. In this case `this` is the way that `demo` can refer to itself within its own object body.:

```js
var demo = {
  firstName : 'Marcus',
  lastName : 'Jackson',
  age : 35,
  appearance : {
    hairColor : "black",
    height : "74in",
    weight : "195lb"
  },
  hobbies : [
    "golf",
    "swim",
    "read"
  ],
  sayHello : function(){
    console.log( "Hello, my name is " + this.firstName + " " + this.lastName + "." );
  },
  describeMyself : function(){
    console.log( "I am " + this.appearance.height + " tall and " + this.age + " years old. I like to " + this.hobbies[ 1 ] + " and to " + this.hobbies[2] + "." );
  }
};

demo.sayHello();
demo.describeMyself();
```

Explore these two larger examples before moving on. Its okay if you don't understand everything, but try to wrap your mind around as much as possible before moving on. Make note of the things you find confusing.

The next couple sections will take a detailed look at accessing and altering data in object literals.

## A Deep Dive into Bracket Notation  

You can access properties of objects using bracket notation in a manner similar to how brackets are used with arrays. In bracket notation the object's property is accessed by first referencing the object's name, followed by a `[` character (a square bracket, non-curly, non-mustache, call it what you will, just be consistent), the name of the property as a string, and finally a `]` character. For example:

```js
vehicle["mpg"]
```

The above example refers to the `mpg` property on the `vehicle` object.

### Brackets and Reading Object Literal Data  

We can use bracket notation to read the value of a property.

```js
var company = {
  name: "ThyssenKrupp",
  country: "Germany",
  business: "elevator manufacturing",
  established: 1999
};

console.log("The company " + company["name"] + " was founded in " + company["established"] + " in " + company["country"] + " and is in the business of " + company["business"] + "." );
```

This program shows how we can read properties from an object using bracket notation.

> It's important to note that, just like we'll see next in dot notation, if you try to read a property that doesn't exist you will receive an `undefined` result.


### Brackets and Editing Object Literal Data  

You can also use bracket notation to edit or create properties on objects.

This example shows how you can create and modify properties using bracket notation.

```js
var country = {}
country["name"] = "Zimbabwe";
country["motto"] = "Unity, Freedom, Work";
country["capitol city"] = "Harare";
country["estimated population"] = 13061239;

// log our country to the console
console.log(country);

// Our population just grew by 2!
country["estimated population"] += 2;

// log our updated country to the console
console.log(country);
```

## A Deep Dive into Dot Notation  

One of the greatest feelings while learning to code is when you start to identify patterns and similarities in different aspects of the discipline. You'll find that this can be said of bracket vs. dot notation. Take a look below to spot the similarities and differences.

In dot notation the object's property is accessed by first referencing the object's name, followed by a . character, and then the name of the property. For example:

```js
vehicle.mpg
```

The above example refers to the `mpg` property on the `vehicle` object.

> The `.` character can be thought of as being similar to the possessive "apostrophe s" in english. As in the `.` indicates that the property belongs to the object, like the car's mpg property.

### Dots and Reading Object Literal Data  

We can use dot notation to read the value of a property.

```
var animal = {
  name: "African bush elephant",
  scientificName: "Loxodonta africana",
  weightInLbs: 13000,
  heightInFt: 11
};

// describe our animal
console.log("The " + animal.name + " (" + animal.scientificName + ") weighs in at " + animal.weightInLbs + "lbs and is " + animal.heightInFt + " feet tall.");
```

The above example demonstrates how we can read properties from an object using dot notation.

> As was the case with bracket notation, if you try to read a property that doesn't exist you will receive a result of `undefined`.

Run the code below. What result do you anticipate, and why does it log that?

```js
var hat = {
  style: "bowler hat",
  price: 39.00,
  material: "whool",
  productNumber: 83729
};

console.log(hat.inventory);
```

### Dots and Editing Object Literal Data  

Just like with brackets, you can also use dot notation to update or create properties on objects.

This example shows how you can create properties using dot notation.

```js
// create an empty object to hold planet information
var planet = {};

// create some properties
planet.name = "Earth";
planet.orbitalPeriod = 365.256363004;
planet.orbitalSpeedKms = 29.78;
planet.meanRadiusKm = 6371;
planet.satellites = 1;
planet.axialTilt = 23.4392811;

// output the planet to the console
console.log(planet);
```

Note that we begin with `var planet = {};`, an empty object to fill with properties. In essence all we're doing here is defining "planet". Without creating that object, none of the rest of the dot-notated-properties make sense. If we wanted to we could have said `var umbrella = {};` and then each of our subsequent properties would have to be `umbrella.name`, `umbrella.orbitalPeriod`, etc.

Here is an example where properties are *edited* using dot notation:

```js
// create a person object
var person = {
  name: "Orion Monserrat",
  age: 47,
  weight: 195
}

// output out person
console.log(person);

// the person gets older
person.age++;

// the person eats way too many dough-nuts
person.weight = person.weight + 10;

// output out our changed person
console.log(person);
```

By now you're probably aware but just in case- the `++` on line 12 after `person.age` is a way of incrementing a property by one. It's kind of like saying (person.age)+1.

**If you write to a property that doesn't exist on an object the property will automatically be created for you.**

## Conclusion  

One of the main benefits of object literals is that they encase data within a set of curly brackets, a move that helps the coder from writing a ton of global variables. Often, the more global variables you use the greater the risk of errors when combining code blocks into one document.

As for whether bracket notation is better than dot notation or vice versa, it's really coder's choice. The dot method is perhaps, at first glance, a cleaner form of writing code. That being said, cleanliness is important when programming but at the end of the day the most important thing is writing code that behaves the way you want it to behave, and that includes obtaining the ability to create and edit data.

### References  

* [Grammar and types - MDN (scroll to middle of article for Object Literals)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types)

* [Property accessors - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_Accessors)

* [JavaScript Object Literals Simplified](http://www.standardista.com/javascript/javascript-object-literals-simplified/)
