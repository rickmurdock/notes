# Constructor Functions  

In programming, we need to be able to write code that is efficient. Template style code can be used to replicate things 
without having to write detailed code over and over again. In JavaScript, we use constructors in order to be efficient 
with our code.

## What is a Constructor?  

A constructor is a `function` that is used to create objects. JavaScript objects are a way of keeping data contained and 
well-organized. Objects allow us to group different types of data together into more complex and convenient ways.

With a constructor, you can define properties and methods on an object to be set later. You can think of a constructor as 
a cookie cutter, in that it allows you to make multiple objects with the same properties and methods.

### Instance  

An object created by a constructor is called an *instance* of that constructor. Where a constructor creates a cookie cutter 
object, the instance is the concrete occurrence of the object - the actual cookie. *The creation of an instance is called 
instantiation*.

Simply put, a constructor is a function that is invoked by using the keyword `new`.

### The `new` keyword  

By placing the word `new` in front of the constructor name, JavaScript recognizes the keyword and creates a constructor.

```javascript
function Car(){};

let car = new Car();
```

> Important
If you forget the `new` keyword before the constructor, `this` will be set equal to the global object (`window`) and you will 
be unintentionally modifying the global object.

### The `this` Keyword  

When using constructors, `this` takes on a special meaning. When invoked with `new`, `this` is the object being constructed.

`this` is always the new object being constructed. And any properties set inside the constructor on `this` will be attached 
to the object that is constructed when you use the `new` keyword to instantiate it.

```javascript
function Car(){
console.log(this);
};

let car = new Car();
```

When adding properties to your object, you'll want to attach the properties to `this` inside the function.

## Properties & Methods  

When creating a Constructor, you can attach properties and methods to it. Then, when you create an instance, these 
properties will be copied on to it.

```javascript
// Defining the properties on a function:
function Car(make, model){
  'use strict';
  this.make = make;
  this.model = model;
}

// Using the Car constructor above, we can instantiate a new object and set the properties:

let myCar = new Car("Honda", "Civic");
```

In the above example, `myCar` has make and model properties since it is created based on the `Car` constructor.

Note:

* A constructor is always capitalized.

* Always use strict mode. Because of the `this` variable points to the global object `window` when not attached to an object, 
strict mode removes accidental or malicious access to the global object by assigning `this` a value of `undefined`. Strict 
mode prevents you from accidentally calling a constructor without the `new` keyword.

The return value of a constructor is one of two things:

1. If you *don't* return from a constructor, the value is `this`, which is set to an empty object.

2. If you do return from the constructor, then the return value is whatever you return from the constructor, as long as 
it's an object. (otherwise, it's `this`)

```javascript
function Car(){
  this.make = "Toyota";
}
//adding a property of 'make' and setting it to be 'Toyota'
// then instantiate the new object:
let newCar = new Car();
console.log(newCar.make); //logs Toyota

---------

function Car2(){
  this.make = "Toyota";
  return {make: "Subaru"};
}

newCar = new Car2();
console.log(newCar.make); //logs Subaru

---------

function Car3(make){
  this.make = make;
}

let newCar = new Car3("Ford");
console.log(newCar.make)//logs Ford
```

In the above example, the `Car2` object is returned with a make of 'Subaru', so the `this` object and properties bound to 
it simply get discarded.

In the example with `Car3`, the constructor is being set to take an argument (the make) which is passed into the 'make' 
property bound to `this`. When this object is instantiated, the argument 'Ford' is passed in and attached to the new object 
created, which `this` refers to inside the function.

Additionally, we can use prototypes to connect properties and methods to an object. This will be covered in detail in 
another lesson.

## Conclusion  

* a `function` + the `new` keyword equals a constructor

* in a constructor, `this` is the object being constructed

* constructors can be used to make multiple objects with the same properties and methods

* Beware, if you forget the `new` keyword, `this === window`

### References  

