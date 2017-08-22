# Arrays  

Storing, manipulating, and creating lists of items is a big part of programming. Very often, you will work with lots of data in your applications. If you are working on an eCommerce site, you may manipulate lists of products or lists of users. If you are building a Google Map clone, you may have to store long lists of coordinates, street labels, and markers.

Most programming languages offer some "indexed list" to make storing and parsing data easier. We'll define and explain these concepts in just a few minutes. In this article, we will talk about the "list" object available in JavaScript. We will discuss three things:

* **The purpose of the Array object** - Understanding its purpose and use-case can help us to develop an intuition about how best to use an array.

* **Provide an overview of array syntax** - This will include the general syntax for accessing and manipulating array index values.

* **Discuss Array object properties** - Arrays are objects, have properties that we can access to inspect the details of our array data, including values that tell us how many members the array includes and some useful functions for manipulating the data included in an array.

## What are Arrays?  

** An array is a zero-indexed JavaScript object that stores a list of values.**

We have a few relevant terms about which we should develop a strong intuition before we talk about syntax. One term is "zero indexed," another is "list." To develop this intuition, let's talk about the idea of an array before we discuss the *syntax* of an array.

Imagine that you are building an app that stores data about all of the Pixar films. Typically, the movie data would be stored as objects, but for simplicity, we'll treat them as strings. What follows is the full list of films:

```
Toy Story
A Bug's Life
Toy Story 2
Monsters, Inc.
Finding Nemo
The Incredibles
Ratatouille
WALL-E
Up
Toy Story 3
Cars 2
Brave
Monsters University
Inside Out
The Good Dinosaur
Cars 3
```

Working with this data could be difficult. If we store each movie as a string in a variable, we have to decide what to call the variables. I could use a couple of patterns for my variables. For instance, the variable name could be derived from the string:

```js
let toyStory = "Toy Story";
let bugsLife = "A Bug's Life";
let toyStory2 = "Toy Story 2";
... etc.
```

The problem with this pattern is that I have to know all of the variable names to access the movie titles and the variable names are the same as the film titles. Storing data this way is confusing and clunky. Imagine if we were storing all the movies made by every studio for the last ten years. That's thousands of movies!

We need a simpler convention for storing the movies. What if we develop a simple naming convention for the movies: All variables will start with "movie" and will end with a number. That way, I can access movies if I know the naming convention.

```js
let movie00 = "Toy Story";
let movie01 = "A Bug's Life";
let movie02 = "Toy Story 2";
let movie03 = "Monsters, Inc.";
let movie04 = "Finding Nemo";
let movie05 = "The Incredibles";
let movie06 = "Ratatouille";
let movie07 = "WALL-E";
let movie08 = "Up";
let movie09 = "Toy Story 3";
let movie10 = "Cars 2";
let movie11 = "Brave";
let movie12 = "Monsters University";
let movie13 = "Inside Out";
let movie14 = "The Good Dinosaur";
let movie15 = "Cars 3";
```

Thats better, but still pretty clunky. We are repeating ourselves over and over again. Look at all those variables that start with "movie". Again, imagine if we were working with ten thousand movie titles.

Arrays try to solve this problem by providing a way to store all the movies in one variable. The idea is that arrays provide a syntax so that all the items can be stored in one object and then accessed by their location in the list.

The following code snippet shows you an array containing all of the Pixar films. We'll talk about the syntax in a moment. For now, notice that all of the movie titles are stored in one variable. I'll print a couple of elements from the array as well so you can see that they are accessed by their location in the list.

```js
let pixarMovies = [ "Toy Story", "A Bug's Life", "Toy Story 2", "Monsters, Inc.", "Finding Nemo", "The Incredibles", "Ratatouille", "WALL-E", "Up", "Toy Story 3", "Cars 2", "Brave", "Monsters University", "Inside Out", "The Good Dinosaur", "Cars 3" ];

// prints the first movie title
console.log( pixarMovies[ 0 ] );

// prints the third movie title
console.log( pixarMovies[ 2 ] );
```

The big win here is that we only have one variable storing all of the information. Again, this list could be thousands (millions even) of movie titles, and we'd still only have one variable. Notice also that we now have an easy way to access data from the list.

