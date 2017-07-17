# What is a Build System and Why do We Use Them?  

What is a build system? Why do we need them for front-end applications? In fact, you will be using a build system very 
soon, `create-react-app`. Let's take a look at what a build system is and why we use it in front-end development.

## What is a Build System?  

To understand a build system, we first must know workflow. Workflow is the set of tasks that someone goes through when 
doing a job. Every job has a different workflow and most jobs have more than one workflow. Web development is no different. 
Tasks required to create a web application have a series of steps that must be completed get a final production-ready product.

A build system is a set of tools designed to make the workflow of web development easier. Build systems contain tools that 
can be thought of in two categories: Installers and Task Runners.

### Installers  

First, let's look at *installers*. Installers do what you might expect and install things. You've been using installers 
for some time now. *npm* (Node Package Manager) is a tool we employ in our bash terminal commands to install front-end 
libraries and dependencies - such as React.js, React Router, or Sass. npm can also help us install servers to run our 
development environment on (`npm start`) so we can see our project as we work on them. npm can even help us install other 
installation tools. Other installers include Bower, Yeoman, and Brew.

### Task Runners  

The other half of a build system we can refer to as task runners. These additions perform tasks for us and use their own 
special packages and plugins to complete the task. Tools such as Babel, webpack, Require.js, Browserify, Gulp, etc. are 
all considered task runners. Babel, for instance, is a translator that can take JSX and translate it to regular JavaScript,
or ES2015 syntax into ES5.

## Development vs. Production  

The end game for a build system is to get us ready for production, that is to send our code out into the wild world of 
the web and up and running on a server. Often times during our development of an application we can have tons of files 
and folders - this chaos would ultimately lead to slower loading and poor functionality of an application. Build systems 
are often responsible for minimizing file sizes and compacting folders into a much more streamlined and efficient version 
of our project, which enables our application to run smoothly on a server.

## Conclusion  

* Build systems make our workflow easier and automate tedious tasks for us.

* Build systems contain two different types of tools:

  * Installers: like npm or Bower help us install libraries and dependencies like React.js, or Sass.
  
  * Task Runners: like Babel or webpack, help automate tedious tasks in our project.
  
* A build system should contain the right tools for your project, the right amount of installers and task runners to get the work done.

### References  

* [radify.io: Using Build Tools](http://radify.io/blog/using-build-tools/)

---

# Benefits of ES2105  

Modules were introduced as a way to fix the problems of global variables, which could cause problems for large programs, 
or programs with multiple authors. Prior to ES2015 syntax, modules were available in ES5 by coupling requireJS and using 
`require` statements at the top of a page where you wanted to bring them in. Exporting was done in a similar fashion at the 
bottom a file using `module.export` statements.

ES2015 simplifies the whole process and allows for streamlined importing and exporting of code between pages.

## Export to Import  

Let's say that we have a file, `utility.js`, that contains a few different utility functions that we plan to use in 
multiple places. By using the `export` command we can export one or multiple files. To bring them into another file we 
simply use the `import` method as seen below.

```javascript
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

We can import one or multiple pieces of data this way as well. Let's take a look at how we would do that:

```javascript
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

When importing multiple modules from a single file, we simply add `{ }` around the exported modules we are importing 
(like `makeMath` and `makeFood`).

If we have a lot of modules to export and import from a single file, that might be a bit time consuming. We handle that 
situation by using the `*` operator. Let's take a look at that in action using our previous example:

```javascript
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

Above, you can see that we used the `*` operator and coupled it with a new keyword `as` to allow us access to all files 
being exported `from` our './utility.js' file.

It's essentially saying "I want to take everything from this file as an object `utility` to access later on". The new 
`utility` object exists in the scope of `index.js`.

Now, any of the functions, variables, etc that we plan to use and import from that utility.js file can be accessed on 
the `utility` object. For example:` utility.makeFood("ham","swiss")`.

You can also still export everything from the end of a file like we did in ES5. Instead of using `export ...` for each 
function, variable, etc. You could place an export statement at the bottom of the file like so:

`export { makeMath, makeFood }` This would achieve the same results.

## What Are the Benefits of This?  

Modules allow us to eliminate issues with global variables, organize our code in to reusable pieces, and keep our code tidy.

## Conclusion  

* Using `export` allows us to share pieces of code or entire files to other files within our program.

* To retrieve those files we simply call `import` in the file where we want to use them.

* We can import multiple pieces of code by using `import { myFunction, myVariable, myArray} from './myFile.js'` or -

* We can import them with a generalized method by using `import * as myThings from './myFile.js'`

* We would then reference those files by means of `myThings.myFunction` or `myThings.myVariable`.

* Though it is good practice to keep all import statements together at the top, imports are hoisted, meaning it doesn't really matter where you call them within a module.

### References  

[ExploringJs](http://exploringjs.com/es6/ch_modules.html)
