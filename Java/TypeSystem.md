# Basic Data Types

Primitives are the most basic type of data in Java. They are simple values and can serve as building blocks for more complex types of data.

The most common primitive data types are:

* **boolean** A value indicating true or false.

* **integer** A whole number, e.g. 1, 2, 3, ...

* **double** A decimal number., e.g. 1.2, 2.5, 0.00001

> In this discussion, we'll talk about these three primitive types, though Java offers several more. [You can get more details about primitive data types here.](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html)

## Boolean  

*Keyword:* `boolean`

Booleans are primitive values that indicate *true* or *false*.

12 is greater than 42 is `false`.

The word "five" starts with an 'f' is `true`.

```java
boolean isAwesome = true;
```

We can use boolean values to make decisions and control what our application does. For example, we can check to see if a user is logged into our application. If not (`false`), they are sent to a login form. If so (`true`), they may use the application.

## Integer  

*Keyword:* `int`

Integers represent whole numbers. Whole numbers are the numbers we count with, 1, 2, 3..., etc.

In Java, integers are any whole number from -2,147,483,648 to 2,147,483,647.

*Note: Java's syntax for integers does not allow for commas!*

Here are some example integers:

```java
int myNumber = 100;
```

* -2147483648

* -7836832

* -74

* 0

* 1

* 42

* 90032

* 2147483647

These are not integers:

* -123.456

* 0.57721

* 3.1415

* 4.6692

* 90032.0000000001

## Double

*Keyword:* `double`

Doubles represent decimal numbers. In programming, decimal numbers are often called floating point numbers. Unfortunately, it's hard to represent decimal numbers precisely using computers. Because of this, doubles are estimates of decimal values, which can create problems when comparing values.

Doubles can be any number between 4.9 x 10-324 (very, very small) and 1.7976931348623157 x 10308 (exceptionally large).

```java
double myDecimal = 123.456;
```

Here are some example doubles:

* -2147483648

* -7836832

* -123.456

* -74

* 0

* 0.57721

* 1

* 3.1415

* 4.6692

* 42

* 90032

* 90032.0000000001

* 2147483647

There is another data type in Java named `float` that also represents floating point numbers. The value range of `floats` is smaller than the `double` and `int` primitives. `floats` are less precise than `doubles`. For everything we're going to do in this class, `doubles` should be sufficient.

## Variable Widening

Here's a puzzle for you:

We know that the number 123 is an integer.

We know that variables can only hold the type of data that they're declared to hold. For example, in this snippet:

```java
int x = 123;
```

`x` can only hold integer values. If you try to set `x` to `4.2` you'll get an error.

```java
int x = 123;
// ERROR!
x = 4.2;
```

This error occurs because an integer variable can't hold a double. It simply can't represent anything with a decimal point.

Now consider this snippet:

```java
double y = 4.2;
```

`y` can hold double values. If you attempt to set `y` to `123`, you will find that it works.

```java
double y = 4.2;
// Not a problem, no error
y = 123;
```

So, what gives? Why does it work one way, but not the other?

The answer is that Java can **widen** an integer *into* a double. The idea of widening is that one type can encapsulate another. Doubles can handle decimals; integers cannot.

As such, you can represent an integer as a double without losing any information. This magic doesn't work the other way. If you tried to represent a double as an integer, you'd have to remove all the digits after the decimal point. Because this could introduce errors in the code, Java doesn't allow it and throws an error.

## Null  

Null is the absence of value. Null is *not* zero. Null is not an empty string. Null *isn't anything*. It is the black hole of values.

Null is used to tell Java that there is no value for a variable, but only for non-primitive variables. In the following example, `myDog` is of type `String`. Unlike `int` or `boolean`, `String` isn't a primitive type. Because `String` isn't primitive, I can set its initial value to `null`.

```java
String myDog = null;
```

Setting `myDog` to `null` means that it has no value. Sadly, I don't have a dog. We can set `myDog` to `null` because `myDog` is a `String`. If you try to set a primitive to `null`, Java will throw an error.

```java
// ERROR!
int myNumber = null;
```

Why do we want or need null? Here's a quick example.

