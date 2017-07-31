# Application State  

We know that each component has access to state and we can manage the state of the application by passing props to children components. We also know that managing state for all of the different pieces of data in our application is tedious and must be carefully done to maintain state consistently.

There are a few libraries for maintaining state accross components in an application, *Flux* is one of the original ones. It was created by Facebook to compliment React.js. We won't be covering Flux but it is important to understand that *Redux* was evolved from Flux and is substantially less complicated. To understand Redux, we must first understand *application state*.

## Application State  

Application state does not refer to the state of each component but rather the data that flows through the entire application. The real magic of Redux is how it handles data and manages what actions should be taken when events are triggered.

### Is A State Management Library Right For My Project?  

By managing all of the application data for our app in one place, we don't have to figure out which component is responsible for which data. We simply associate the application state into each component as we build our application. Redux ensures only the relevant components receive data and state changes while the rest of the application remains unaffected.

Here a few questions we can ask to determine whether an application state library would benefit your project:

1. Do other parts of the application care about the data in question?

2. Do we need to create more data based on the data in question?

3. Is the data in question being used to create multiple components?

4. Is there value in having a record of the changes to state in application? (i.e. Would it help you debug your application if you could track events as you test?).

5. Do you want to cache (hold on to) your data instead of using multiple requests from another API?

If you answer yes to any of these questions, there's the potential for a library like Redux to lower the complexity in your components.

## Conclusion  

* As our projects get more complex and handle more data there are tools we can use to manage our application state.

* Flux was designed to manage application state.

* Redux evolved from Flux and can help us manage application state in a simple and straight forward way.

### References  

