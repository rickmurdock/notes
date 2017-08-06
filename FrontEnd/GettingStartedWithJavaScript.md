# Including JavaScript in HTML  

You've already worked with HTML. Before we dive into writing JavaScript, let's talk about how we can link it to our `index.html` file. This way we can make use of it on the front-end.

The best way to add a JavaScript file to your HTML is to use the `<script>` tag. Here's what it might look like:

```html
<script src="main.js"></script>
```

You'll notice that unlike the `<link>` tag we use to include CSS, this tag has both an open and close. We'll see why this is later. After creating the `<script>` tag, you need to add the `src` attribute. This attributes points to the location of your JavaScript file. Here are some examples:

```html
<!-- Located in the root, next to your index.html -->
<script src="main.js"></script>

<!-- Located inside of an `js` folder -->
<script src="js/main.js"></script>

<!-- Located up one level and inside of a `js` folder -->
<script src="../js/main.js"></script>
```

## Script Tag Location  

Where you place the `<script>` tag does matter. It's not uncommon to see it called in the `<head>` tags, like so:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Cool Website</title>
    <!-- Not Recommended -->
    <script src="main.js"></script>
  </head>
  <body>
  </body>
</html>
```

While this *could* work, it's a best practice to include your script tags as the last elements in the `<body>` tags, like so:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Cool Website</title>
  </head>
  <body>

    <!-- Recommended -->
    <script src="main.js"></script>
  </body>
</html>
```

This way, the script won't be called until after all your HTML has loaded. While this might not matter much now, it will matter soon. When your JavaScript interacts with the elements on your page, you will be glad for this placement.

> There are ways of including JavaScript in your `<head>` tag and telling it to wait to execute. That is beyond the scope of this lesson.

## JavaScript in the Script Tag  

While we recommend linking to an external JavaScript file, you can also write code in your HTML. Just use the `<script>` tag to write the code directly onto the HTML.

```html
<script>
  let answer = 10 - 5;
  console.log("The answer is " + answer);
</script>
```

Don't worry about what the code is doing. Just notice that you don't need to use the `src` attribute, you can include JavaScript code right on the page. As you might have guessed, this is why the `<script>` tag has an open and close.

## Linking Dependencies  

Since HTML parses from top to bottom, we should include `<script>` tags at the lower part of the document. The same top down approach applies for adding multiple JavaScript files. Take the following as an example:

```html
<script src="js/parents.js"></script>
<script src="js/siblings.js"></script>
<script src="js/family.js"></script>
```

The ordering of these JavaScript inclusions is important. When the browser reads the files, it will do so in the order provided. In the above case, `parent.js` is read first, then `sibling.js`, and finally `family.js`. So long as the files included first don't reference code loaded later, all is well.

For instance, `family.js` can reference code in `parent.js` without creating errors. `parent.js` is already loaded by the time that `family.js` references it.

However, the opposite isn't true. If `parent.js` references code in `family.js`, the interpreter will throw an error. If `parent.js` tries to run code in `family.js`, it will do so before `family.js` is loaded and will throw an error.

## Conclusion  

Turning a simple HTML document into a powerful and cool web app is possible thanks to JavaScript. There are several ways to include JavaScript into your project. You can run a script from within the body and/or link to an external JavaScript file. Following best practices will ensure that your web app loads fast and correctly. JavaScript should be in a separate file and should be loaded *after* the HTML content.

---

# Learning a Programming Language  

It's important to know about the fundamental parts of any new language that you are learning. With spoken language, it helps to understand nouns, verbs, and adjectives. In programming, the same concept applies. One of the important parts of any programming language is data **types**.

**types specify what kinds of data can be stored and used in a language.**

## JavaScript Primitive Data Types  

JavaScript includes two categories of data types: Primitives and Objects. Objects are a complex subject that we will cover at a later date. Primitives act as simple values that can be passed around and referenced directly. In this lesson, we'll discuss Primitives.

