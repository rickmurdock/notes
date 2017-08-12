
Language Specialization: ReactJS
1. Single Page Applications  
2. React for SPAs  
3. Constructors  
4. Prototypal Inheritance  
5. Bind Functions to a Prototype  
6. Build System  
7. Nesting Components  
8. Iteration in React  
9. State and Props  
10. Implementing a Form on a React Component  
11. Rendering Children Components in React  
12. Pass data via props to children React components  
13. Author functions in a React component and bind them to the component  
14. Passing a Function Via Props  
15. Fetching JSON  
16. Styling Components with React  
17. Stateless Components  
18. React Router and Single Page Applications  
19. Hash Based Routing vs Resource Routing  
20. BrowserRouter  
21. How to Incorporate a Layout Component into Our React App  
22. Use Exact Path to Render a Specific Route  
23. Active Navigation Links with React Router  
24. Creating a Dynamic App with A Parent and Child Detail Page  
25. Application State  
26. React and Redux Workflow  
27. Authoring Actions  
28. Authoring a Reducer  
29. Using State to Model Events in Redux  
30. Authoring a reducer to filter data based on a user action  
31. Handling Asynchronous Actions in Redux  
32. Authentication in Redux  
33. Testing React applications using Jest  
View Course
Language Specialization: ReactJS Encyclopedia

The Encyclopedia is a compilation of all the lesson study notes in this course. Use it to look up the a term or concept that you have covered.

---

[Single Page Applications](SinglePageApplications.md)

# Single Page Applications  

At this point, you've learned the basics of front-end development. You've also learned to create multi-page websites combining HTML content, CSS styling, and JavaScript functionality. Multi-page websites make up the majority of websites visited, but there is a growing trend that favors single-page applications over the traditional multi-page applications. This lesson will define and explore the benefits of SPAs, or single-page applications.

## Terminology  

* Single-page application: Web application that loads a single HTML page and dynamically updates the page as the user interacts with the app.

* Multi-page application: A more traditional web applciation where each request made renders a new page in the browser.

## Examples  

SPAs

* Facebook

* Gmail

MPAs

* Reddit

## References  