[Object Playground](http://www.objectplayground.com/)

---

# Prototypal Inheritance  

Prototypal inheritance, sometimes referred to as prototype-based inheritance or delegation, is a powerful tool in JavaScript. It allows JavaScript functions (in the form of functions, arrays, or most common objects) to pass properties and methods down to other functions using prototypes.

## How Does Inheritance Work?  

Think of the food chain you may have learned about in Biology class. Sitting at the top of the chain is the `Object.prototype`. All inheritance starts here and trickles down the prototype chain to each child given access to that chain.

## What is a Prototype?  

A prototype in JavaScript begins when we create a new function. JavaScript has a built in Object constructor that by default passes its prototypes to any new functions.

If you were to do the following, you can see what JavaScript has pre-baked into its Object constructor.

```javascript
let obj = new Object();
console.log(obj);
```

The above snippet would render a breakdown of the available properties and methods on the JavaScript Object.

![Object image](https://github.com/rickmurdock/notes/blob/master/ReactJS/images/object.png)

* You can see above is that every object has the ability to be a constructor as mentioned in our previous lesson on Constructors. That ability is because of prototypal inheritance.

## How Do Prototypes Work?  

A fundamental property of objects in JavaScript is that they are mutable, meaning they can be changed. This plays an important role in prototypal inheritance.

It begins with a prototype object that is an **object** designed with a specific set of properties and methods available to it.

Because of the mutability of objects and the ability for any function (object) to be a constructor, we can create new instances of the prototype object that carry on the properties of the original object - while still allowing us to add new properties and methods or change and override inherited ones if necessary.

## Inheritance in Action  

Let's take a look below at the constructor function `Fruit`.

```javascript
function Fruit() {
  this.sweet = true;
  this.hasSeeds = true;
}
```

We can give the `Fruit` constructor properties such as a `sweet`, by using `this.sweet = true`. We've also given it the property and value of `hasSeeds: true` because we are going to create more fruit, and we know that the majority of fruit will be sweet and have seeds.

Let's see inheritance in action. We'll create new constructors called `Apple`, `Pear`, and `Grape` that will become children of the Fruit constructor.

```javascript
function Apple() {
  this.texture = 'crunchy';
}

function Pear() {
  this.texture = 'weirdly crunchy and soft simultaneously';
}

function Grape() {
  this.hasSeeds = false;
}
```

The `Apple`, `Pear`, and `Grape` constructors do not inherit anything from the `Fruit` constructor. If we were to check if `Apple` was sweet by using `console.log(Apple.sweet)`, we would get back `undefined`. To make `Apple` sweet, we need to set up the prototypal inheritance. Using the `new` keyword let's inherit the `Fruit` object and access the `Fruit.prototype` which will link the chain together.

```javascript
Apple.prototype = new Fruit();

console.log('Apple.prototype:', Apple.prototype);
// output => 'Apple.prototype: Fruit {sweet: true}'
```

The `Fruit` constructor passed its properties down the prototypal chain to the new instance `Apple`. This is why `console.log` returns `Fruit` and its properties.

What about those other prototypes from the JavaScript Object? We haven't defined a `valueOf` method anywhere, but when we run the following code you can see the output below gives us a value! That's inheritance at work again.

```javascript
console.log(Apple.prototype.valueOf());
// output => Fruit {sweet: true}
```

## Continuing Prototypal Chains  

Prototypal chains can continue even further. Take a look at the following code:

```javascript
const apple = new Apple();
const pear = new Pear();
const grape = new Grape();

console.log('apple.texture:', apple.texture);
//output => 'apple.texture: crunchy'
console.log('pear.texture:', pear.texture);
//output => 'pear.texture: weirdly crunchy and soft simultaneously'
console.log('grape.hasSeeds:', grape.hasSeeds);
//output => 'grape.hasSeeds: false'
console.log('apple.sweet', apple.sweet);
//output => 'apple.sweet: true'
```

The reason `apple` is crunchy stems from it inheriting crunchy from the `Apple` constructor when the `const apple = new Apple();` was instantiated. It would be impossible without prototypal inheritance for `apple` to also be sweet without the declaration of `Apple.prototype = new Fruit();`.

We also learned that properties and methods can be overridden. `grape` returns `false` for the `hasSeeds` property because we gave the `Grape` constructor a property of being seedless.

### In the Wild  

Below is the code from above in runnable examples. Take a few minutes to run the code and play around with prototypal inheritance.

```javascript
function Fruit() {
  this.sweet = true;
  this.hasSeeds = true;
}

function Apple() {
  this.texture = 'crunchy';
}

function Pear() {
  this.texture = 'weirdly crunchy and soft simultaneously';
}

function Grape() {
  this.hasSeeds = false;
}

Apple.prototype = new Fruit();

console.log('Apple.prototype:', Apple.prototype);

console.log(Apple.prototype.valueOf());

const apple = new Apple();
const pear = new Pear();
const grape = new Grape();

console.log('apple.texture:', apple.texture);
console.log('pear.texture:', pear.texture);
console.log('grape.hasSeeds:', grape.hasSeeds);
console.log('apple.sweet', apple.sweet);
```

## Conclusion  

* Properties and methods can be passed down the prototype chain and inherited by their children functions.

* It's possible to override inheritance when necessary to do so, allowing us to blanket much of an objects properties with generalized statements, but make them more specific when needed.

* Inheritance allows us to program in a 'DRY' method, saving us from repeated tons of code by rewriting objects with the same properties.

* The prototype chain searches locally on a function and then traverses up the chain looking for matching prototype data.

### References  

* [Object Playground](http://www.objectplayground.com/)
* [Crockford's JavaScript](http://javascript.crockford.com/prototypal.html)
* [Stack Overflow Discussion](https://stackoverflow.com/questions/4166616/understanding-the-difference-between-object-create-and-new-somefunction)

---

# Functions & Prototypes  

You're already familiar with JavaScript functions, so in this section we'll discuss how to use prototypes to pass properties and methods to different functions.

### Defining a function  

As you know, a function is defined using the `function` keyword, followed by the name of the function, parentheses enclosing the parameters, and curly brackets encasing what the function does. For example:

```javascript
function addTwo(number){
  return number + 2;
}
```

Since functions are just specialized objects, we can add functions as methods inside an object. A function declaration without a name is called a `function expression`.

```javascript
let thing = function(param){
  return param;
}
```

We can also assign a function to a variable and pass that variable as an object. Since we can pass objects as regular variables, it makes sense that we can attach functions as properties of JavaScript objects. A function on an object is referred to as a `method`.

```javascript
  let thing = {
    one: function(){
      return makeMischief;
    },
    two: function(){
      return makeMoreMischief;
    }
  }
```

### Binding an object to the prototype  

Every function in JavaScript has a prototype. Since functions are objects, they inherit properties and methods from their prototypes, but methods can also be passed along in this way. In a previous lesson you learned that properties on objects can be passed using prototypal inheritance:

```javascript
function Fruit() {
  this.sweet = true;
  this.hasSeeds = true;
}

function Apple() {
  this.texture = 'crunchy';
}

Apple.prototype = new Fruit();

console.log('Apple.prototype:', Apple.prototype);
// output => 'Apple.prototype: Fruit {sweet: true}'
```

Here's an example of passing a function as a method on the prototype of an object:

```javascript
Fruit.prototype.biteMe = function(){
    console.log("once bitten, twice shy")
}

let pear = new Fruit();

console.log(pear.biteMe()); //logs "once bitten, twice shy"
```

### How `this` works in binding Methods  

Remember that with constructors `thi`s is always the new object being constructed. So any methods attached to `this` in a constructor are attached to that new object. This is where prototypes can come in handy. Let's say we wanted to add a method to a constructor:

```javascript
function Const(par1, par2){
  this.par1 = par1;
  this.par2 = par2;
  this.someMethod = function(){
    return this.par1 + " " + this.par2;
  };
}
```

While the above may look logical, there are two issues with writing it this way.

1. Binding a method to `this` inside the function object provides it only to that particular instance.

2. any method attached this way will get re-declared for every new instance created, negatively affecting memory usage if you are creating many instances.

Prototypes to the rescue!

If you apply the method to the object's prototype, it is only stored in the memory once, and all instances of that object will have access to that method.

```javascript
function Const(par1, par2){
  this.par1 = par1;
  this.par2 = par2;
}

Const.prototype.someMethod = function(){
  return this.par1 + " " + this.par2;
};
```

While there are advantages to using the prototype approach, how you write code will always depend on the situation. There may be times when you will need to access local private variables for a method.

### Conclusion  

Functions can be defined in a few different ways. When you attach a function onto an object it's called a method. Methods can be inherited the same way that properties can using prototypal inheritance.