* [Redux JS: Organizing State](http://redux.js.org/docs/faq/OrganizingState.html)

* [Flux](https://facebook.github.io/flux/)

* [Redux ReadME](http://redux.js.org/)

---

# React and Redux Workflow  

We've discussed application state and what that means for our program and its data. We'll now dive into Redux and React and how they work together.

Let's talk about the following before diving in on Redux:

1. Setting up our application file/folder structure in a conventional way for Redux.

2. Downloading and installing the necessary dependencies to use Redux along with React.

3. The pieces of the Redux puzzle and how they work with the React workflow.

## Folder Structure  

By now we should be comfortable with having separate folders for scripts and styles, as well as a folder for our React components. We'll now need to add a few more folders to our app. In addition to our components folder we need to add the following:

* containers

* actions

* reducers

Our file tree should now look something like this:

![filetree](https://github.com/rickmurdock/notes/blob/master/ReactJS/images/filetree.png)

### Adding Files  

Inside of our `actions` and `reducers` folders we'll need to add an `index.js` file to each folder. We'll touch back on these files a bit later.

## Installing Dependencies  

In a terminal, go to the project directory and use npm to install the following dependencies:

```sh
npm install --save redux
npm install --save react-redux
```

The first command will install Redux. The second command installs React-Redux — a package that allows React and Redux to work together.

### Further Installations  

Some addition installs will come up as we journey through the Redux workflow, but for now, this is enough to get started.

## React and Redux Workflow  

A lot is going on within a React application, from application state to components and component state. For now, we'll focus on the application state.

Imagine a simple app where a user clicks an option which displays data on another screen. Let's look at the flow of events that take place when this happens:

![react redux workflow](https://github.com/rickmurdock/notes/blob/master/ReactJS/images/react-redux-workflow1.jpg)

Upon clicking an *Action Creator* is triggered.

### Action Creator  

**Action Creators** are functions that return an **Action**. They give us the specific "action" for our particular event.

### Actions  

**Actions** are payloads which send data from your application to the `dispatch` function.

### Dispatch  

The `dispatch` function directs your action payload to the *reducer*. The function takes action and passes it the *Store* and on down to the correct reducer.

### Store  

The **Store** holds the whole state tree of your application. The only way to change the state inside it is to dispatch an action on it.

### Reducers  

There is one reducer in a Redux application. You can split the reducer into multiple functions that work like the root reducer, but you have to use the `combineReducers` function to assemble them into your root reducer. The important part to know is that they receive the action payload. When they receive the action payload, they pair it up with a proper response.

### In Summary  

Actions describe the fact that something happened, but they don't specify how things should change in response. Reducers take that action and respond with the proper change, which manages application state and how it should change. Reducers are pure functions that pair an action and the application state and return a new state based on what the action dictated.

### Complications of State in Redux  

An important thing of note is that application state is never mutated. After an action meets the previous state in the reducer, a brand new object representing the next state is returned. We can't just alter the existing state (which we'll discuss in upcoming examples).

Reducer functions should also never perform API calls or routing transitions, nor should they use non-pure functions like `Date.now` or `Math.random` — as these would mutate state.

> "Given the same arguments, it should calculate the next state and return it. No surprises. No side effects. No API calls. No mutations. Just a calculation." - Redux docs.

### Application State  

The application state is updated as a new copy of state with the changes performed by the reducer. Inside of our components, we can utilize this state to help re-render and update our application with the newly changed data. This application state is a bundle of all of our state, which means that we have access to it throughout our entire application. However, we generally reserve the term *container* for a React component that will bridge between Redux and React. These containers contain the application state and determine how it should flow throughout React.

## Conclusion  

* The React and Redux workflow cycle is fairly complicated but centers around updating application state.

* A data change or event triggers an Action creator.

* The Action Creator creates an Action.

* An Action is the payload of information and data regarding what happened.

* A dispatcher handles the Action and directs it to the proper Reducer function inside of our Store.

* The reducer function is a pure function that pairs an action with an appropriate piece of application state and updates it accordingly.

* Application state is never mutated, but rather copied and returned as a new object.

* The new application state is sent back to React and utilized by our Container components.

* Container components are the bridge between React and Redux and determine what should be done inside of React with the newly created application state.

### References  

* [Redux Basics](http://redux.js.org/docs/basics/)

* [Redux diagrams GitHub](https://github.com/reactjs/redux/issues/653)

---

# Authoring Actions  

We just wrapped up our discussion on the workflow of React and Redux working hand in hand. So let's start to dissect some of the ideas we discovered and take them on piece by piece from the top down. First, we know what actions are: they are the payload of information that send data from your application to the store (through the dispatcher and to the reducers).

This knowledge is fantastic to have, but we have no idea how to implement it yet - so that is our very next step. We are going to walk through setting up some actions for a simple React/Redux application.

## In the actions folder...  

Inside of our actions folder we need to create an index.js file. This will be our root action folder and house all of our actions and action creators. Remember the action creators are just functions that return an object (which is the action).

Lets pretend for a moment we have an application that allows us to select a user and see details of that user. In this simple example our index.js (inside of our actions folder) would be set up something along the lines of :

```jsx
//########### actions/index.js ################
const USER_SELECTED = 'USER_SELECTED';

export function selectUser(user) {
  return {
    type: USER_SELECTED,
    payload: user
  };
};
```

A couple of things are going on here. First we are declaring a constant variable `const USER_SELECTED = 'USER_SELECTED';` (which by convention is all capitol letters) and we are setting it to a string value that is the same name as the variable. This variable will be our **type** (we will discuss that momentarily). The important thing to take away from this is that this is a means to protect our action. *This should prevent someone from accidentally tampering with our action type and messing up a whole lot of things down the line.* As our applications grow, it is common to add another folder "actionTypes" to our application file tree and create a file inside to house all of our action types there. Since our application is small - we will forgo that for now.

The next step is exporting our action creator (a function that houses and returns the action) `export function selectUser(user){};`. Each action we create will be exported individually. We can have multiple export statements in this file and each action should have its own action creator.

The argument we pass into our action creator function is `user` in this case. We want to select a user and therefore thats the data the function will expect. We then return the action object.

### The action object { }  

The action object has only one critical *require* piece:

1. A type

There are also a few *optional* pieces that may be added to the action object should the complexity require it:

1. A payload (very common to have)

2. An error key

3. A meta key that houses data the payload doesn't have.

The **type** key is simply a representation of what type of action is being performed. It helps us keep track of our actions. It is a string value, generally represented by a variable (for protection) that describes the type of action expected: `"SELECT_USER"` for instance.

In our example, we have data that needs to be passed along with the action type. So we have a "payload" key as well. The payload is our `user` that is passed into our action creator. This ensures that we will have access to the user when we reference our action creator inside of our React container component.

It is not uncommon to see payload without the payload key: you could expect to encounter something like this which would have an identical effect.

```jsx
//########### actions/index.js ################
const USER_SELECTED = 'USER_SELECTED';

export function selectUser(user) {
  return {
    type: USER_SELECTED,
    user
  };
};
```

This code is functionally the same as what we saw earlier and simply has the payload as an ES6 syntax where `user: user` can be whittled down to just `user`.

This is really all there is to authoring an action in Redux. Below, we can take a look at some other examples of action creators and actions.

### Some more examples...  

Let's imagine an application in which we wish to keep score of a user and be able to add or delete a user. For this instance we might first have an actionTypes folder to separate our action type variables from our action creator functions. We can also look at incorporating more ES6 syntax into this example as well.

```jsx
//######### actionTypes/actions.js #############

export const ADD_USER = 'ADD_USER';
export const DELETE_USER = 'DELETE_USER';
export const UPDATE_SCORE = 'UPDATE_SCORE';
```

Now to create our action creators!

```jsx
//################ actions/index.js ###############

import * as ActionTypes from '../actionTypes/actions';

export const addUser = userName => {
  return {
    type: ActionTypes.ADD_USER,
    userName
  };
};

export const deleteUser = userId => {
  type: ActionTypes.DELETE_USER,
  userId
};

export const UPDATE_SCORE = score => {
  type: ActionTypes.UPDATE_SCORE,
  score
};
```

And thus we have authored action creators with actions. This is just the first step in the cycle of information in Redux land. We need to take a look at how these action creators and actions are implemented in our React components and also how we pass them through the dispatcher to the store to the reducers.

Let's go back to our User List example, where we wanted to select a user and display their details after selecting and see how we link it up to our container components.

**(Remember: The container component is the component that we designate to handle application state for React - in other words the bridge between Redux and React.)**

### The Containers  

Given this simplistic app: we should envision that we have two components that will rely on application state. We should readily guess that our User detail page would need application state to display the user's details. But also, we should start thinking about the list of users that will need to pass the individual user to another component.

Inside of our `containers` directory we can create two files.

1. user_list.js

2. user_detail.js

First look at user_list.js and see how we would set that container component up with some commented notes - and discuss it thoroughly after a peek at it.

```jsx
//################ containers/user_list.js ################

//normal react imports
import React, { Component } from 'react';

//the glue that binds React to Redux so they can communicate
import { connect } from 'react-redux';

//our action creator to select a user
import { selectUser }  from '../actions/index';

//a function that will make sure our action creator is passed to all the reducers to make sure it goes through the right one
import { bindActionCreators } from 'redux';


//for now, just understand that props are coming from the reducer... we will examine that more thoroughly in a coming lesson
//the mapStateToProps function below does this for us.
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
  //what is returned will show up as PROPS inside of the UserList container
  //inside of this function we generally return an object
  return {
    users: state.users,
  };
}

function mapDispatchToProps(dispatch) {
  //whenever selectUser is called, result should be passed to
  //all of the reducers. (flows through dispatch function -- like a funnel - finding the right reducer for the job)
  //in our return we are binding our action creators to the dispatch function that works behind the scenes for us.
    return bindActionCreators({ selectUser: selectUser }, dispatch)
}

// promotes UserList from component to container and needs to know about dispatch method selectUser (make available as prop)
//this "connects" our functions to our container component and really enables the magic to happen.
export default connect(mapStateToProps, mapDispatchToProps)(UserList);
```

### A deeper look  

We create a class based component to be our designated container component (the link for React and Redux). Inside of that class based component (`UserList`) it should absolutely look like nothing new is happening. We pass in props and map over them to create a list of user. Something we have done countless times by now. We create an `onClick` event for list item that is passed in a function (this function { `selectUser` } is our **action creator**).

We then display the results as normal and voila we have created a list of users. The real magic and connection happens in the functions below our container component.

The first function `mapStateToProps` receives and argument of `state`. It simply returns the state that we wish for our `UserList` container to receive as props.

The second function `mapDispatchToProps` receives an argument of `dispatch`. We wont go in to detail of how dispatch works behind the scene, but remember it acts a lot like a 911 call center and matches our action with the proper reducer to create new state.

Inside of our mapDispatchToProps function we are going to return our action creator function and bind that action using a function `bindActionCreators` to our `dispatch`.

```jsx
function mapDispatchToProps(dispatch) {
  //whenever selectUser is called, result should be passed to
  //all of the reducers. (flows through dispatch function -- like a funnel - finding the right reducer for the job)
  //in our return we are binding our action creators to the dispatch function that works behind the scenes for us.
    return bindActionCreators({ selectUser: selectUser }, dispatch)
}
```

The last piece of the puzzle is connecting everything with the proper wiring. We imported a `connect` function from redux in our imports and now we are going to use it to rig the wiring. We will export this connect function instead of our actual component and will pass it the `mapStateToProps` and `mapDispatchToProps` in the first set of arguments and then our container component after in a separated argument `UserList`

```jsx
// promotes UserList from component to container and needs to know about dispatch method selectUser (make available as prop)
//this "connects" our functions to our container component and really enables the magic to happen.
export default connect(mapStateToProps, mapDispatchToProps)(UserList);
```

We are all set up in our UserList to use our action creators. Now a quick glance at our user_detail.js file which will do much of the same thing. We won't go into more detail with this since it behaves exactly as the other container did - except it is focused a detail of the user rather than an entire list.

**Also, since this container component is not concerned with selecting a user, but rather displaying one, we dont actually need a dispatchStateToProps function to line up our reducer with our action. That happens in our UserDetail container component.**

```jsx
//################# container/user_detail.js ################


//React imports
import React, { Component } from 'react';

//our connect function
import { connect } from 'react-redux';

//import our action creator
import { selectUser }  from '../actions/index';

//make sure action created flows through all reducers
import { bindActionCreators } from 'redux';


class UserDetail extends Component {
  //we can add a conditional statement to make sure that if a user has not been selected,
  //we don't receive an error message or have issues rendering this component.
  render() {
    if(!this.props.user) {
      return (
        <div>Select a user to see their details!</div>
      )
    }
    return (
      <div>
        <h3> Details for: </h3>
        <div>{this.props.user.name}</div>
        <div>{this.props.user.bio} pages</div>
      </div>
    );
  }
}

//activeUser reducer (we will examine in next lesson) creates activeUser state.
//this gives us the details for a single user

//Also, since this container component is not concerned with dispatching our action creator, we don't need to
//mapDispatchToProps.

function mapStateToProps(state) {
  return {
    user: state.activeUser,
  };
}


export default connect(mapStateToProps)(BookDetail);
```

Now we are all rigged up to check out the reducers in the next section! We have authored some action creators and actions and are ready to put them to use!

## Conclusion  

* Action creators are functions that deliver the action to the reducer via dispatch in the store.

* The each action needs/is required to have a `type:` property which is simply a string description of the event associated with the action.

* An action can also have some other key properties, most commonly `payload:` in which data can be passed along to the reducer as well.

* We use `mapStateToProps` to return our state as props inside of our container component.

* We use `mapDispatchToProps` to bind our action creators to be passed to the reducers with the function `bindActionCreators` that receives the action creator and the dispatch object.

* We use `connect` to link up our component to our map functions and export it for use in the program.

### References  

* [Redux](http://redux.js.org/)