* [Single-page application vs. multiple-page application](https://medium.com/@NeotericEU/single-page-application-vs-multiple-page-application-2591588efe58)

* [Stack Overflow](https://stackoverflow.com/questions/21862054/single-page-application-advantages-and-disadvantages)

---

[Single Page Applications](SinglePageApplications.md)
# React for SPAs  

React is a JavaScript library that allows the view to change when data changes. Only the necessary number of components are updated and rendered when data changes. Components are encapsulated and only care about their own state. Multiple components can be composed into more complex UIs.

What that does this mean? If we are authoring a messaging app, we would want a message to be rendered every time a new message hits the database. Most of the UI components don't need to be refreshed every time a message is added. React makes sure only the component with new data is refreshed, leaving the rest of the components as they are.

## Terminology  

* **React**: React is not really a framework, but a library for creating user interfaces. Unlike frameworks that rely on the MVC pattern in order to separate the view from the code, React focuses primarily on the concerns of the view using components.

* **Component**: Components let you divide the UI into smaller, reusable elements. These components are used to display data as it changes over time.

* **Virtual DOM**: React represents every DOM object with a corresponding virtual DOM object. A virtual DOM object is a representation of a DOM object, like a lightweight copy.

## References  

[Thinking in React](https://facebook.github.io/react/docs/thinking-in-react.html)

---

Lesson: Constructors and Prototypes
# Constructors  

## Terminology  

Constructor: method used to create and initialize an object created with a class. It accepts any number of arguments.

* Capitalized to distinguish from regular functions.

new operator: keyword used to create an instance of a built-in object type or a custom user-defined object.

Object.prototype.constructor: returns reference to the constructor function by sharing a property with all objects created by the function.

Prototype chain: inheritance chain that starts from the instantiated object and moves all the way up to the Object.prototype.

## Creating And Instantiating  

### Creating and object  

#### Example

```js
function Vehicle (engine,doors,tires) {
  this.engine = engine;
  this.doors = doors;
  this.tires = tires;
}
```

### Creating an instance  

#### Example

```js
var mustang = new Vehicle ("v-8", 2, 4);
```

```js
 1 function Vehicle (engine, doors, tires) {
 2   this.engine = engine;
 3   this.doors = doors;
 4   this.tires = tires;
 5 }
 6
 7 var mustang = new Vehicle ("v-8", 2, 4);
 8 console.log(mustang);
 9
10 // Practice makes perfect. Create a car of your choice by creating an instance of 'Vehicle' and passing the proper arguments.
```
 
> Forgetting to use new will change the scope from the object to the window. Therefore this will not operate as intended!
Adding Methods And Using Prototypes  

### Adding Methods  

#### Example

Ensure that all objects have access to a method by attaching it to a prototype. In this example we create a method what will sound the car's engine.

```js
 1 function Vehicle (make, model, engine, doors, tires, sound) {
 2   this.make = make;
 3   this.model = model;
 4   this.engine = engine;
 5   this.sound = "vroom!";
 6   this.doors = doors;
 7   this.tires = tires;
 8 }
 9   // Add a method
10 function engineSound(){
11     return "The" + " " + this.make + " " + this.model + "'s " + this.engine + " " + "goes" + " " +  this.sound;
12 }
13 // Attach the method to the constructor
14 Vehicle.prototype.engineSound = engineSound;
15
16 // Create an instance
17 var mustang = new Vehicle ("Ford", "Mustang","v-8", 2, 4);
18 // Call the 'sound' method
19 console.log(mustang.engineSound());
```

### Using Prototypes  

#### Example

In this example we use prototype to change the sound the car makes by overriding the constructor sound property.

```js
 1 function Vehicle (make, model, engine, doors, tires, sound) {
 2   this.make = make;
 3   this.model = model;
 4   this.engine = engine;
 5   this.sound = "vroom!";
 6   this.doors = doors;
 7   this.tires = tires;
 8 }
 9   // Add a method
10 function engineSound(){
11     return "The" + " " + this.make + " " + this.model + "'s " + this.engine + " " + "goes" + " " +  this.sound;
12 }
13 // Attach the method to the constructor
14 Vehicle.prototype.engineSound = engineSound;
15
16 // Change the sound the car makes.
17 Vehicle.prototype.changeSound = function(sound) {
18     this.sound = sound;
19 }
20 // Create an instance
21 mustang = new Vehicle ("Ford", "Mustang","v-8", 2, 4);
22
23 // Change the sound to "vroom vroom!"
24 mustang.changeSound("vroom vrooom!");
25 console.log(mustang.engineSound());
```

### Creating new objects and prototype inheritance  

```js
 1 function Person(name){
 2     this.name = name
 3 }
 4
 5 Person.prototype = {
 6     walk: function(){
 7       return this.name+' is walking like its hot'
 8     }
 9 }
10
11 function Student(name, test){
12     Person.call(this, name);
13     this.test = test
14 }
15 // make Student's prototype a Person instance
16 Student.prototype = new Person()
17
18 // then add on methods specifically to that Person instance
19 Student.prototype.takeTest = function(){ return this.name+' just aced: '+this.test }
20
21 // "Joe" is a student which inherits all properties from "Person" and "Student".
22 var joe = new Student("Joe", "Testing inheritance");
23
24 // Joe has his own method.
25 console.log("Joe", joe);
26
27 // Joe has access to Person's method
28 console.log("Joe walks:", joe.walk());
29 // Jane only inherits from "Person"
30
31 var jane = new Person("Jane");
```

---

Lesson: Constructors and Prototypes
# Prototypal Inheritance  

Prototypal inheritance, sometimes referred to as prototype-based inheritance or delegation, is a powerful tool in JavaScript. It allows JavaScript functions (in the form of functions, arrays, or most common objects) to pass properties and methods down to other functions using prototypes.

## Examples  

Inheritance in Action

```js
 1 function Fruit() {
 2   this.sweet = true;
 3   this.hasSeeds = true;
 4 }
 5
 6 function Apple() {
 7   this.texture = 'crunchy';
 8 }
 9
10 function Pear() {
11   this.texture = 'weirdly crunchy and soft simultaneously';
12 }
13
14 function Grape() {
15   this.hasSeeds = false;
16 }
17
18 Apple.prototype = new Fruit();
19
20 console.log('Apple.prototype:', Apple.prototype);
21
22 console.log(Apple.prototype.valueOf());
23
24 const apple = new Apple();
25 const pear = new Pear();
26 const grape = new Grape();
27
28 console.log('apple.texture:', apple.texture);
29 console.log('pear.texture:', pear.texture);
30 console.log('grape.hasSeeds:', grape.hasSeeds);
31 console.log('apple.sweet', apple.sweet);
```

## References  

* [Object Playground](http://www.objectplayground.com/)

* [Crockford's JavaScript](http://javascript.crockford.com/prototypal.html)

* [Stack Overflow Discussion](https://stackoverflow.com/questions/4166616/understanding-the-difference-between-object-create-and-new-somefunction)

---

Lesson: Constructors and Prototypes
# Bind Functions to a Prototype  

How to use prototypes to pass properties and methods to different functions.

## Examples  

Define a function

```js
function addTwo(number){
  return number + 2;
}
```

A function declaration without a name is called a function expression.

```js
function(param) {
  return param;
}
```

A function on an object is referred to as a *method*.

```js
  let thing = {
    one: function(){
      return makeMischief;
    },
    two: function(){
      return makeMoreMischief;
    }
  }
```

Properties on objects can be passed using prototypal inheritance:

```js
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

```js
Fruit.prototype.biteMe = function(){
    console.log("once bitten, twice shy")
}

let pear = new Fruit();

console.log(pear.biteMe()); //logs "once bitten, twice shy"
```

### How `this` works in binding Methods  

Add a method to a constructor with `this`:

```js
function Const(par1, par2){
  this.par1 = par1;
  this.par2 = par2;
  this.someMethod = function(){
    return this.par1 + " " + this.par2;
  };
}
```

### Prototypes  

If you apply the method to the object's prototype, it is only stored in the memory once, and all instances of that object will have access to that method.

```js
function Const(par1, par2){
  this.par1 = par1;
  this.par2 = par2;
}

Const.prototype.someMethod = function(){
  return this.par1 + " " + this.par2;
};
```

---

Lesson: Classses
## Constructor Function  

```js
 1 function Fruit() {
 2   this.sweet = true;
 3   this.hasSeeds = true;
 4 }
 5
 6 function Apple() {
 7
 8 }
 9
10 Apple.prototype = new Fruit();
11
12 let apple = new Apple();
13
14 console.log(apple.sweet);
```

## ES2015 Classes  

Classes are syntactic sugar for JavaScript constructors. They provide a means to create objects and deal with inheritance. Classes are special functions that can be expressions or declarations as with any function.

```js
 1 class Fruit {
 2   constructor(sweet, texture) {
 3     this.sweet = sweet;
 4     this.texture = texture;
 5   }
 6 }
 7
 8 let apple = new Fruit(true, 'crunchy');
 9 console.log(apple.sweet);
10 console.log(apple.texture);
```

## Sub-Classes  

```js
 1 //Let's create a class called Primate
 2 class Primate {
 3   constructor() {
 4     this.isMammal = true;
 5     this.isSmart = true;
 6     this.opposableThumbs = true;
 7   }
 8 }
 9
10 //Now let's create a SUB-CLASS called Monkey using the extends and super keywords
11 class Monkey extends Primate {
12   constructor(name, color, isMammal, isSmart, opposableThumbs) {
13     super({isMammal, isSmart, opposableThumbs});
14     this.name = name;
15     this.color = color;
16   }
17 }
18
19 //Now let's create an instance of Monkey
20 let spiderMonkey = new Monkey("Spider Monkey", "black and brown");
21
22 //We've given the Monkey constructor some key value pair information
23 //Let's see if spiderMonkey inherited from the Primate constructor by using "super"
24
25 console.log(spiderMonkey.isMammal);
```
 
### Static Methods  

```js
 1 class Chameleon {
 2  static colorChange(newColor) {
 3    this.newColor = newColor;
 4  } //Let's set a default of green to the constructor
 5  constructor({ newColor = 'green'} = {}) {
 6    this.newColor = newColor;
 7  }
 8 }
 9
10 let pantherChameleon = new Chameleon({newColor: "purple"});
11
12 console.log(pantherChameleon.newColor);
```

```js
 1 class Chameleon {
 2  static colorChange(newColor) {
 3    this.newColor = newColor;
 4 } //Let's set a default of green to the constructor
 5  constructor({ newColor = 'green'} = {}) {
 6    this.newColor = newColor;
 7  }
 8 }
 9
10 let pantherChameleon = new Chameleon({newColor: "purple"});
11 //Try calling the function colorChange from pantherChameleon.
12
13 pantherChameleon.colorChange("orange");
14 console.log(pantherChameleon.newColor);
``` 

---

Lesson: Modules and Build Tools
# Build System  

A build system is a set of tools designed to make the workflow of web development easier. Build systems contain tools that can be thought of in two categories: Installers and Task Runners.

### Installers  

Installers do what you might expect and install things. You've been using installers for some time now. npm (Node Package Manager) is a tool we employ in our bash terminal commands to install front-end libraries and dependencies - such as React.js, React Router, or Sass. npm can also help us install servers to run our development environment on (npm start) so we can see our project as we work on them. npm can even help us install other installation tools.

Other installers include Bower, Yeoman, and Brew.

### Task Runners  

These additions perform tasks for us and use their own special packages and plugins to complete the task. Tools such as Babel, webpack, Require.js, Browserify, Gulp, etc. are all considered task runners.

## Development vs. Production  

Development: The environment you develop your code in.

Production: The environment you run your code in.

---

Lesson: Modules and Build Tools
## Export to Import  

```js
//###### utility.js #######
export function makeMath(x,y) {
  return ( x * y );
}

//then on our index.js page...

//###### index.js ########
import makeMath from './utility.js';

console.log(makeMath(2,3));
//output => 6
```

## Import One or More  

```js
//###### utility.js #######
export function makeMath(x,y) {
  return ( x * y );
}

export function makeFood(meat, cheese) {
  return ("You've got a " + meat + " sandwich with " + cheese +  " cheese.");
}

//then on our index.js page...

//###### index.js ########
import {makeMath, makeFood} from './utility.js';

console.log(makeMath(2,3));
//output => 6

console.log(makeFood("salami", "gruyere"));
//output = > "You've got a salami sandwich with gruyere cheese"
```

## Import All  

```js
//###### utility.js #######
export function makeMath(x,y) {
  return ( x * y );
}

export function makeFood(meat, cheese) {
  return ("You've got a " + meat + " sandwich with " + cheese +  " cheese.");
}

//then on our index.js page...

//###### index.js ########
import * as utility from './utility.js';

console.log(utility.makeMath(2,3));
//output => 6

console.log(utility.makeFood("roast beef", "cheddar"));
//output = > "You've got a roast beef sandwich with cheddar cheese"
```

Above, you can see that we used the * operator and coupled it with a new keyword as to allow us access to all files being exported from our './utility.js' file.

It's essentially saying "I want to take everything from this file and name it as utility to access later on".

We simply need to place the utility. in front of any of the function, variables, etc that we plan to use and import from that utility.js file. For example: utility.makeFood("ham","swiss") Easy peasy lemon squeezy, right?

---

Lesson: ReactJS: Introduction
## Create React App  

* A library that helps us build React projects.

* Contains all of the tools that will take our React application and translate it into something the browser can understand.

* Provides us with a development server (A local server that hosts a web application in development on a set port and watches for changes made to the code base, refreshing in the browser.).

### Libraries included  

* **Create React App** includes Webpack for module bundling. Webpack takes modules with dependencies and emits static assets representing those modules.

* Babel is the library that **Create React App** includes to compile ES2015 and later JavaScript into a browser-ready version of JavaScript.

* Autoprefixer is included to parse CSS and add vendor prefixes to rules.

* ESLint is included as a JavaScript linting utility.

* Jest is included as a JavaScript testing solution.

### Installing  

```sh
npm install -g create-react-app
```

### Generating  

```sh
create-react-app the-name-of-my-application-here
```

### Development Server  

```sh
npm start
```

### Virtual DOM  

The Virtual DOM is a lightweight version of the DOM detached from the browser-specific implementation details. React works by constantly comparing the Virtual DOM to the actual DOM of our application.

When React sees a change it automatically re-renders the application for us. Data being received, data being given by a user, interface animations, all of these change the DOM. React listens and renders the application accordingly.

## Translating HTML into JSX  

Components are written in the language of JSX, which is XML syntax combined with JavaScript.

---

Lesson: ReactJS: Introduction
## Components  

A component is a chunk of code that is responsible for rendering a specific portion of the UI (user interface).

They function a lot like, well, JavaScript functions. They can accept inputs (called "props") and return elements describing what should appear on the screen.

# Nesting Components  

Components can be nested to increase the organization of our application.

They can even be imported from other files:

```jsx
//############### mainbody.js ###############
import React, { Component } from 'react';

//importing Components
import TopPara from './toppara.js'
import BottomPara from './bottompara.js'

export default class MainBody extends Component {
  render() {
    return (
      <div className="main-body">
        <TopPara /> // Nested Component
        <BottomPara /> // Nested Component
      </div>
    );
  }
}
```
---

Lesson: ReactJS: Introduction
## Iteration in React  

A huge part of React stems from the way that it handles data and re-renders the page according to that data.

React components can have components that render data and elements after iterating over that data.

Let's look at that simple application again. This application takes in an array of objects, and renders each object into it's own `<div>` on our website.

```jsx
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

let famousAstroPhysicists = [
  {
    id: 1,
    firstName: "Carl",
    lastName: "Sagan",
    alive: "false",
    nationality: "United States",

  },
  {
    id: 2,
    firstName: "Neil",
    lastName: "deGrasse Tyson",
    alive: "true",
    nationality: "United States",
  },
  {
    id: 3,
    firstName: "Stephen",
    lastName: "Hawking",
    alive: "true",
    nationality: "United Kingdom, England",
  },

];


export default class App extends Component {
  render() {
    let famedPhysicists = famousAstroPhysicists.map((physicist) => {
      return (
        <div key={physicist.id} >
          <h1>{physicist.lastName + ", " + physicist.firstName}</h1>
          <h2>Still alive: {physicist.alive}</h2>
          <h3>Nationality: {physicist.nationality}</h3>
        </div>
      )
    });
    return (
      <div className="App">
        {famedPhysicists}
      </div>
    );
  }
}
```

science.png

---

Lesson: ReactJS: Working wth Props and State
## React DOM  

React creates a **virtual DOM** by abstracting the **application DOM** and maintaining it in memory.

The Virtual DOM is used to handle in-page interactions and updates from the server.

This is accomplished by comparing the *new state* with the *previous state*.

## Component Lifecycle Methods  

### Mounting  

When the component is created (initialized) and then inserted into the DOM.

* `constructor`: called before the component is mounted

* `componentWillMount`: invoked immediately before mounting occurs (before render).

* `render`: required and must contain a single element

* `componentDidMount`: invoked immediately after a component is mounted.

### Updating  

A component is re-rendered because a props or state change occurs.

* `componentWillReceiveProps`: invoked before a mounted component receives new props.

* `shouldComponentUpdate`: invoked before rendering, when new props or state are being received.

* `componentWillUpdate`: invoked immediately before rendering when new props or state are being received

* `componentDidUpdate`: invoked immediately after updating occurs

### Unmounting  

Component is removed from the DOM.

* `componentWillUnmount`: invoked immediately before a component is unmounted and destroyed

### Other methods  

* `setState`: primary method used to update the user interface in response to event handlers and server responses.

* `forceUpdate`: will cause the component to re-render

---

Lesson: ReactJS: Working wth Props and State
# State and Props  

* Both props and state are plain JS objects

* Both props and state changes trigger a render update

## Props  

In React, *props* (short for properties) are a component's configuration.

* received from above

* immutable

* considered the "options" of the component

## State  

In React, the *state* is a serializable representation of one point in time.

COMPONENT TYPES

* **Stateless Component** — A stateless component has only props, no state.

* **Stateful Component** — A statefull component has both props and state.

---

Lesson: ReactJS: Working wth Props and State
## Implementing a Form on a React Component  

```jsx
class Form extends React.Component {
  constructor(props){
    super(props);

    this.handleNameChange = this.handleNameChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);

    this.state = {name: ''};
  }
  handleNameChange(event){
    this.setState({name: event.target.value});
  }
  handleSubmit(event){
      event.preventDefault();
      alert('Thank you, ' + this.state.name + ' your name was submitted');
  }
  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Your Name:
          <input onChange={this.handleNameChange} name="name" type="text" value={this.state.name}/>
        </label>
        <input type="submit" value="Submit"/>
      </form>
    )
  }
}
```

---

Lesson: ReactJS: Children Components
## Rendering Children Components in React  

## Examples  

`{this.props.children}` is used to pass components or elements into a parent component component. A parent component doesn't have any inherent awareness of nested components.

`{this.props.children}` allows for a component to render things passed into it, and doesn't require that the component even reside inside of the same script sheet.

```jsx
// baselayout.js

import React, {Component} from 'react';

export default class BaseLayout extends Component {
  constructor(props) {
    super(props);
  }
  render(){
    return (
      <div className="base">
        <nav className="navbar">
          <h3>Navigation Bar: A cocktail lounge for coders</h3>
        </nav>

        {this.props.children}

        <footer>
          <h3>This is the BaseLayout footer</h3>
        </footer>
      </div>
    );
  }
}
```

Now, we'll use the `<BaseLayout></BaseLayout>` component inside our `App` component. `<BaseLayout></BaseLayout>` will contain its own nested elements.

```jsx
// App.js

import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

//import components
import BaseLayout from './Components/baselayout';

export default class App extends Component {
  constructor(props){
    super(props);
  }
  render() {
    return (
      <BaseLayout>
        <div className="main">
          <h4>This is the body!</h4>
        </div>
      </BaseLayout>
    )
  }
}
```

## References  

[React Components](https://facebook.github.io/react/docs/composition-vs-inheritance.html)

---

Lesson: ReactJS: Children Components
# Pass data via props to children React components  

With React, each component should be responsible for a minimal number of elements and minimal functionality. Data management and distribution throughout a project should be handled by multiple components. We should have a lot of "presentational" components that inherit their properties from "container" components. Each should be compartmentalized so that the code can be accessed and reused in the project multiple times if need be.

## Examples  

`<App />` is a "container" component which renders a "presentational" component within. It saves user input to its `state` and passes the data to `<StudentList />` via a `students` property.

```jsx
export default class App extends Component {
  constructor(props){
    super(props);

    this.handleName = this.handleName.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);

    this.state = {
      students: [],
      name: "",
    };
  }

  handleName(e){
    this.setState({name: e.target.value});
  }

  handleSubmit(e) {
    e.preventDefault();
    this.state.students.push({studentName: this.state.name});
    this.setState({students: this.state.students, name: ""});
  }


  render() {
    return (
      <div className="class-maker">
        <h3>Create Your Class List Here: </h3>
        <form onSubmit={this.handleSubmit}>
          <input type="text" onChange={this.handleName} value={this.state.name}/>
          <input type="submit" value="Add Student" />
        </form>
        <StudentList students={this.state.students}/>
      </div>
    );
  }
}
```

## Setting State and Sharing Props  

### Setting Initial State  

Here we assign the initial values of the app's state.

```jsx
export default class App extends Component {
  constructor(props){
    super(props);

    // Method binding. We will explain this in detail in the next lessons.
    this.handleName = this.handleName.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);

    // Set initial state:
    this.state = {
      students: [],
      name: "",
    };
  }
  // Handler and render methods...
}
```

* That simply lets the component know what changes it needs to keep track of. In this case, we want it to track an empty array called "students" and a variable called "name" that is initially set to an empty string.

### Changing State  

```jsx
// render() method in <App /> component.
render() {
  return (
    <div className="class-maker">
      <h3>Create Your Class List Here: </h3>
      <form onSubmit={this.handleSubmit}>
        <input type="text" onChange={this.handleName} value={this.state.name}/>
        <input type="submit" value="Add Student" />
      </form>
      <StudentList students={this.state.students}/>
    </div>
  );
 }
}
```

* When one of these values changes in a component, the state has to be updated by using the this.setState() method.

* We have a <form>that contains a text <input /> and a <input type="submit" /> that functions as our submit button.

* On our form we have a onSubmit property and have given it a value of {this.handleSubmit}. We'll go more into this later. For now, just take note that this is happening.

* On our <input type="text" /> we have an onChange= property that has a value of {this.handleName} and we've given the input itself a value of value={this.state.name}.

* We need that value from the input because that is how we will grab the name that the teacher types into the input.

### Methods  

```jsx
export default class App extends Component {
  constructor(props){
    super(props);

    this.handleName = this.handleName.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);