```java
String userResponse;
if (complexCondition) {
    //Some huge complex block of code
    //Where you know at some point userResponse will be initialized
    userResponse = "Awesome!";
}
//Later, we go to use the variable:
System.out.println(userResponse); // <- Compiler error!
//"Variable userResponse might not have been initialized"
//If we're sure that it really WAS initialized, we can resolve this error
//By changing String userResponse; to String userResponse = null;
```

## Other Java Types  

Java is not limited to the three types of primitive data we discussed previously. In fact, there are eight total types of primitive data that you can read over in the article linked above. Java also has complex types of data you can use. These complex data types are called *classes*.

Java ships with a standard library with over 20,000 classes. These classes represent almost everything you might need as a developer including dates, strings of text, encryption algorithms, protocols for communicating over the internet, and more.

For now, let's focus in on one particular class, `String`. We already saw an example of `String` in a previous code sample. Now, let's discuss it in more detail to expand on our discussion of classes.

## The String Class  

*Keyword:* `String`

**In Java, Strings are sequences of characters.** Characters are letters, numbers, punctuation, and other special characters such as emoji. Every word is a String. Every sentence is a String. Every paragraph or any other block of text is a String. Even a single character could be a String.

Java has a special primitive called a `char` that it uses to represent a single character. Strings are actually just a sequential collection of `char`s. To create a String, use the "String" class name to define the type, give the variable a name, and assign it a value.

```java
String greeting = "Hello world!";
String farewell = "Goodbye!";
```

Notice the double quotation marks. These tell Java that the characters contained between them make up a String.

One thing that makes Strings unique is that they can be added together using the addition operator. When the addition operator is used to combine strings it is called *concatenation*.

For example:

```java
String fullGreeting = "Hello" + " " + "World!";
// fullGreeting is "Hello World!"
```

### Concatenating Strings with Other Values  

If the addition operator is used with a String, it will automatically convert anything else to a String and then concatenate them together.

```java
"Hello World" + 2
// Will result in "Hello World2"

3 + "Hello World"
// Will result in "3Hello World"
```

## Conclusion  

Java supports eight types of primitive data and an infinite number of classes. In this article, we discussed some very common primitive types, `int`, `bool`, and `double`. We also discussed Strings and concatenation. Over time, you will see many additional types and classes.

If you want to read ahead and familiarize yourself with the other primitive types, please read [You can get more details about primitive data types here.](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html).

---

# Variables  

All variables in Java are *statically typed*. You have to tell Java what type of data a variable is allowed to store. Static typing is how Java knows what it can or can't do with the variable.

"Static" has a few different definitions in programming, depending on the language and the circumstances. Getting confused can be easy. In this case, **"statically typed" just means that once we've assigned a type to a variable, we can't later change the type.**

JavaScript isn't statically typed. We can create a variable and assign it a string and then later change the value to a number. JavaScript is fine with this type of change; Java is not.

JavaScript is "loosely typed":

```javascript
let thing = "I am a thing.";
thing = 9;
// JavaScript is totally fine with this
```

Java is "statically typed":

```java
String thing = "I am a thing.";
thing = 9;
// ERROR!!! Not allowed
```

## Setting Variables  

When setting a Java variable, you must specify the variable's data type, its name, and (optionally) an initial value. If we wanted to create a variable to hold an integer value, we could do so like this:

```java
int x = 1;
```

This code creates a variable named `x` which can hold an integer value. The value of `x` is `1`. Let's break this statement down:

When defining a variable, the very first part is a keyword that tells Java what type of data the variable can hold. In this case, the keyword is `int`, which indicates that the variable will hold an integer.

The next part (`x`) is the name of the variable.

Finally, we have the actual value the variable is being set to, `1`.

The assignment operator, `=`, sets the value of the variable after the entire expression to the right side has been evaluated. Consider this example:

```java
int x = 1 + (120 - 5);
```

The value of `x` will be set to 116 (not 1) because the expression `1 + (120 - 5)` results in 116.

## Reading, Using, and Updating Variables  

Referring to variables is the same in Java as it is in JavaScript. Once the variables are declared, you can manipulate them using the variable name:

```java
int x = 12;
int y = 32;

x  = x + y; // x now equals 44
y = x; // y now equals 44
x = 123; // x now equals 123

x = "Howdy."; //ERROR!!
```