In the next section, we'll discuss array syntax and the basics for accessing and manipulating array data.

---

# Array Syntax  

## Defining an empty array  

Use square brackets, [ and ], to define an array. The following code sample creates an empty array. This array contains no data, but still has all of the array object properties.

```js
let empty = [];
```

## Initializing an Array with Data  

When creating an array with data, write the data between the brackets. Separate each element from its neighbor with a comma ,. The following array contains three strings: "Toy Story," "A Bug's Life," and "Toy Story 2".

```js
let movies = [ "Toy Story", "A Bug's Life", "Toy Story 2" ];
```

Arrays can contain any data, which you can mix and match at your leisure. It's usually a good idea to fill your arrays with one type of data because you will often be running through the data, item by item. The following array contains some random information.

```js
let things = [ "Toy Story", 9, { name : "Woody" } ];
```

## An Array's Index Numbers  

Each element in an array is assigned a number, known as an `index`. You may have noticed in our Pixar movie example that we accessed each movie with a number. That number was the index. One common point of confusion is to decide *which number to use*.

JavaScript arrays are "zero indexed," meaning that the first element in the array is index 0, the second element in the array is index 1, the third element in the array is index 2, etc.

When counting the position of an element in an array, count starting with the number 0. For example:

```js
let pixarMovies = [ "Toy Story", "A Bug's Life", "Toy Story 2" ];

// prints the first movie title
// the first element in the array is index 0
console.log( pixarMovies[ 0 ] );

// prints the first movie title
// the first element in the array is index 1
console.log( pixarMovies[ 1 ] );

// prints the third movie title
// the third element in the array is index 2
console.log( pixarMovies[ 2 ] );

// this prints undefined because
// there is no fourth movie title at index 3
console.log( pixarMovies[ 3 ] );
```

> Remember: Indexing starts with the number 0. The items in an array are numbered 0, 1, 2, ... and not 1, 2, 3, ...

## Accessing Array Data  

You've already seen this a few times in this article, but let's go over the ways that array data can be access and altered using bracket notation. Arrays use bracket notation to access object data, but instead of passing a key string into the brackets, we pass the index number of the element that we want to access.

In the following example, I want to access "flower." It is in position 3, so its index value is 2. I use bracket notation and pass into the brackets the number 2, returning the string "flower."

```js
let plants = [ "tree", "bush", "flower", "grass" ];

console.log( plants[ 2 ] );
```
 
> Change the 2 to different numbers to see what values returns. Try to return the string "grass." Remember, arrays are "zero indexed."

## Assigning New Values to an Array  

To assign a new value to an array, combine bracket notation with the assignment operator. You can change the value of an existing index or add a new value to a new index.

```js
let plants = [ "tree", "bush", "flower" ];

// nothing has changed yet
console.log( plants );

// change the value of index 1 to the string "grass"
plants[ 1 ] = "grass";

// The array values are different
console.log( plants );

// adding a new array value at index 3
plants[ 3 ] = "algae";

//now the array has an additional element
console.log( plants );
```

We can even start with an empty array and build it from scratch using bracket notation. In the following example, we create an empty array `avengers` and add all of the characters via bracket notations. We then overwrite the string at index 1 with the string "Hawkeye" and print the final array.

```js
// create an array
let avengers = [];

// add elements to specific index locations
avengers[0] = "Iron Man";
avengers[1] = "War Machine";
avengers[2] = "Thor";
avengers[3] = "Captain America";
avengers[4] = "Hulk";
avengers[5] = "Black Widow";

// update an item in the array
avengers[1] = "Hawkeye";

// output the array to the console again.
console.log( avengers );
```

There is a tremendous amount that can be done to (and with) an array using just these couple techniques, but most manipulations are more easily accomplished via array object functions and properties. In the next section, we'll discuss some of the most common array object properties.

# Array Object Properties and Functions  

For each example below, I will first create an array and then demonstrate different methods of manipulating it using array functions and properties. Run the code samples to see the outcome of each array property and function.

## Array.length  