    this.state = {
      students: [],
      name: " ",
    };
  }
  handleName(e){
    this.setState({name: e.target.value});

  }
  handleSubmit(e) {
    e.preventDefault();
    this.state.students.push({studentName: this.state.name});
    this.setState({students: this.state.students, name: " "});

  }
  // Render method...
}
```

* We have a constructor(props) with super(props) called inside of it. This should be familiar as all components are created as a class should have a base constructor that receives props.

* We'll examine the two unfamiliar expressions this.handleName = this.handleName.bind(this); and this.handleSubmit = this.handleSubmit.bind(this); in the next lesson. For now, just understand that we are giving the two methods access to this.

* Inside of handleName we pass an argument e that is our event object. The onChange property listens for changes to the value of the ________ and fires each time a key stroke is registered.
 
* We then take the e.target.value which gives us the value correlated to our input and store it in the state of "name" : like so, this.setState({name: e.target.value});.

* This simply updates our state which can be retrieved throughout our component at any time.

* handleSubmit, and see that we again pass it an event object using the shorthand e.

* This allows us to call the e.preventDefault() method which prevents the form from trying to POST to the server.

* Next, we grab the current (state) value of our name and push that value into our student's array:

```jsx
this.state.students.push({studentName: this.state.name});
```

* We pass the array an object with the property studentName and the value this.state.name. This ensures that each time we hit submit, a new name will be added to our array.

```jsx
this.setState({students: this.state.students, name: ""});
```

* We update the state using setState to make sure we have saved our changes.

* Update the value of our name state back to an empty string (this clears our input box, since it's value is the value of this.state.name).

## Passing Props to Child Components  

### Parent Component  

```jsx
return (
  <div className="class-maker">
    <h3>Create Your Class List Here: </h3>
    <form onSubmit={this.handleSubmit}>
      <input type="text" onChange={this.handleName} value={this.state.name}/>
      <input type="submit" value="Add Student" />
    </form>
    <StudentList students={this.state.students}/>
  </div>
);
```

* The <StudentList/> is given a property students and passed the value of this.state.students.

  * This gives access to the state properties to <StudentList /> component in the form of props.
  
### Child Component  

```jsx
class StudentList extends Component {
  constructor(props){
    super(props);

  }
  render(){
    let kids = this.props.students.map((student, index) => {
      return (
        <li key={index}>{student.studentName}</li>
      )
    })
    return(
      <div>
        <h4>My Class:</h4>
        <ul>
          {kids}
        </ul>
      </div>
    );
  }
}
```

* Make sure to pass the constructor() and super() with props to the component.

```jsx
this.props.students.map((student, index) => {
  return (
    <li key={index}>{student.studentName}</li>
  )
})
```

* Inside of that method we map over this.props.students (which was passed down from the <App /> components in the form of this.state.students).

* We extract each student from that array and return an index and student as a list item : <li key={index}>{student.studentName}</li>.

* The student.studentName give us access to the studentName property inside of our students array.

* We know that each mapped item needs a unique key, so we use the index of each item being mapped over for simplicity sake. key={index}.

* We then insert each mapping using bracket notation with{kids}. That is put into the return statement where the <ul> {kids} </ul> is placed. 

### References 

[React Docs](https://facebook.github.io/react/docs/state-and-lifecycle.html) 
[Presentational and Container Components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)

---

Lesson: ReactJS: Children Components
# Author functions in a React component and bind them to the component  

In Javascript this functions differently based on the scenario it's used in. In a constructor, you use the new keyword to create a new object. In this case, this always refers to the new object being constructed. We've also talked about the reasons to use strict mode, as otherwise this gets set to the global object window. When a function is defined as a property of an object, it's called a method. In those cases, you can use this to bind properties to their parent component. The confusion arises because unlike regular variables, the this keyword does not have a scope, meaning nested functions do not inherit the this value of their parent function automatically. When you invoke a method inside of another function or component, the this value for the method is set to the object it was invoked upon, not the object it was invoked inside of.

## Examples  

```jsx
export default class List extends Component {
  constructor(props){
    super(props);

    this.handleTopping = this.handleTopping.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);