JavaScript provides five primitive types:

| Type |	Syntax |
| --- | --- |
| String |	`"hello there!"`, `'hello there!'` |
| Number |	`1`, `200`, `3.14159` |
| Boolean |	`true`, `false` |
| Undefined |	`undefined` |
| Null |	`null` |

> Technically there are six types, but **Symbol** is new and fairly uncommon. We won't cover it here.

## String  

A `string` is a sequence of characters surrounded by quotation marks. You can use either `'hello there'` or `"hello there"` when writing a string. This convention varies widely between developers. In the JavaScript world, you'll find it more common to use the single quote since it's easier to type. Also, note that if you need to use an apostrophe inside of a string you either have to escape it or use double quotes. Take a look at these examples below.

```js
'The cat isn\'t in the barn' // Escaped
"The cat isn't in the barn" // Using Double Quotes
```

> In JavaScript, the `\` has a special purpose when dealing with strings. Combining it with another character enables that character to be represented in a string. This is called an `escape sequence`. It allows you to 'escape' from the normal string interpretation. When writing a string wrapped in single quotes, it would be impossible to write an apostrophe. Using an escape sequence makes this possible. For example:

```js
 `'I\'m always right. I can\'t be wrong.'`
```

> See references for a complete list of escape sequences.

## Number  

A `number` represents any numeric value. Some languages split numbers into multiple types depending on whether or not they contain decimals. JavaScript only has one `number type.

```js
24 // Number
-5 // Number
3.15 // Number
0.0001 // Number
```

## Boolean  

A `boolean` can only have two possible values: `true` or `false`. Booleans are standard across many languages. In JavaScript, they are used in control structures, like in a conditional statement.

```js
true // Boolean
false // Boolean
```

## Undefined  

`undefined` is the default value for any variable that has been declared, but **has not been assigned a value.**

```js
1 var newVar;
2
3 console.log( newVar );
```

## Null  

`null` is a representation of the **intentional absence of any object value.** It is often used to indicate that a variable should hold a value and doesn't.

```js
null // null
```

## Conclusion  

Boolean, number, string, null and undefined are used to optimize the data that a variable can contain. Understanding these primitive data types is a basic building block when learning JavaScript.

### References  