The length property tells you how many elements are stored in the array, which is *amazingly* useful. All the code examples in this article are short, but real world data may have thousands, even millions, of elements. Knowing how much data you are working with is extremely helpful.

```js
let pixarMovies = [ "Toy Story", "A Bug's Life", "Toy Story 2", "Monsters, Inc.", "Finding Nemo", "The Incredibles", "Ratatouille", "WALL-E", "Up", "Toy Story 3", "Cars 2", "Brave", "Monsters University", "Inside Out", "The Good Dinosaur", "Cars 3" ];

// prints the number of elements in the array
console.log( pixarMovies.length );
```

## Array.push  

The `push` function will add a new element to the end of the array. This feature is useful if you don't know how long the array is, and you want to add an element to it without accidentally overwriting an existing index value.

```
let zooCreatures = [ 'monkey', 'lion', 'penguin' ];

zooCreatures.push( 'otter' );

console.log( zooCreatures );
```

## Array.pop  

The `pop` function removes the last element from the end of the array and returns it.

```js
let holidays = [ 'Thanksgiving', 'Birthday', 'Festivus', 'Pi Day' ];

// remove the last item and store it in a variable
let removed = holidays.pop();

// log the altered list
console.log( holidays );

// log the item that was removed
console.log( removed );
```

## Array.unshift  

The `unshift` function adds a new element to the beginning of the array, putting the new element at index 0 and move existing elements up by one index.

```js
let exclamations = [ 'woah', 'awesome', 'oh joy', 'ummm' ];

exclamations.unshift( 'hooray' );

console.log( exclamations );
```

## Array.splice  

The `splice` function removes elements from an array using two number: the index from where elements should be removed and the number of elements to remove. This section will show a few examples.

In the following example, `splice` will remove 3 elements starting with index 2.

```js
let footwear = ['sneakers', 'tap shoes', 'clogs', 'slippers', 'heels', 'boots', "moccasins", "galoshes" ];
footwear.splice( 2, 3 );
console.log( footwear );
```

In this example, `splice` will remove 2 elements starting with index 3.

```js
let footwear = ['sneakers', 'tap shoes', 'clogs', 'slippers', 'heels', 'boots', "moccasins", "galoshes" ];
footwear.splice( 3, 2 );
console.log( footwear );
```

In this example, `splice` will remove 1 element starting with index 5.

```js
let footwear = ['sneakers', 'tap shoes', 'clogs', 'slippers', 'heels', 'boots', "moccasins", "galoshes" ];
footwear.splice( 5, 1 );
console.log( footwear );
```

## Conclusion  

Arrays are an essential element to all programming. You will write and manipulate them throughout your entire programming career. You should familiarize yourself with not only the concept of an array but the basic syntax and object methods that make manipulating arrays easier and more natural.

### References  

* [^1] [Array - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)

---

# Accessing and Modifying Specific Array Elements  

As we've discussed, an `array` is a list of elements, separated by commas, between brackets, and usually defined by a variable. Each item in this list is given an index value and, because arrays are `zero indexed`, the first item is given the value 0. In the example below, "empathy" is 1.

```js
const coolThings = ['tigers', 'empathy', 'pizza bagels'];
```

Note the brackets in particular here, as they will be our best friend when it comes to accessing specific array elements, as well as making changes to arrays. They also make a great friend because brackets look like they're giving a hug: [you]. See? Those brackets just hugged you. You're welcome.

## Bracket Notation  

Arrays in JavaScript provide a special syntax called bracket notation. Bracket notation allows you to reference a specific element in an array to either read or set its value.

In bracket notation you follow a variable name immediately with a set of square brackets that contain the index of the element you want to access. In the example from the introduction, the variable is rightly named `coolThings`, so you can access the third item in the array using `coolThings[2]`. (Remember, arrays are zero indexed).

In general, you can think of bracket notation as being a variable that points to a particular place in an array.

### Reading From Arrays Using Bracket Notation  

```js
const avengers = ["Iron Man", "Thor", "Captain America", "Hulk", "Black Widow"];

// output the character at index 0
console.log(avengers[0]);

// output the character at index 3
console.log(avengers[3]);
```

This example shows how we can read elements from an array at the specified index using bracket notation.

