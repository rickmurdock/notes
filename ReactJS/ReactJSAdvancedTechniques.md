# Fetching JSON  

We've started to see how a React application can manage our data and render our application accordingly. We haven't looked at pulling data in from an outside source using `fetch` and what we can do with that data after we retrieve it.

Let's examine the flow of a simple application designed to list some of the characters from the **Star Wars** movie series. We'll use `fetch` to get the data from the Star Wars API (https://swapi.co/). Next, we'll look at how we store that data and apply it our app.

## Fetching the Data  

By now you should be comfortable with using `fetch` and promises such as `then` to retrieve data. Let's set up our first component.

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

* The top portion of this should look familiar by now. Inside of our `constructor`, we are setting the `state` and giving it a property `characters` that is initially set to an empty array.

* The next method we see is the `componentDidMount` method. This method is the preferred method to use when retrieving data with a network request.

* `componentDidMount` is invoked immediately after the component is mounted on the DOM, so it assures us our data will have a place to live.

* We then begin our network request using the `fetch()` method. We pass `fetch` the URI of our API (the specific point at which we wish to retrieve our data)––in this case `fetch('http://swapi.co/api/people/')`.

* Then we initiate and chain together our methods to return a promise. We can use the `then` method to chain together our promises and complete our request only after the entire cycle of our request is complete.

### A Closer Look  

```jsx
fetch('http://swapi.co/api/people/')
.then(results => results.json())
.then(responseData => {
  this.setState({characters: responseData.results});
})
.catch((error) => {
  console.log("Error with Fetching : ", error);
});
```

* After we call `fetch`, we call our first `then` method: `then(results => results.json())` .

* This first method says that after the request has gone through successfully then run a callback function (in which we pass in the `results` parameter and return those results as a JSON object using the `json` method.

* We then call another `then` method: `then(responseData => {this.setState({characters: responseData.results});})` .

* This second `then` method makes sure that we received our promise and data from the first `then`.

* When that promise is achieved, we take the `responseData` and we set the `state` of our `characters` array to the `responseData.results`.

* We use `results` because we researched our API and saw the object returned from the API had a "results" property in which our data was stored.

* We use the `this.setState({characters: responseData.results})` to achieve this.

* This last method, `catch` is optional. `catch` takes a function that is passed the parameter of error. It listens for any error messages from the server and allows us to do something with them should they occur.

* In this case we made a simple `console.log` statement to write the error to the console.

## What We Do With Our Data...  

In this case, we have set our `state` with the data we pulled in. Now we can use that data to render part of this component or a child component. To keep our learning sharp, we are going to pass our data to our child component.

We do this when we give the child component an attribute of `people`.

* `<CharacterList people={this.state.characters}/>` inside of our `<App />`'s `render` method.

* We pass the state of our `characters` array down to the child component `<CharacterList />`.

### Now Let's Look at the Child Component  

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

This should all look familiar, but let's recap what's going on:

* We set up our `component` with a constructor method with `super` inside of the method. This allows us to receive `props`.

* Inside of our `render` method we declare a variable `peeps` using the the ES2015 `let` syntax.

* Our `peeps` variable is a mapping of `this.props.people` which we passed down from our parent components state.

* When we `map` over each item in the array we grab each `person` from that array.

* We create a `return` statement in the `map` function that returns a `<li>` with a `key` value equal to the `index` so every list item has a unique identifier and React can render and track of them in the virtual DOM.

* We then extract the properties we want to list (like `person.name` or `person.eye_color`) by calling each attribute from our `person` object.

* We can get these values by examining our `console.log` statement inside of the `render` method that we used for reference in our parent component...

```jsx
render() {
    console.log("characters", this.state.characters);
    return (
      <div className="main">
        <CharacterList people={this.state.characters}/>
      </div>
    );
  }
```
  
* That console.log() statement returns something we can drill into and examine the data with...

![Star Wars data](https://github.com/rickmurdock/notes/blob/master/ReactJS/images/starWarsData.png)

Now when we run this we get:

![Star Wars characters](https://github.com/rickmurdock/notes/blob/master/ReactJS/images/starWarsCharacters.png)

## Conclusion  

* We can use the `fetch` method in React in order to make a network request.

* We use `then` to manage our promises and make sure our network request is completed.

* We can use `catch` to listen for error messages from the network and utilize them for functionality in our program, or simply for reference.

* We can store our data in `state`, using `setState` inside of our promises.

* `componentDidMount` is the ideal method in which to make network requests.

* `state` can be passed down as `props` to children components.

### References  

* [MDN Then Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)

* [MDN Body.json()](https://developer.mozilla.org/en-US/docs/Web/API/Body/json)

* [MDN Using Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)

* [StarWars API : SWAPI](https://swapi.co/)

* [React Component Lifecycles](https://facebook.github.io/react/docs/react-component.html#componentwillreceiveprops)

---

# Styling Components with React  

We've already covered the building of components and data flow in `state` and `props`, but our websites have been pretty underwhelming so far. We haven't concentrated much on the styling aspects available to us when building React applications. The ability to apply a style based on a class or using inline styles in HTML has long been a tool for software engineers. React allows us to do the same sort of styling inside of our components within our JSX elements. The functionality is not much different, though some of the properties and declarations need to be a bit different to function in our JSX.

React's foundation centers around modularizing (creating stand alone pieces of code) our code to keep everything self-contained. React JSX components allow us to write our Javascript and HTML in a single file. React takes the same approach when it comes to styling. Let's take a look at how React suggests we style our components.

## Using `classNames`  

In HTML, as in JSX, we have the ability to designate a class (HTML) or *classNames* (JSX). Let's see the HTML approach first and then we'll examine the same situation within React.

Below is some boilerplate HTML with a few classes:

```html
<body>
  <nav>
    <header class="header">Header</header>
  </nav>
  <div class="box">
    <h3 class="title">An Interesting Title</h3>
  </div>
  <div class="box">
    <h3 class="title">An Interesting Title</h3>
  </div>
  <div class="box">
    <h3 class="title">An Interesting Title</h3>
  </div>
</body>
```

Given this HTML we could easily style our elements using CSS. In our CSS stylesheet we'd write something similar to the following:

```css
.header {
  color: red;
}
.box {
  background-color: blue;
}
.title {
  color: white;
}
```

We created classes to target within our stylesheet and altered them according to our design. Now, let's visit how we would approach the same problem with React:

```jsx
class App extends Component {
  constructor(){
    super();
  }
  render(){
    return (
      <body>
        <nav>
          <header className="header">Header</header>
        </nav>
        <div className="box">
          <h3 className="title">An Interesting Title</h3>
        </div>
        <div className="box">
          <h3 className="title">An Interesting Title</h3>
        </div>
        <div className="box">
          <h3 className="title">An Interesting Title</h3>
        </div>
      </body>
    )
  }
}
```

With React, our CSS stylesheet would be exactly the same as before. We'll target the same classes as the previous example to apply our styling.

```css
.header {
  color: red;
}
.box {
  background-color: blue;
}
.title {
  color: white;
}
```

Now, let's include our styles in our JSX component instead of an external stylesheet.

## Inline Styling  

Similar to using inline styles in HTML, we can use inline styles in JSX components as well. There are just a few new rules and concepts to abide by:

* Multi-word CSS properties (commonly separated by a dash) such as `text-align`, `font-family`, or `background-color` must now adhere to JavaScript standards. They should be written in *camelCase* (all one word, with the first word lowercased and the second word capitalized). Previous examples would become: `textAlign`, `fontFamily`, and `backgroundColor`.

* Single worded CSS properties such as `border`, `color`, and `width`, will remain **unchanged** and used as they would be in CSS.

**Let's take a look at a couple of ways we can apply these inline styles in React.**

### Using Variables  

Inside of our `render` method before our `return` statement we can assign our styling to a variable and pass that variable to our elements. We can then add a `style` attribute to the element or component in question and pass our variable in as the value using `{}` brackets: `style={yourVariableHere}`. This method is a great choice when dealing with a lot of styles or a larger component.

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

`<div className="box">` is styled to have a blue background. What if we wanted to control the color for each instance of this div? We could first create a property `bgroundCol` on each `<div className="box">`. Then apply the value of each `bgroundCol` property as the `backgroundColor` for the `boxStyle` to be applied to that element. We'll use `this.props.bgroundCol` to access the value of each element's `bgroundCol` property.

```jsx
let headerStyle = {
  color: "red",
};
let boxStyle = {
  backgroundColor: this.props.bgroundCol,
};
return (
<nav>
  <header className="header" style={headerStyle}>Header</header>
</nav>
<div className="box" bgroundCol="blue" style={boxStyle}>
  <h3 className="title">An Interesting Title</h3>
</div>
<div className="box" bgroundCol="green" style={boxStyle}>
  <h3 className="title">An Interesting Title</h3>
</div>
<div className="box" bgroundCol="brown" style={boxStyle}>
  <h3 className="title">An Interesting Title</h3>
</div>
  )
}
```

By passing our `backgroundColor:` property the value of `this.props.bgroundCol`, we are able to style directly inline as we see fit.

### Completely Inline  

We can take this one step further by styling everything completely inline. Let's take a look at how this would work by creating a new div and giving it a `style` attribute. We can then pass in our properties as an object, like so:

```jsx
<div className="new" style={{color:"orange", backgroundColor: "purple", fontSize: "33px", textAlign:"right", height: 80}} >Im the best Div ever </div>
```

**Note that double brackets : we are passing our first set of brackets an object containing our styling**

> The majority of the properties that involve a default value of pixels (px) can either exist as a string, `"33px"` or `"33"`, or a straight number such as `80`. The compiler knows to add 'px' on the end when it renders. (There are a few exceptions that you may find).

Hopefully, this provided a comprehensive overview of how to style your React components.

## Conclusion  

* It's common practice in React to create self-contained components.

* Styling can take place by using the `className` attribute and creating a stylesheet to transform our component.

* We can also style directly inline with React using a few approaches.

* Multiword style properties should obey the camelCase (`backgroundColor`) naming convention. Exceptions are the `data-` and `aria-` properties, which remain separated by a dash.

### References  

* [Kirupa.com](https://www.kirupa.com/react/styling_in_react.htm)

* [React Docs](https://facebook.github.io/react/docs/dom-elements.html)

* [React JSX](https://facebook.github.io/react/docs/jsx-in-depth.html)

---

# Stateless Components  

State isn't required in React components. In fact, you can have components that rely only on the props. If you're not changing any data in the component, then there's no need for state - simply use props to pass static data.

## Component Types  

There are three different component types: *stateless*, *stateless Functional*, and *stateful*.

**Stateless Component** — Only props, no state. There's not much going on besides the render() function whose logic revolves around the props they receive. This makes them very easy to follow and test.We sometimes call these **presentational** components.

Stateless components are pure functions of their props. They don't have lifecycle methods and don't retain internal state. Another way to think about **stateless** components is that they deal with the UI rather than behavior. This is why you'll also see them called **presentational** components.

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

**Stateless Functional Component** - If your component only has a render method, you can write it as a stateless functional component, and your function will be passed `props` as its first argument like so:

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

Another thing to note with the **stateless functional component** above is that there's no `this`, and there's no binding required.

**Stateful Component**  — Both props and state. We also call these state managers. They are in charge of client-server communication, processing data, and responding to user events. We sometimes call these **container** components. These sort of logistics should be encapsulated in a moderate number of stateful components, while all visualization and formatting logic should move downstream into as many stateless components as possible.

You've seen (and written by now) stateful components. Imagine if you wanted to let the user interact by inputting their favorite movies. Here's an example to compare to the stateless components we looked at above:

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

### To Have or Not Have State?  

**Should this Component have state?**

**State** is optional. Since state increases complexity and reduces predictability, a component without state is preferable. Even though you can't do without state in an interactive app, you should avoid having many stateful components. Optimally, an app is comprised of mostly stateless components.

Often, apps are organized with higher level "container" components that manage state with multiple stateless components.

## Conclusion  

* Stateless/presentational/presentational components deal with props only and don't have state or lifecycle methods.

* Stateful/container components manage state and props and deal with processing data that is changing over time.

* Best practice in writing clean code is to have mostly stateless components and a few stateful components.

### References  

* [JSPlayground](https://javascriptplayground.com/blog/2017/03/functional-stateless-components-react/)

* [Todd Motto](https://toddmotto.com/stateful-stateless-components)

* [Dan Abramov](https://medium.com/@dan_abramov/react-components-elements-and-instances-90800811f8ca)