    this.state = {
      pizza: [],
      topping: "",
    };
  }
  handleTopping(e){
    this.setState({topping: e.target.value});

  }
  handleSubmit(e) {
    e.preventDefault();
    this.state.pizza.push({pizzaTopping: this.state.name});
    this.setState({pizza: this.state.pizza, topping: ""});

  }
  render() {
    return (
      <div className="pizza-maker">
        <h3>Favorite Pizza Toppings List: </h3>
        <form onSubmit={this.handleSubmit}>
          <input type="text" onChange={this.handleTopping} value={this.state.topping}/>
          <input type="submit" value="Add Topping" />
        </form>
        <ToppingsList pizza={this.state.pizza}/>
      </div>
    );
  }
}
```

In the above example, we're allowing the user to input their favorite toppings for pizza. We created an event handler to handle the user input of toppings called handleTopping. We invoked that handler on this inside the render method using the onChange function. We want the this context to come from the top level component. This is important because we want that handler (method) to have access to the properties, state and methods of the component. To ensure this is bound to the correct context, we explicitly write a line of code in the constructor method to bind our event handler to the this of the component: this.handleTopping = this.handleTopping.bind(this);.

### Other Options  

Within an event listener

```jsx
<input type="text" onChange={this.handleTopping.bind(this)} value={this.state.topping}/>
```

Arrow function:

```jsx
onChange={e => this.handleTopping(e)}
```

Expression Arrow Function

```jsx
handleTopping = (e) => {
  this.setState({topping: e.target.value})
}
```

### References  

* [Todd Motto](https://toddmotto.com/understanding-the-this-keyword-in-javascript/)

* [GitHub Gist](https://gist.github.com/amitai10/adb66d6faa714e8c3cdb94946bb98356)

---

Lesson: ReactJS: Children Components
# Passing a Function Via Props  

* Passing a function via props works exactly the same way the passing any other data via props would work.

* The child component can actually call and fire the function on the parent component.

* Using this method allows state to reside in the parent component and not be spread into more components than necessary.

## Examples  

Parent component ("container" component)

```jsx
//An array of colors for us to randomly sort through and apply to the background color
let colors = ["blue", "green", "red", "yellow", "orange", "purple", "pink", "tomato"];