* [Escape Sequences](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String#Escape_notation)

* [MDN - JavaScript Data Types and Data Structures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures)

---

# Storing Data  

In programming, it's imperative that you have a way to store information. In most programming languages, these are called **variables**. A **variable** is a way for us to store a piece of data that can be used over and over throughout our program. It helps us write way less code and still accomplish our goals.

## Declaring and Assigning  

When talking about variables, there are two concepts one must understand. The first is the declaration of a variable. The second is the assignment of a value to that variable.

Think of it as a filing cabinet. You know you'll need to store your paperwork, but before you do so, you actually need to go out and get a place to store it. The filing cabinet is the *declaration* or creation of a place to hold your paperwork. Once you have that, you can now decide what paperwork to put in the cabinet, thus *assigning* it.

Let's look at the keywords we can use to declare and assign variables.

### Let Keyword  

The `let` keyword is the most common one you will be using. Let's look at how it works.

```js
let firstName; // declaration
```

There, we've just *declared* a new variable named `firstName`. Now let's assign something to it.

```js
let firstName; // declaration
firstName = 'Bill'; // assignment
```

We've now taken our `firstName` variable and *assigned* a value called `'Bill'` to it. You can also do your *declaration* and *assignment* at the same time, by doing the following:

```js
let firstName = 'Bill'; // declaration & assignment
```

### Const Keyword  

The `const` keyword stands for **constant**. We should use it when creating a variable that will never change. For instance, in the above examples, we used `firstName`. That could easily change in your program from `'Bill'` to maybe `'Julie'` and so on. However, let's say that you wanted to keep track of something that will never change in your entire program. That's when you would create the variable using `const`.

```ja
const pi = 3.14; // won't ever change
```

```js
// Let's define our favorite color, then change it.
let favoriteColor = 'green';
favoriteColor = 'blue';
```

While the above works just as you'd expect, the following will throw an error:

```js
const favoriteColor = 'green';
favoriteColor = 'blue'; // ERROR: Can't reassign "favoriteColor"
```

### Var Keyword  

The `var` keyword is short for **variable** and is the original way to create and store variables. In fact, for a long time, it was the only way to do so.

```js
var firstName = 'Bill';
```

In the current version of JavaScript, the `let` keyword seeks to fully replace `var`. The reasoning is due to a concept called **scoping**. This is beyond the scope of this lesson. For now it's just important to understand that you should use `let` instead of `var` to store standard variables.

Do note that `var` is still completely valid. You'll likely see it only in tutorials, or other developers you work with might still use it. There is nothing wrong with `var`, but the JavaScript community is moving away from using it. Learning the current way of assigning a variable is important.

## Variable Rules  

When declaring and naming a variable, there are a few rules that you should take into consideration.

* They can only **begin** with letters, `$` or `_`

* They can only **contain** letters, `$`, `_` and numbers.

* They are case sensitive (`firstName` is not the same as `firstname`)

* You can't use any reserved words ([see list](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Keywords))

* You can only use camelCase or snake_case

  * The standard convention in JS is to use camelCase

## Conclusion  

Variables are a fundamental concept in programming. We use them to create reusable bits of data, and we can pass them around throughout our application. We've looked at a few different ways of creating them, so you'll be ready to move on to more complex concepts.

---

# Truthy Vs. Falsey  

A previous reading described the idea of types in JavaScript. One of those types was the boolean type, which has only two values: `true` and `false`. An interesting JavaScript feature is that you can use any type where a boolean is expected. The language will treat that value like a boolean. The terms `truthy` and `falsey` are commonly used to describe this phenomenon.

Unfortunately, this can be a bit confusing. If anything can be a boolean, how do we know if it will be `truthy` or `falsey`? This lesson will explain some of the rules for which values will be either `truthy` or `falsey`.

Unfortunately, `truthy` and `falsey` are not a black or white comparison when we look at the rules for variable comparison.

However, even though you have `true` and `false`, it's not that easy. Sometimes when looking at certain types of values, they could actually be `true`/`false` or rather `truthy`/`falsey`.

## Falsey Rules  

There are certain values that are *always* `falsey` in JavaScript, these are:

```ja
undefined
null
0
"" // empty string
NaN // Not a Number
false
```

## Truthy Rules  

In JavaScript, anything not false is `truthy`. It is important to note that when we refer to something as being `truthy`, it does not just mean that the value is true. It means that the value coerces to `true` when evaluated in a boolean context.

```js
1 // Truthy
true // Truthy
"a" // Truthy
```

This might seem a little weird to you, but it's important to keep in mind when writing conditional statements. If a conditional statement doesn't boil down to one of the `falsey` values, then it's considered `truthy` and thus `true`.

```js
5 - 5 // 0 -> false
let x; // undefined -> false
x = ""; // empty string -> false
x = "a"; // non-empty string -> true
5 - 6; // -1 -> true
```

## Conclusion  

Understanding the intricacies of *truthy* and *falsey* will make your JavaScript clear and more concise. `Undefined`, `null`, `0`, `""`, `NaN`, and `false` are **always** falsey. All other values are truthy.

---

# Developer Tools  

Most modern browsers ship with a built-in set of tools called **Developer Tools**. These tools allow developers to interact with the front-end of their website. Developers can edit the HTML of your web page and adjust the CSS styles applied to any element. They can also run and debug JavaScript. In this section, we will focus on the `console`, which is where we can run and debug JavaScript.

## The Console Object  

Even if you haven't learned about JavaScript Objects, you can still use the `console` object. It's a great tool to get some insight as you develop.

The `console` object is made available through a Web API and provides access to the browser's debugging console. Although it can vary from browser to browser, most browsers will ship with a standard set of methods.

While the `console` object has a fairly large API, we will only be focusing on the `log` method.

## Console Log Method  

The `console.log()` method is used to output a message to the web console. You can write out any type of data you like. It is a great technique to use when trying to debug something you are working on.

The `log` method can take unlimited arguments. You can either specify a single piece of data like follows:

```js
1 let name = "Jeff";
2 console.log(name);
```

You can also specify multiple pieces of data like this.

```js
1 let name1 = "Jeff";
2 let name2 = "Martha";
3 let name3 = "Stacy"
4 let name4 = "Gwen";
5
6 console.log(name1, name2, name3, name4);
```
 
## Conclusion  

Developer tools allow developers to interact with the front-end of their website. These tools will give you access to the console object, which will help you debug your code. This is an easily accessible and invaluable tool that every developer should use.

---

# if...else Conditional Statements

Conditional statements dictate which parts of a program run, based on a "condition." They determine *if*, and *when* a block of code should run. The condition is an expression that resolves to either `true` or `false`.

## The `if` Statement  

The simplest conditional statement is called an `if` statement. The idea of an `if` statement is simple. It allows us to define a condition to determine if a block of code should run. If the condition resolves to `true`, then the code is run. If the condition resolves to `false`, then the code doesn't run.

The logic for an `if` statement is:

```
if the CONDITION is TRUE, perform an ACTION
otherwise, do nothing
```

The "otherwise, do nothing" line is necessary. It tells us that the code included in the if statement block will only run some of the time.

The syntax for an `if` statement requires a few items.

* The `if` keyword specifies that the statement is an `if` statement

* The parenthesis `()` contains the condition that is checked, and resolves to either `true` or `false`.

* The curly braces define the block of code (ACTION) that will run if the condition is `true`.

If the condition is `false`, then the action doesn't run. Nothing happens.

```js
if( /* CONDITION */ ){
  /* ACTION */
}
/* do nothing */
```

### An `if` Statement with a True Condition  

In this example, the conditional is `x > 0`. `x` holds the value 1. The expression evaluates to `true` because 1 is greater than 0, so `if` statement runs the block of code. You can read this example as: "If x is greater than 0, log the string 'This will log because the condition is true.'"

Because `x` is greater than 0, the string gets logged.

```js
1 let x = 1;
2 
3 // first if statement
4 if ( x > 0 ) {
5     console.log( "This will log because the condition is true" );
6 }
```

### An `if` Statement with a False Condition  

This time, the conditional is `x < 0`. `x` is 1, so the expression evaluates to `false`. The `if` statement does *not* run the block of code. You can read this example as: "If x is less than 0, log the string 'This will not log because the condition is false.'"

Because `x` is *not* less than 0, the string is *not* logged.

```js
1 let x = 1;
2 
3 // second if statement
4 if( x < 0 ){
5   console.log( "This will not log because the condition is false" );
6 }
```

### An `if` Statement that Compares Two Variables  

This third example compares the values of two variables. You can read it as saying: "If `count` is greater than or equal to `minimum`, log the string 'We have enough. Good job.'"

Run the following code sample. You should see "Evaluation complete. Nothing printed to STOUT." Because the condition is false, this message states that we did not log anything to the console. The `count` is *not* greater than or equal to `minimum`.

```js
1 let count = 10;
2 let minimum = 12;
3 
4 if ( count >= minimum ) {
5     console.log( "We have enough. Good job." );
6 }
```

Now, change `count` to the number 14 and rerun the code. When you run it this time, you should see a different message in the console: "We have enough. Good job.". Cool, this means that our condition is `true`. `count` is now 14, and `minimum` is 12. 14 *is* greater than or equal to 12, so the block runs.

## The `if/else` Statement  

`if/else` statements are slightly more complicated than `if` statements. Fortunately, they operate on the same basic concept. They contain an `if` condition that must be true for the first block of code to run. Besides the `if` block, the statement also includes an `else` block that will run if the condition is `false`.

The logic looks like this:

```
if the CONDITION is TRUE, perform the FIRST ACTION
if the CONDITION is FALSE, perform the SECOND ACTION
```

The syntax looks like this:

```js
if( /* CONDITION */ ){
  /* FIRST ACTION */
}
else{
  /* SECOND ACTION */  
}
```

You can read `if else` statements as "If the condition is true, run the first block. If the condition is false, run the second block."

### An `if/else` Statement Comparing Two Variables  

Let's elaborate on one of our previous examples:

```js
1 let count = 10;
2 let minimum = 12;
3 
4 if ( count >= minimum ) {
5     console.log( "We have enough. Good job." );
6 }
7 else{
8     console.log( "Not enough. We need more." );
9 }
```
 
When we run this code, we no longer get the message: "Evaluation complete. Nothing printed to STOUT." because we are always printing something to the console regardless of the condition. If the condition is `true`, the first block prints. If the condition is `false`, the second block prints.

As long as `count` and `minimum` store numbers, this code will always produce some output to the console.

## The `else if` Statement  

We can take the `if/else` statement a step further. We can define multiple conditions and multiple blocks to respond to those conditions. The `else if` statement allows us to add more than one conditional statement to determine the code to run.

The logic looks like this:

```
if the FIRST CONDITION is TRUE, perform the FIRST ACTION
else if the SECOND CONDITION is TRUE, perform the SECOND ACTION
if both CONDITIONS are FALSE, perform the THIRD ACTION
```

The syntax looks like this:

```js
if( /* FIRST CONDITION */ ){
  /* FIRST ACTION */
}
else if( /* SECOND CONDITION */ ){
  /* SECOND ACTION */
}
else{
  /* THIRD ACTION */  
}
```

## An `else/if` Statement Comparing Two Variables  

```js
 1 let count = 10;
 2 let minimum = 12;
 3 
 4 if ( count > minimum ) {
 5     console.log( "We have more than enough. Good job." );
 6 }
 7 else if( count === minimum ){
 8     console.log( "Just right; the perfect number" );
 9 }
10 else{
11     console.log( "Not enough. We need more." );
12 }
```
 
Run the code, unchanged. You should see the message: "Not enough. We need more.". Makes sense, right? 10 is less than 12, so the condition `count > minimum` (read as "`count` is greater than `minimum`") is `false`. Likewise, `count === minimum` (read "`count` is exactly equal to `minimum`") is also `false`. If both of our conditions are `false`, then we are left only with the final `else` block, which runs.

Now, change the value of `count` to 14 and rerun the block. You should see the message: "We have more than enough. Good job." because the first condition in the `else if` statement evaluates to `true`. The rest of the statement gets ignored, so the second condition isn't even tested.

Now, change the value of `count` to 12 and rerun the block again. You should see the message: "Just right; the perfect number." In this case, the first conditional evaluated to false (`count` isn't greater than `minimum` because it holds the same value as `minimum`). Since the first condition is false, the first code block doesn't run. Then, the second condition gets evaluated. Sure enough, it resolves to `true` (`count` is 12, and `minimum` is 12, so they are equal). Because this second condition is `true`, the `else` block is ignored as well.

> Another powerful conditional statement is called a `Switch Statement`. We don't cover it in this lesson, but its behavior is somewhat similar to the `else if` statements. If you want to read about it go to the [MDN Switch article](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch). Theoretically, you may never use a switch statement, but its good to be familiar with the basic syntax.

## Conclusion  

Conditional statements are significant for controlling the flow of your programs. In this lesson, we've used simple examples to show basic conditional statements. Over time, you'll learn to integrate conditional logic into your programs. You will be able to produce truly complex behaviors and sophisticated decision making.

### References  

* [if...else - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else)

* [Conditional Computer Programming](https://en.wikipedia.org/wiki/Conditional_(computer_programming))