Take note, if you try to read an element in an array that doesn't exist you will receive an `undefined` value.

```js
const avengers = ["Iron Man", "Thor", "Captain America", "Hulk", "Black Widow"];

// this logs "undefined"
console.log(avengers[99]);
```

### Writing To Arrays Using Bracket Notation  

Another cool use for bracket notation is the ability to add and update items to our array.

```js
// create an array
const avengers = [];
avengers[0] = "Iron Man";
avengers[1] = "War Machine";
avengers[2] = "Thor";
avengers[3] = "Captain America";
avengers[4] = "Hulk";
avengers[5] = "Black Widow";

// output the array to the console.
console.log(avengers);

// update an item in the array
avengers[1] = "Hawkeye";

// output the array to the console again.
console.log(avengers);
```

This example shows how we can add items to an array using bracket notation. It also shows how we can change the value at an index.

Take note, any time you add an element to an array, any missing elements before that item become `undefined`. For example:

```js
// create an array
const avengers = [];

// add at index 0
avengers[0] = "Iron Man";

// add at index 5
avengers[5] = "Black Widow";

// output the array to the console.
// note that indexes 1 to 4 are "undefined"
console.log(avengers);
```

## Conclusion  

Using bracket notation, we are able to read information from an array, add information to an array, and change or update elements. This may not seem like a big deal with an array that contains six elements, but don't forget that arrays can contain massive amounts of information; they can have 100's of elements. Rather than sifting through them ourselves to find the 157th element so we can read it or change it, bracket notation comes to the rescue like Iron Man, Black Widow, or Undefined.

### References  

* [Property Accessors - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_Accessors)

--- 

# Control Flow  

When you write a program, you are giving the computer a series of instructions to follow. Those instructions are often complex and can depend on many factors including user input, current data, or instructions written by other programmers. Most programming languages include a series of "control flow" features to manage this complexity.

* **"Control flow"** - The execution order in instructions.**

* **Control flow statements** - The programming tools we use to define the order of execution, usually loops and conditional statements.

This lesson is not intended to give you a detailed breakdown of the syntax necessary to author control flow in your programs. Rather, this article outlines the general concept of control flow and why it is important.

## Managing Logic with Control Flow  

As a programmer, you will spend your time implementing some logic in your programs. Logic is a big topic, with lots of interesting branches into philosophy, math, and cognition. In this course, we will focus only on the concepts necessary to produce logically structured code. In essence, when we talk about flow control, we are talking about ways to tell our programs *when to run some code and how many times to run it*.

Now, let's look at a scenario to demonstrate how flow control is used to determine when specific blocks of code can run. The following scenario is common: When the user clicks a button, we want to perform an action.

In this case, the action is to change the color of a square. In the real world, the button might be opening and closing a dropdown to show content or a thousand other things.

Here is where control flow is significant: *the new color of the square depends on its current color*. If the square is red, we change it to blue and vice-versa. Review the following three lines of logic.

```
- If the user clicks the button, change the color of the rectangle.
- If the rectangle is blue, change it to red.
- If the rectangle is red, change it to blue.
```

Now, let's look at a program that follows this logic.

> The following program uses JavaScript that you haven't seen yet. Don't get bogged down in the code; the point of this little program is to *show the logic in action*, not to explain the details of the JavaScript syntax.

```html
<!-- index.html -->
<!doctype html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>Colored Square</title>
        <link rel="stylesheet" href="style.css">
    </head>
    <body>
        <div class="wrapper">
            <div id="square" class="red"></div>
            <a href="#" id="button">Change Color</a>
        </div>
        <script src="script.js"></script>
    </body>
</html>
````

```css
/* style.css */
body{
    margin: 10px 0 0 0;
    background: #666;
    font-family: Helvetica, Arial, sans-serif;
}
.wrapper{
    width: 600px;
    background: white;
    margin: 0 auto;
    padding: 10px;
}
#square{
    height: 200px;
    margin-bottom: 10px;
}
#button{
    display: block;
    padding: 10px 15px;
    background: #ccccff;
    text-align: center;
    text-decoration: none;
    border-radius: 10px;
    width: 50%;
    margin: 0 auto;
}
#button:hover{
    background: #aaaaff;
}
.red{
    background: red;
}
.blue{
    background: blue;
}
```

```js
//script.js
const square = document.getElementById( "square" );
const button = document.getElementById( "button" );