export default class App extends Component {
  constructor(props){
    super(props);
    this.state = {
      color: ""
    }
    this.changeColor = this.changeColor.bind(this);
  }
  changeColor() {
    let num = Math.floor(Math.random()* colors.length);
    this.setState({color: colors[num]});
  }
  render(){
    return (
      <div className="main" style={{backgroundColor: this.state.color ? this.state.color : "red"}}>
        <h1>Color Me Badd</h1>
        <ColorMaker changeColor={this.changeColor} />
      </div>
    );
  }
}
```

Child component ("presentational" component)

```jsx
class ColorMaker extends Component {
  constructor(props) {
    super(props);
  }
  render() {
    return (
      <div className="color-change-div">
        <button onClick={this.props.changeColor}>Change Color</button>
      </div>
    )
  }
}
Within `<ColorMaker />`, `this.props.changeColor` refers to the `changeColor` method in the `<App />` component.
```

## References  

* [React Docs](https://facebook.github.io/react/docs/state-and-lifecycle.html)

---

Lesson: ReactJS: Advanced Techniques
# Fetching JSON  

Using `fetch` to pull outside JSON data into our applications from an existing API.

* We can use the fetch method in React in order to make a network request.

* We use then to manage our promises and make sure our network request is completed.

* We can use catch to listen for error messages from the network.

* We can store our data in state, using setState inside of our promises.

* componentDidMount is the ideal method in which to make network requests.

* state can be passed down as props to children components.

## Examples  

The `App` component ("container" component) performs the `fetch` and passes data down to `CharacterList` ("presentation" component) via `state`. `CharacterList` has access to the data via `this.props.people`.

```jsx
export default class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      characters: [],
    }
  }
  componentDidMount() {
    fetch('http://swapi.co/api/people/')
    .then(results => results.json())
    .then(responseData => {
      this.setState({characters: responseData.results});
    })
    .catch((error) => {
      console.log("Error with Fetching : ", error);
    });

  }
  render() {
    console.log("characters", this.state.characters);
    return (
      <div className="main">
        <CharacterList people={this.state.characters}/>
      </div>
    );
  }
}
```

```jsx
class CharacterList extends Component {
  constructor(props) {
    super(props);

  }
  render() {
    let peeps = this.props.people.map((person, index) => {
      return (
        <li key={index} className="personList">
          <div className="persona">
            <h4 className="name">{person.name}</h4>
            <h5 className="gender">Gender: {person.gender}</h5>
            <h5 className="eyes">Eye Color: {person.eye_color}</h5>
            <h5>Hair Color: {person.hair_color}</h5>
            <h5>Height: {parseInt(person.height) / 100} meters</h5>
            <h5>Mass: {person.mass} kilograms</h5>
            <h5>Appears in {person.films.length} films.</h5>
          </div>
        </li>
      )
    })
    return (
      <div className="characters">
        <ul className="character-ul">
          {peeps}
        </ul>
      </div>
    )
  }
}
```

## References  

* [MDN Then Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)

* [MDN Body.json()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)

* [MDN Using Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)

* [StarWars API : SWAPI](https://swapi.co/)

* [React Component Lifecycles](https://facebook.github.io/react/docs/react-component.html#componentwillreceiveprops)

---

Lesson: ReactJS: Advanced Techniques
# Styling Components with React  

React's foundation centers around modularizing (creating stand-alone pieces of code) code to keep everything self-contained. React JSX components allow us to write Javascript and HTML in a single file, and takes the same approach when it comes to styling. There are just a few new rules and concepts to abide by:

* It's common practice in React to create self-contained components.

* Styling can take place by using the `className` attribute and creating a stylesheet to transform our component.

* We can also style directly inline with React using a few approaches.

* Multiword style properties should obey the camelCase (`backgroundColor`) naming convention. Exceptions are the `data-` and `aria-` properties, which remain separated by a dash.

## Examples  

In HTML, as in JSX, we have the ability to designate a class (HTML) or classNames (JSX). Inside of the `render` method before the `return` statement we can assign styling to a variable and pass that variable to elements inside of the component. Adding a `style` attribute to the element allows us to pass a variable in as the value using `{}` brackets: `style={yourVariableHere}`. This method is a great choice when dealing with a lot of styles or a larger component.

```jsx
class App extends Component {
  constructor(){
    super();
  }
  render(){
    let headerStyle = {
      color: "red",
    };
    let boxStyle = {
      backgroundColor: "blue",
    };
    return (
      <body>
        <nav>
          <header className="header" style={headerStyle}>Header</header>
        </nav>
        <div className="box" style={boxStyle}>
          <h3 className="title">An Interesting Title</h3>
        </div>
        <div className="box" style={boxStyle}>
          <h3 className="title">An Interesting Title</h3>
        </div>
        <div className="box" style={boxStyle}>
          <h3 className="title">An Interesting Title</h3>
        </div>
      </body>
    )
  }
}
```

References  

* [Kirupa.com](https://www.kirupa.com/react/styling_in_react.htm)

* [React Docs](https://facebook.github.io/react/docs/dom-elements.html)

* [React JSX](https://facebook.github.io/react/docs/jsx-in-depth.html)

---

Lesson: ReactJS: Advanced Techniques
# Stateless Components  

State isn't required in React components. In fact, you can have components that rely only on the props. If you're not changing any data in the component, then there's no need for state - simply use props to pass static data.

## Terminology  

* **Stateless Component** — Only props, no state. There's not much going on besides the render() function whose logic revolves around the props they receive. This makes them very easy to follow and test.We sometimes call these **presentational components**.

* **Stateless Functional Component** - If your component only has a render method, you can write it as a stateless functional component, and your function will be passed `props` as its first argument

* **Stateful Component** — Both props and state. We also call these state managers. They are in charge of client-server communication, processing data, and responding to user events. We sometimes call these **container** components.

## Examples  

### Stateless Component  

```jsx
class List extends Component {
  render() {
  return (
    <div>
      <h3>Favorite Movie List: </h3>
      <ul className="list-group">
        {this.props.movies.map((movie, index)=>{
          return (
            <li className="list-group-item" key={movie.title}>
            <h3>{movie.title}</h3>
            </li>
          )
        })}
      </ul>
    </div>
    );
  }
}
```

### Stateless Functional Component  

```jsx
const List = ({movies}) => {
  return (
    <div>
      <h3>Favorite Movie List: </h3>
      <ul className="list-group">
        {movies.map((movie, index)=>{
          return (
            <li className="list-group-item" key={movie.title}>
            <h3>{movie.title}</h3>
            </li>
          )
        })}
      </ul>
    </div>
    );
  }
}
```

### Stateful Component  

```jsx
export default class List extends Component {
  constructor(props){
    super(props);

    this.handleMovie = this.handleMovie.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);