**Note:** We only specify the type of the variable when we declare it. Once, `x` and `y` exist, we can refer to them by name, without also declaring the type.

### Integer Math  

Up until now, everything except having to explicitly declare the type of a variable should feel familiar from JavaScript. One big difference is that because Java has different types for integers (whole numbers) and floating point (decimal) numbers, it will calculate things a bit differently.

Consider the following code:

```java
int x = 12; //Or let x = 12 in JS
int y = 32; //Or let y = 32 in JS

int z = x / y;
```

In JavaScript, `z` would evaluate to `0.325`. In Java, however, it results in `0` - if you start with two integers, it will give you back an integer. `0.325` gets rounded down to 0.

## Naming Variables in Java  

You can't call a variable just anything in Java. These rules are controlled by Java and violating them will result in an error.

The rules for naming are as follows:

* Variable names are *case sensitive*! A variable named `person` can only be accessed as `person`, not `Person`, `PERSON`, `perSon`, or any other variation.

* Variables can only start with a letter, the underscore character `_`, or a dollar sign `$`. This means that a variable may not start with a number or any other special character.

* Subsequent characters in the variable name can be any combination of letters, numbers, dollar signs, or underscores.

Here are some valid (though mostly terrible) variable names:

* `x`

* `X`

* `person`

* `$system`

* `_______`

* `a`

* `NAME`

Here are some invalid variable names:

* `@username`

* `1thing`

* `\[person\]`

* `something with spaces`

In addition to the strict rules dictated by Java, developers have adopted some conventions to make naming consistent and easier to understand.

### Give variables meaningful names that are easily understood.

1. Don't unnecessarily abbreviate or use acronyms. For example, a variable named `delay` is much easier to read than one named `dly` or `d`.

2. As an extension of (1) above, single-letter variable names like `x` or `i` should be reserved for simple things like loop counters ONLY and never for "real" variables. For example, if you are modeling the position of something on an x/y grid (like a game piece):

```java
//Good
int xPosition;
int yPosition;
//Fine
int xLoc;
int yLoc;
//Bad
int x;
int y;
```

3. Avoid excessively long variable names when they aren't needed. For example, `xPositionOfItemOnGameBoard`.

4. Don't be afraid of longer names if they ARE needed to describe your concept, like `String itemActionDescription`

### Variables should use [camelCase](https://en.wikipedia.org/wiki/Camel_case).  

With camel case, the first letter of a variable is always lowercase. If the variable is made up of multiple words strung together, the first letter of each subsequent word should be capitalized.

Here are some good variable names:

* `name`

* `camelCase`

* `grossDomesticProduct`

* `weightPerPigeon`

Here are some bad variable names:

* `NAME`

* `camelcase`

* `gdp`

* `wpp`

In some other languages, it is common to separate multiple words with an underscore. EG: `gross_domestic_product` (this is referred to as "snake case" if you're curious). That is not the convention in Java.

## Final Variables  

You will occasionally see variable names with all letters upper case and words separated by underscores. This naming convention should be used only for `final` or constant variables. Variables set with the `final` keyword cannot change after they are declared.

For example, if we are writing a guessing game we might declare:

```java
final int MAX_GUESSES = 5;
MAX_GUESSES = 3; //ERROR, not allowed!
```

If we try to set `MAX_GUESSES` to 3 after the declaration, Java will throw an error. It's an excellent coding habit to use final variables for any constants, especially if the constants get employed in multiple places in your code.

There are two main reasons for this:

1. It effectively provides free documentation for your code. Someone reading your code can easily see `if (currentGuesses >= MAX_GUESSES) { //end game }` and understand what's going on.

2. If you decide later you want to allow more guesses, you only have to change the variable in one place.

## Conclusion

Java has a lot of rules for variables, operations, and declarations. These rules are a good thing because they force us to write better, more readable code. These rules also give us a lot of information about the details of our application. Are we accidentally trying to divide a number by a String? Oops, Java throws an error. Did we accidentally assign a double to an int? Yup, Java will let us know.

Java is great about providing a detailed body of objects to work with. In time, you will pick up lots of them and learn to use a wide range of tools that help you write more predictable code.