// When the button is clicked
button.addEventListener( "click", function(){
    
    let color = square.className;
    
    // Change the color of the square

    // if the color is red
    if( color === "red" ){
        // change it to blue
        square.className = "blue";
    }
    // if the color is blue
    else{
        // change it to red
        square.className = "red";
    }
} );
``` 

To see the above logic written as JavaScript, open the `script.js` file. Again, you won't understand all that you see in that file. However, read through the comments to follow the logic. The "colored rectangle" example uses a control structure called an `if/else` statement to determine the color of the rectangle. `if/else` statements are an example of control flow statements, of which JavaScript has several.

This example is intended only to give you a sense of how control flow works; we'll go over the details in following lessons.

## Types of Control Flow  

Control flow statements fall into two broad categories: **Loops and Conditional Statements.**

* **Conditionals Statements define the criteria that must be met to allow a block of code to execute.** The idea of conditional statements is that some code need only run some of the time. For instance, if you see phrases like: "If 'x' is true, run 'y' code," then you should be using conditional statements. If 'x' is false, then 'y' won't run. 'y' only runs some of the time.

* **Loops specify how many times a block of code should run.** Use Loops to repeat an action more than once. You'll know that you need loops you see phrases like: "Run this code 'x' number of times," or "Run this code until a condition is met." Whenever you need to do the same thing multiple times, use loops.

## Conclusion  

Control flow allows us to manage program logic. Using Loops and conditional statements will enable us to create (almost) any logic your program requires. We can determine when a section of code should run or how many times it should execute. Control flow structures are the backbone and foundation for all programming logic and define every decision made by our programs.

### References  

* [Control Flow and Error Handling - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Control_flow_and_error_handling)

* [Control Flow - Wikipedia](https://en.wikipedia.org/wiki/Control_flow)

---

# For Loops  

A `for` expression is a method to execute a block of code over and over again. It gives us the ability to massively simplify certain tasks, particularly when we want to run the same code multiple times (perhaps ad infinitum) while changing the value each time.

Here is a quick example. Take a few seconds to think about what is happening in this code. Don't worry so much about syntax (we're talking about that next). Instead, just think about what is happening to the variable `val` and how many times it's being printed to the console:

```js
let val = "a";

for( let i = 0 ; i < 10 ; i++ ){
    console.log(val);
    val += "a";
}
```

## Syntax of the `For` Loop  

The `for` loop is composed of several parts. The first part is the `for` keyword. The second part is the parenthesis, which contains three expressions: initialization, condition, and afterthought. The third part of the `for` loop is the block that runs each time the condition is `true`. Remember, the point of `for` loops is to run code multiple times based on the success or failure of a condition.

```js
// the `for` keyword
for( /*conditions*/ ){
    // The body block
}
```

The three items that go in the parenthesis can be hard to understand when first using the for loop, so let's break it down, item by item and talk about what each expression is doing.

```js
for ([1. initialization]; [2. condition]; [3. afterthought]){}
```

1. **Initialization expression**

An initialization expression is the first item in a `for` loop's parentheses. **It sets up a control variable for use in the loop's body and is run once before the first time to loop is run.** This expression runs only once, just before the loop starts.

2. **Conditional expression** 

The conditional expression is the second item in a `for` loop's parentheses. **The conditional expression is evaluated before each execution of the loop.** The loop's body executes if the conditional expression evaluates to true. If it's not true, JavaScript ends the loop and moves to the next line of code.

3. **Afterthought expression**  

The afterthought expression is the third item in a `for` loop's parentheses. **It is evaluated after each execution of the loop's body.**

The afterthought expression usually increments the variable defined in the initialization expression.

### An Example - Counting Loops  

Let's break down an example of a `for` loop:

```js
for(let i = 0 ; i < 15 ; i++){
    console.log(i);
}
```

The `for` keyword tells JavaScript that what follows is a `for` loop. Inside the parentheses, we have three distinct expressions separated by semicolons.

In our example above, the initializing expression is `let i = 0`. We are telling JavaScript that when we start the loop to create a variable named `i` and set its initial value to 0. This variable `i` will act as a counter, incrementing each time we complete the loop.

The conditional expression is `i < 15`. JavaScript will repeatedly execute the body of our loop so long as `i`, the variable we defined in the initialization expression, is less than 15.

The afterthought expression runs after each run of the body. In this case, the afterthought expression is `i++`, which is another way of saying `i = i + 1`. This line of code increments `i` by 1 after each iteration. Remember that `i` is the variable we defined in the initializing expression and what we're checking after each iteration through the body of the `for` loop.

The entire goal of this `for` loop is to add 1 to `i` over and over again until `i` is equal to 15. The body only prints the value of `i` each time that `i` is checked to see that its less than 15.

### Another Example - Building a String  

Let's revisit the first example we saw. In this example, we are slowing building a string. The initialization, conditional, and afterthought expressions are very similar to the Counting Loops Example. This example initializes a counter `i` with a value of zero. Before each run of the body, the condition `i < 10` is checked. After each run of the body, `i++` is run, which increments `i`.

In this example, we aren't printing `i`. Instead, we are printing the string in `val` and then concatenating another "a" to that string. The result is that after each loop `val` gets longer and longer. First its value is "a", then "aa", then "aaa", etc.

```js
let val = "a";

