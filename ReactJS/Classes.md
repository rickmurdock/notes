# Classes vs. Constructors  

With the unveiling of ES2015 (often referred to as ES6) syntax, JavaScript made its way toward becoming a class based, object-oriented programming language, at least on the surface. Behind the scenes is a new method to create constructors using the class syntax. Let's take a look at some of the fundamental differences and similarities between class and constructor functions.

## What is a Constructor Function?  

We've already learned what constructor functions do and how they operate. Constructors allow us to create objects that will inherit methods and properties from parent objects using the prototypal chain.

Recall the fruit example from earlier:

```javascript
function Fruit() {
  this.sweet = true;
  this.hasSeeds = true;
}

function Apple() {

}

Apple.prototype = new Fruit();

let apple = new Apple();

console.log(apple.sweet);
```

We created a `Fruit` constructor function, defined an `Apple` constructor function and using the `Apple.prototype = new Fruit` 
allowed `Apple` to inherit from the prototypal chain of the `Fruit` constructor.

## What is a Class, and How Can it Help Us?

Classes are syntactic sugar for JavaScript constructors. They provide a means to create objects and deal with inheritance. Classes are special functions that can be expressions or declarations as with any function. Let's take a look at a class declaration. Below is a list of keywords we'll need to know moving forward.

* class

* constructor

* extends

* super

We'll also be taking note of JavaScript's hoisting properties and sub-classes

### Class Declaration  

Run the code below to see the expected output.

```javascript
class Fruit {
  constructor(sweet, texture) {
    this.sweet = sweet;
    this.texture = texture;
  }
}

let apple = new Fruit(true, 'crunchy');
console.log(apple.sweet);
console.log(apple.texture);
```

Above we've created the class `Fruit`. `Fruit` acts as our constructor function, and you'll see that we used the function 
`constructor` to set up how our constructor function would pass down properties. We left the `sweet` and `texture` properties 
empty and assigned each a value when we instantiated a `Fruit` instance with the variable `apple`. We passed the key value 
pairs to the `new Fruit` function and the result from the `console.log` statements reflect those values.

## Sub-Classes  

Classes provide us with an efficient and clean method to create inheritance. We can go even further by diving into the 
world of *sub-classes*. Let's take a look at sub-classes in action below.

```javascript
//Let's create a class called Primate
class Primate {
  constructor() {
    this.isMammal = true;
    this.isSmart = true;
    this.opposableThumbs = true;
  }
}

//Now let's create a SUB-CLASS called Monkey using the extends and super keywords
class Monkey extends Primate {
  constructor(name, color, isMammal, isSmart, opposableThumbs) {
    super({isMammal, isSmart, opposableThumbs});
    this.name = name;
    this.color = color;
  }
}

//Now let's create an instance of Monkey
let spiderMonkey = new Monkey("Spider Monkey", "black and brown");

//We've given the Monkey constructor some key value pair information
//Let's see if spiderMonkey inherited from the Primate constructor by using "super"

console.log(spiderMonkey.isMammal);
```

Let's take a look at the `extends` keyword. We're able to create a `Monkey` sub-class by using `class Monkey extends Primate`. 
`extends` does what we'd expect and extends a constructor's inheritance to the new constructor or instance.

Upon examining the `constructor` function in `Monkey`, you'll notice that we passed along several parameters. We gave `Monkey` 
new properties of `name` and `color` that weren't present in the `Primate` class and also passed along parameters we wanted 
carried from the `Primate` class, such as `isMammal`.

If we tried to reference them as they are, we'd get a reference error. This is where the `super` function helps us with 
inheritance. Using `super` and passing properties and methods from the parent constructor class allows us to retrieve those 
inherited properties and methods.

You'll see when we `console.log(spiderMonkey.isMammal)` or any of the other inherited traits the output we get in return is 
`true`.

### Static Methods on Classes  

The new `class` syntax also has access to `static` methods. Static methods are designed to live only on the constructor in 
which they are described and cannot be passed down to any children. We will also take a look at how to apply default 
properties to a constructor.

```
class Chameleon {
 static colorChange(newColor) {
   this.newColor = newColor;
 } //Let's set a default of green to the constructor
 constructor({ newColor = 'green'} = {}) {
   this.newColor = newColor;
 }
}

let pantherChameleon = new Chameleon({newColor: "purple"});

console.log(pantherChameleon.newColor);
```