    this.state = {
      movies: [],
      title: "",
    };
  }
  handleMovie(e){
    this.setState({title: e.target.value});

  }
  handleSubmit(e) {
    e.preventDefault();
    this.state.movies.push({movieTitle: this.state.title});
    this.setState({movie: this.state.movies, title: ""});

  }
  render() {
    return (
      <div className="movie-list">
        <h3>Favorite Movie List: </h3>
        <form onSubmit={this.handleSubmit}>
          <input type="text" onChange={this.handleMovie} value={this.state.title}/>
          <input type="submit" value="Add Movie" />
        </form>
        <MovieList movie={this.state.movies}/>
      </div>
    );
  }
}
```

## References  

* [JSPlayground](https://javascriptplayground.com/blog/2017/03/functional-stateless-components-react/)

* [Todd Motto](https://toddmotto.com/stateful-stateless-components)

* [Dan Abramov](https://medium.com/@dan_abramov/react-components-elements-and-instances-90800811f8ca)

---

Lesson: React-Router: Part 1
# React Router and Single Page Applications  

## Terminology  

* Single-page application: Web application that loads a single HTML page and dynamically updates the page as the user interacts with the app.

## Examples  

### Installing React Router  

```sh
npm install --save react-router-dom
```

### Example Routes  

```jsx
<Route path="/" component={App}/>
<Route path="/page_one" component={PageOne} />
<Route path="/page_two" component={PageTwo} />
```

## References  

* [React Router](https://reacttraining.com/react-router/web/guides/quick-start)

---

Lesson: React-Router: Part 1
# Hash Based Routing vs Resource Routing  

## Terminology  

* **Hash Based Routing**: Routing method that uses the hash symbol to designate the path that the router should direct the browser to.

## Examples  

Given the URL `http://myrecipes.com/#/hashbrowncasserole` let's look at Hash based routing. The base URL `myrecipes.com` will be sent to the server, the fragment identifier `/hashbrowncasserole` will be stored in local storage. JavaScript code is returned to the browser after the initial page is pulled, prompting the browser to then send back the fragment as a query to the server.

## References  