for( let i = 0; i < 10; i++ ){
    console.log(val);
    val += "a";
}
```

The point of loops is that they give us a compact way to run code over and over again. In this example, the following two lines run ten times:

```js
console.log(val);
val += "a";
```

I could do the same thing without a loop, but it requires a lot of repetitive code.

```js
let val = "a";

console.log(val);
val += "a";
console.log(val);
val += "a";
console.log(val);
val += "a";
console.log(val);
val += "a";
console.log(val);
val += "a";
console.log(val);
val += "a";
console.log(val);
val += "a";
console.log(val);
val += "a";
console.log(val);
val += "a";
console.log(val);
val += "a";
```

## Iterating over Arrays  

An extremely common use of loops is to "iterate" over arrays. Iteration means that we step through the array and do something with each of the elements. I can demonstrate this without a `for` loop by accessing each element by index. In this example, I'll step through the array and multiply each element by 2.

> nums[ 0 ] *= 2; is a more compact way of writing nums[ 0 ] = nums[ 0 ] * 2;

```
let nums = [ 25, 11, 56, 73, 92, 453, 23, 23 ];

nums[ 0 ] *= 2;
nums[ 1 ] *= 2;
nums[ 2 ] *= 2;
nums[ 3 ] *= 2;
nums[ 4 ] *= 2;
nums[ 5 ] *= 2;
nums[ 6 ] *= 2;
nums[ 7 ] *= 2;

console.log( nums );
```
 
Writing code like this works but creates a lot of duplication. What if our array contained 100000 numbers and we needed to double all of them? A `for` loop can simplify this task immensely. Also, take note of the index values we are using to access each element in the array. The index increments from 0 to 7. It looks a lot like the `i` variable in our loop examples.

The goal of iterating over arrays with loops is to replace this:

```js
nums[ 0 ] *= 2;
nums[ 1 ] *= 2;
nums[ 2 ] *= 2;
nums[ 3 ] *= 2;
nums[ 4 ] *= 2;
nums[ 5 ] *= 2;
nums[ 6 ] *= 2;
nums[ 7 ] *= 2;
```

with this:

```js
// The index is now `i`
nums[ i ] *= 2;
```

This next example shows a `for` loop that iterates over the `nums` array and doubles every value. The loop is simply executing the same block multiple times and changing the value of the `i` variable to access different members of the `nums` array:

```js
let nums = [ 25, 11, 56, 73, 92, 453, 23, 23 ];

for( let i = 0; i < 5; i++ ){
    nums[ i ] *= 2;
}

console.log( nums );
```

### Iterating Over Multiple Arrays  

In this next example, we will use `i` to iterate over two arrays and print the full names of each person.

```js
let first_names = [ "Tim", "Bill", "Sally", "Joan" ];
let last_names = [ "Carpenter", "Stapleton", "Duncan", "Smith" ];

for( let i = 0; i < 4; i++ ){

    console.log( first_names[ i ], last_names[ i ] );
}
```

Don't get me wrong, iterating over four names isn't anything difficult. However, imagine if we were a huge company with 50,000 employees and we wanted to run complex operations on lots of our employee data.

Maybe we want to find average salaries in different regions of the country; we could iterate over our list and add up all the salaries in a certain area and then divide that total by the number of people in the area. The `for` loop gives us lots of freedom to solve problems like that.

```js
let salaries = [ 120000, 80000, 92000, 65000, 37000, 52000, 43000, 69000, 100000, 56000 ];

//get the number of items in the salaries array
let length = salaries.length;

// we need to add up all the salaries
// so we need a variable to hold the total
let sum = 0;

// add up all of the salaries
for( let i = 0; i < length; i++ ){
    sum += salaries[ i ];
}

// divide by the number of salaries
// to get an average
// print the average to console
console.log( sum / length );
```

It wouldn't matter if we had a million salaries. The code above would work and print an accurate average salary. Loops are essential logic structures and when combined with arrays can produce some powerful code to iterate over and modify data.

### Nested Loops  

Nested loops are somewhat difficult to understand. You will become accustomed to the idea of nesting logic in time, but for now, you should simply take away from this discussion the fact that loops can be *nested* within one other. That is to say, we can create a *loop of loops*.

Let's say I wanted to print numbers from 1 to 10 the *number of times of their value*. For example:

* 1

* 2

* 2

* 3

* 3

* 3

* 4

* 4

* 4

* 4

* etc ...

Study those numbers to understand the behavior we are looking for. e.g. 1 prints once, 2 prints twice, 3 prints three times, 4 prints four times, etc.

This behavior can be accomplished several ways, but I'll implement nested loops. here is the logic that we need to implement:

```
initialize the OUTER LOOP counter (n) with 1
run the OUTER LOOP 10 times
each run of OUTER LOOP
    initialize the INNER LOOP counter (t) with 0
    run the INNER LOOP n times
    each run of the INNER LOOP
        console.log( t )
        increment t
    increment n
```

OUTER LOOP will iterate from 1 to 10 and will have a control variable n (for "number"). This will make it run ten times. Each time we step through OUTER LOOP, n will increase by 1.

```js
// OUTER LOOP, run from 1 to 10
for(let n = 1 ; n <= 10 ; n++){
    // print n, n times
    console.log( n );
}
```

I'm printing `n` so you can see that the body of OUTER LOOP is executed once for each iteration through the loop.

Now, we place an INNER LOOP inside OUTER LOOP. I must give INNER LOOP a new control variable so that it knows what step it's on. I'll call this `t` for "times," as in the number of times `n` has been printed. I'll default this to 0. Because `t` only exists within INNER LOOP, it will be reinitialized to 0 on each iteration through the OUTER LOOP. Since I want to print `n` a variable number of times, I'll write the conditional expression of INNER LOOP to be limited by `n`.

Finally, We increment `t` each time we complete an iteration of INNER LOOP.

```js
// loop from 1 to 10
for(let n = 1 ; n <= 10 ; n++){
    // print n, t times
    for(let t = 0 ; t < n ; t++){
        // print n
        console.log(n);
    }
}
```

This is *not* simple content. The idea of nested loops is complex and sometimes difficult to track. At this point, we have two reasons for studying nested loops:

* You will see them and need to be aware of what is occurring

* You should learn to try to avoid nested loops like this when possible. Its not always possible, but preferable to find some other means for implementing your algorithms

## Conclusion  

The `for` expression is vital to the way a computer thinks, reads, and interprets data.

Coders have been accused of being lazy; this is not accurate at all. A more astute observation is that coders have built, through trial after trial, a system of shortcuts and executable functions that make difficult and arduous work more easily accomplished. The `for` loop is a perfect example of this. It demonstrates why computers are vital; they aid in tasks that are either endlessly repetitive or too difficult to calculate. A `for` expression can compute in ways the human mind could only dream of.

### References  

* [For - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for)