* [Basic resource routing](https://expressjs.com/en/starter/basic-routing.html)

* [Client Server](https://en.wikipedia.org/wiki/Client%E2%80%93server_model)

* [Simple summary front-end vs back-end routing](https://medium.com/@kennedyjt88/frontend-routing-vs-backend-routing-874b2bc41e5a)

---

Lesson: React-Router: Part 1
# BrowserRouter  

`<BrowserRouter>` is a React component that contains the history API.

## Terminology  

* **Exclusive rendering**: rendering a route based on strict matching of a path.

* **Inclusive rendering**: rendering of routes based on meeting a partial match of the path allowing for multiple routes to be displayed.

## Examples  

```jsx
Using BrowserRouter:
//######### index.js #############

//import React
import React from 'react';
import ReactDOM from 'react-dom';

//import Styles
import './styles/index.css';

//import React Router
import {BrowserRouter, Route, Switch} from 'react-router-dom';

//import Components
import App from './scripts/components/App';
import PageOne from './scripts/components/page_one';
import PageTwo from './scripts/components/page_two';


//wrap our <Route> components with <BrowserRouter> component.
ReactDOM.render(
 <BrowserRouter>
   <Switch>
     <Route path="/page_one" component={PageOne} />
     <Route path="/page_two" component={PageTwo} />
     <Route path="/" component={App}/>
   </Switch>
 </BrowserRouter>

, document.getElementById('root'));
```

## References  

* [Switch Component](https://reacttraining.com/react-router/web/api/Switch)

* [BrowserRouter](https://reacttraining.com/react-router/web/api/BrowserRouter)

---

Lesson: React-Router: Part 2
# How to Incorporate a Layout Component into Our React App  

## Terminology  

* **Layout component**: A component that can render child components inside of itself.

## Examples  

### Layout Component  

```jsx
//############ base-layout.js ##############
import React, { Component } from 'react';

export default class BaseLayout extends Component {
  render() {
    return (
      <div>
        <div className="header">
          <h1>Bill Murray</h1>
          <p className="bill">In Bill We Trust</p>
        </div>
        {this.props.children}
        <footer><h5>Murray Rocks</h5></footer>
      </div>

    )
  }
}
```

### Index File  

```jsx
import registerServiceWorker from './registerServiceWorker';

//import React
import React from 'react';
import ReactDOM from 'react-dom';

//import Styles
import './styles/index.css';

//import React Router
import {BrowserRouter, Route, Switch} from 'react-router-dom';

//import Components
import App from './scripts/components/App';
import PageOne from './scripts/components/page_one';
import PageTwo from './scripts/components/page_two';
import BaseLayout from './scripts/components/base-layout';

ReactDOM.render(
  <BrowserRouter>
    <BaseLayout>
      <Switch>
        <Route path="/page_one" component={PageOne} />
        <Route path="/page_two" component={PageTwo} />
        <Route path="/" component={App}/>
      </Switch>
    </BaseLayout>
  </BrowserRouter>

  ,
  document.getElementById('root'));
registerServiceWorker();
```

---

Lesson: React-Router: Part 2
# Use Exact Path to Render a Specific Route  

An *index route* holds the base components for the application and generally is the place first visited by the user. Frequently, the route that corresponds to the index route is a simple `/`, this means that in the address bar of the browser, a user will simply see the base URL. For example, if we had a website `www.thisisawebsite.com/` the trailing `/` would indicate that we were on our index route.

* Using exact path guarantees that a certain route will render when the endpoint matches exactly.

* exact path is a means for rendering our index route of an application.

* We can't use exact path for dynamic routes.

## Examples  

When we declare an index route, we can use the exact path= attribute to ensure that our path will be met only under strict and exact standards.

```jsx
class App extends Component {
  render() {
    return (
      <BrowserRouter>
        <BaseLayout>
          <Switch>
            <Route exact path="/" component={HomeMenu} />
            <Route path="/people/:actor" component={ActorInfo} />
            <Route path="/people" component={PeopleMenu} />
          </Switch>
        </BaseLayout>
      </BrowserRouter>
    );
  }
}
```

## References  

* [Exact Path](https://reacttraining.com/react-router/web/api/Route/exact-bool)

---

Lesson: React-Router: Part 2
# Active Navigation Links with React Router  

Active links progammatically highlight when the url route of the application also matches the active link target. They are commonly used in application navigation and assist a user in wayfinding.

# Terminology  

* `<NavLink>` comes with some key attributes, the most commonly used are the `isActive` and `activeClassName`.

* `activeClassName` is the class that gets appended to the component classes when the link matches the pathname for the URL.

* `activeStyle` takes an object `{}` filled with style key value pairs.

* `isActive` takes an anonymous function that is used to perform additional verification of whether the link is "active".

# Examples  

```jsx
<NavLink
  activeStyle={{
    color: "blue",
    backgroundColor: "white"
  }}>Main Page</NavLink>
```
  
```jsx
//############## nav.js ################
import React, { Component } from 'react';

import { NavLink } from 'react-router-dom';

export default class NavBar extends Component {
  render() {
    return (
      <div>
        <nav>
          <NavLink activeClassName="selected" className="nav-link" exact to="/">Main</NavLink>
          <NavLink activeClassName="selected" className="nav-link" to="/page_one">One</NavLink>
          <NavLink activeClassName="selected" className="nav-link" to="/page_two">Two</NavLink>
        </nav>
        {this.props.children}
      </div>

    )
  }
}
```

## References  

* [React Training](https://reacttraining.com/react-router/web/api/NavLink/isActive-func)

---

Lesson: React-Router: Part 3
# Creating a Dynamic App with A Parent and Child Detail Page  

## Terminology  

* **Template literal** - String literals allowing embedded expressions. You can use multiline strings and string interpolation features with them.

## Examples  

### Rendering component with ReactDOM  

```jsx
//########### index.js ##############
//import React
import React from 'react';
import ReactDOM from 'react-dom';

//import Styles
import './styles/index.css';

//import Components
import App from './scripts/components/App';

ReactDOM.render(
  <App />,
  document.getElementById('root'));
```

### Router Set Up  

```jsx
//######## App.js #########
import React, { Component } from 'react';
import '../App.css';

//import React Router
import { BrowserRouter, Route, Switch } from 'react-router-dom';

//component imports
import HomeMenu from './home';
import BaseLayout from './base_layout';
import PeopleMenu from './people';
import ActorInfo from './actor_info';

class App extends Component {
  render() {
    return (
      <BrowserRouter>
        <BaseLayout>
          <Switch>
            <Route exact path="/" component={HomeMenu} />
            <Route path="/people/:actor" component={ActorInfo} />
            <Route path="/people" component={PeopleMenu} />
          </Switch>
        </BaseLayout>
      </BrowserRouter>
    );
  }
}

export default App;
```

## References  

* [MDN](https://reacttraining.com/react-router/web/example/basic)

---

Lesson: Redux: Part 1
# Application State  

*Application state* does not refer to the state of each component but rather the data that flows through the entire application. The real magic of Redux is how it handles data and manages what actions should be taken when events are triggered.

## Is A State Management Library Right For My Project?  

Here a few questions we can ask to determine whether an application state library would benefit your project:

1. Do other parts of the application care about the data in question?

2. Do we need to create more data based on the data in question?

3. Is the data in question being used to create multiple components?

4. Is there value in having a record of the changes to the state in your application? (i.e. Would it help you debug your application if you could track events as you test?).

5. Do you want to cache (hold on to) your data instead of using multiple requests from another API?

## References  

* [Redux JS: Organizing State](http://redux.js.org/docs/faq/OrganizingState.html)

* [Flux](https://facebook.github.io/flux/)

* [Redux ReadME](http://redux.js.org/)

---

Lesson: Redux: Part 1
# React and Redux Workflow  

## Folder Structure  

Our file tree should look something like this:

filetree.png

## Installing Dependencies  

```sh
npm install --save redux
npm install --save react-redux
```

The first command will install Redux. The second command installs React-Redux — a package that allows React and Redux to work together.

## React and Redux Workflow  

A lot is going on within a React application, from application state to components and component state. Imagine a simple app where a user clicks an option which displays data on another screen. This is the flow of events that take place when Action Creator is triggered:

react-redux-workflow1.jpg

Overview  

* The React and Redux workflow cycle is fairly complicated but centers around updating application state.

* A data change or event triggers an Action creator.

* The Action Creator creates an Action.

* An Action is the payload of information and data regarding what happened.

* A dispatcher handles the Action and directs it to the proper Reducer function inside of our Store.

* The reducer function is a pure function that pairs an action with an appropriate piece of application state and updates it accordingly.

* Application state is never mutated, but rather copied and returned as a new object.

* The new application state is sent back to React and utilized by our Container components.

* Container components are the bridge between React and Redux and determine what should be done inside of React with the newly created application state.


## References  

* [Redux Basics](http://redux.js.org/docs/basics/)

* [Redux diagrams GitHub](https://github.com/reactjs/redux/issues/653)

---

Lesson: Redux: Part 1
# Authoring Actions  

## Examples  

### Example `actions.js`

```jsx
const USER_SELECTED = 'USER_SELECTED';

export function selectUser(user) {
  return {
    type: USER_SELECTED,
    payload: user
  };
};
```

### Example Application  

```jsx
export const ADD_USER = 'ADD_USER';
export const DELETE_USER = 'DELETE_USER';
export const UPDATE_SCORE = 'UPDATE_SCORE';

export const addUser = userName => {
  return {
    type: ActionTypes.ADD_USER,
    payload: userName
  };
};

export const deleteUser = userId => {
  type: ActionTypes.DELETE_USER,
  payload: userId
};

export const UPDATE_SCORE = score => {
  type: ActionTypes.UPDATE_SCORE,
  payload: score
};
```

`user_list.js`  

```jsx
//################ containers/user_list.js ################

//normal react imports
import React, { Component } from 'react';

//the glue that binds React to Redux so they can communicate
import { connect } from 'react-redux';

//our action creator to select a user
import { selectUser }  from '../actions';

// The props that UserList receives come from our state.
// the mapStateToProps function below does this for us.
class UserList extends Component {
  render() {
    let userNames = this.props.users.map((user) => {
      return (
        <li
          key={user.name}
          onClick={() => this.props.selectUser(user)}>{user.name}</li>
      );
    });
    return (
      <ul>
        {userNames}
      </ul>
    );
  }
}

function mapStateToProps(state) {
  //what is returned will show up as props inside of the UserList container
  //inside of this function we generally return an object
  return {
    users: state.users,
  };
}

function mapDispatchToProps(dispatch) {
  // whenever selectUser is called, the result should be passed to
  // the reducer.
    return {
      selectUser: user => {
        dispatch(selectUser(user))
      }
    }
}

// Promotes UserList from component to container.
// This connects our functions to our container component.
export default connect(mapStateToProps, mapDispatchToProps)(UserList);
```

### `mapDispatchToProps` Function  

```jsx
function mapDispatchToProps(dispatch) {
  // whenever selectUser is called, the result should be passed to
  // the reducer.
    return {
      selectUser: user => {
        dispatch(selectUser(user))
      }
    }
}
```

### Connecting `mapStateToProps` and `mapDispatchToProps` to our Component  

```jsx
// Promotes UserList from component to container.
// This connects our functions to our container component.
export default connect(mapStateToProps, mapDispatchToProps)(UserList);
```

`user_detail.js`

```jsx
//################# containers/user_detail.js ################

//React imports
import React, { Component } from 'react';

//our connect function
import { connect } from 'react-redux';

class UserDetail extends Component {
  // we can add a conditional statement to make sure that if a user has not been selected,
  // we don't receive an error message or have issues rendering this component.
  render() {
    if(!this.props.user) {
      return (
        <div>Select a user to see their details!</div>
      )
    }
    return (
      <div>
        <h3>Details for:</h3>
        <div>{this.props.user.name}</div>
        <div>{this.props.user.bio}</div>
      </div>
    );
  }
}

function mapStateToProps(state) {
  return {
    user: state.activeUser,
  };
}

export default connect(mapStateToProps)(UserDetail);
```

## References  

* [Redux](http://redux.js.org/)

---

Lesson: Redux: Part 2
# Authoring a Reducer  

* A reducer works with actions to update application state.

* A reducer is a function that takes in state and an action and returns a new state according to the action.

## Terminology  

* *Reducer*: a function that takes the current state and an action and returns a new state.

## Examples  

### Action creators and components for reference:  

```jsx
// ### actions.js ###
const USER_SELECTED = 'USER_SELECTED';

export function selectUser(user) {
  return {
    type: USER_SELECTED,
    payload: user
  };
};


// ### containers/user_list.js ###
import React, { Component } from 'react';
import { connect } from 'react-redux';
import { selectUser }  from '../actions';

class UserList extends Component {
  render() {
    let userNames = this.props.users.map((user) => {
      return (
        <li
          key={user.name}
          onClick={() => this.props.selectUser(user)}>{user.name}</li>
      );
    });
    return (
      <ul>
        {userNames}
      </ul>
    );
  }
}

function mapStateToProps(state) {
  return {
    users: state.users,
  };
}

function mapDispatchToProps(dispatch) {
    return {
      selectUser: user => {
        dispatch(selectUser(user))
      }
    }
}

export default connect(mapStateToProps, mapDispatchToProps)(UserList);


// ### containers/user_detail.js ###
import React, { Component } from 'react';
import { connect } from 'react-redux';

class UserDetail extends Component {
  render() {
    if (!this.props.user) {
      return (
        <div>Select a user to see their details!</div>
      )
    }
    return (
      <div>
        <h3>Details for:</h3>
        <div>{this.props.user.name}</div>
        <div>{this.props.user.bio}</div>
      </div>
    );
  }
}

function mapStateToProps(state) {
  return {user: state.activeUser};
}

export default connect(mapStateToProps)(UserDetail);
```

### Reducer `(reducer.js)`:  

```jsx
// ### reducer.js ###
const reducer = function (state, action) {
  return state;
}

export default reducer;
```

### Setting initial state  

```jsx
const initialState = {
  activeUser: null,
  users: [
    {
      name: "Brody",
      bio: "Give me an audiobook and the open road"
    },
    {
      name: "Sawyer",
      bio: "I spend all my free time in nature"
    },
    {
      name: "Kerry",
      bio: "Coding by the pool is the life for me"
    }
  ]
}

const reducer = function (state = initialState, action) {
  return state;
}

export default reducer;
```

### Example reducer  

```jsx
const reducer = function (state = initialState, action) {
  if (action.type === USER_SELECTED) {
    // ... return a new state
  } else {
    return state;
  }
}
```

### `Object.assign` in action  

```jsx
const reducer = function (state = initialState, action) {
  if (action.type === USER_SELECTED) {
    return Object.assign({}, state, {activeUser: action.payload})
  }

  return state;
}
```

### Adding `ADD_USER ` 

```jsx
// ### actions.js ###
const USER_SELECTED = 'USER_SELECTED';
const ADD_USER  = 'ADD_USER';

export function selectUser(user) {
  return {
    type: USER_SELECTED,
    payload: user
  };
};

export function addUser(user) {
  return {
    type: ADD_USER,
    payload: user
  };
};
```

### Adding the above to our reducer  

```jsx
const reducer = function (state = initialState, action) {
  if (action.type === USER_SELECTED) {
    return Object.assign({}, state, {activeUser: action.payload})
  } else if (action.type === ADD_USER) {
    return Object.assign({}, state, {
      users: state.users.concat([action.payload])
    })
  }

  return state;
}
```

---

Lesson: Redux: Part 2
# Using State to Model Events in Redux  

* Planning out your application will help you create the state needed to model your events in Redux.

* It is critical to think what information is necessary for each action.

* Your reducer should be pure and not alter the state but create a new copy that returns the updated state.

## Examples  

### Action creators  

```jsx
export const addPlayer = function (name) {
  return {
    type: ADD_PLAYER,
    payload: name
  }
}

export const deletePlayer = function (index) {
  return {
    type: DELETE_PLAYER,
    payload: index
  }
}

export const updateScore = function (index, amount) {
  return {
    type: UPDATE_SCORE,
    payload: {
      index: index,
      amount: amount
    }
  }
}
```

### Reducer  

```jsx
// Import your action types
import {
  ADD_PLAYER,
  DELETE_PLAYER,
  UPDATE_SCORE
} from './actions';

import update from 'immutability-helper';

const initialState = {
  players: []
};

const reducer = function (state=initialState, action) {
  switch (action.type) {
    case ADD_PLAYER:
      return update(state, {
        players: {$push: [{name: action.payload, score: 0}]}
      });
    case DELETE_PLAYER:
      return update(state, {
        players: {$splice: [[index, 1]]}
      })
    case UPDATE_SCORE:
      return update(state, {
        players: {[action.payload.index]: {
          score: {$apply: function (x) {
            return x + action.payload.amount;
          }}
        }}
      })
    default:
      return state;
  }
}

export default reducer;
```

```jsx
case UPDATE_SCORE:
  return update(state, {
    players: {[action.payload.index]: {
      score: {$apply: function (x) {
        return x + action.payload.amount;
      }}
    }}
  })
```
  
### Evaluating a variable  

```jsx
const name = 'sage';

{name: 1}
// => { name: 1 }

{[name]: 1}
// => { sage: 1 }
```

## References  

* [Redux Docs](http://redux.js.org/docs/basics/)

---

Lesson: Redux: Part 2
# Authoring a reducer to filter data based on a user action  

* We use our action creators to pass our action to our reducer via dispatch inside of the store.

* The reducer is a function that manipulates our state according to actions.

* We can use these to filter our data based off our application state.

* The workflow remains the same for all Redux/React applications and follows the cycle we have discussed.

---

[Lesson: Redux: Part 3]
# Handling Asynchronous Actions in Redux  

In order to make asynchronous actions in Redux, we'll be using Redux-Thunk.

redux-thunk is based on a simple concept: instead of just dispatching actions -- simple JavaScript objects with a type and payload -- you can dispatch functions that take `dispatch` as an argument.

## Setting up redux-thunk  

```jsx
// index.js
import {createStore, applyMiddleware} from 'redux';
import reduxThunk from 'redux-thunk';
import reducer from './reducer';

const store = createStore(
    reducer,
    applyMiddleware(reduxThunk)
);
```

## Creating an asynchronous action  

```jsx
// actions.js
export const SET_LAT_LON = 'SET_LAT_LON';

const setLatLon = (payload) => {
  return {
    type: SET_LAT_LON,
    payload: payload
  }
}
```

We will call this action creator after the Ajax request returns.

To make the Ajax request, we create another action creator that returns a function:

```jsx
// actions.js
const encode = encodeURIComponent;

export const geocodeAddress = (address) => {
  return (dispatch, getState) => {
    fetch(`http://maps.googleapis.com/maps/api/geocode/json?address=${encode(address)}`)
      .then(response => {
        return response.json();
      })
      .then(json => {
        const loc = json.results[0].geometry.location;
        dispatch(setLatLon({
          lat: loc.lat,
          lon: loc.lng
        }));
      })
  }
}
```

The returned function takes two arguments, `dispatch` and `getState`.

---

[Lesson: Redux: Part 3]()

# Authentication in Redux  

To demonstrate authentication with Redux, we are going to use an API built for this lesson.

* [API Documentation](https://github.com/twhitacre/simple-rails-auth/blob/master/README.md)

* [Demo Application](https://github.com/tiycnd/redux-auth-demo)

## Storing a token  

Once a user is registered, you can make a post request to the `/login` endpoint which returns an `auth_token`. You'll then need to store that in a cookie.

## Persisting a login  

The problem with the above code is that it requires the user to log in on every page load. That is suboptimal! If we save our auth token in a cookie, we can load it when we load the page and maintain our logged in status.

To make working with cookies easier, we will install a small library:

```sh
npm install --save js-cookie
```

We will need to set the cookie in our `login` action.

When our app loads we can grab that cookie (if available) and send the `auth_token` along with a header titled `X-AUTH-TOKEN` to our server. Then if we access the `/dashboard` endpoint, it will return the message information related to the user who is logged in.

---

[Unit Tests for React](UnitTestsForReact.md)

# Testing React applications using Jest  

To test React applications, we will use Jest, as it was created by Facebook, the creators of React, to test all JavaScript code, including JSX.

## An overview of Jest  

Jest uses three functions, `describe`, `test`, and `expect`, to define tests.

`describe` is used to set a context for tests. It can be nested, with multiple `describe` calls inside a parent. `test` is used to write individual test cases. You can use `test` outside of a describe block with no problem.

`expect` sets up matchers, which allow you test values. Read an [introduction to matchers](https://facebook.github.io/jest/docs/en/using-matchers.html#content), and then see a list of all matchers](https://facebook.github.io/jest/docs/en/expect.html#content).

## Unit testing  

Testing JavaScript code that does not touch the DOM or is not a React component is as easy as you'd expect. You can import modules you need and write tests for them.

## Snapshot testing  

[Snapshot testing](https://facebook.github.io/jest/docs/snapshot-testing.html#content) is a type of testing specifically for UI where your UI is rendered and compared to a reference to make sure it did not change. Jest gives us the ability to write snapshot tests for React components.

## Testing Redux  

Many parts of Redux are easy to test with unit tests, specifically action creators and reducers. Testing stores and asynchronous code is harder, but [the Redux documentation covers how to test all parts of Redux with Jest](http://redux.js.org/docs/recipes/WritingTests.html).
