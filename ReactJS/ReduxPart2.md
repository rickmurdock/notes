# Authoring a Reducer  

We have walked through Redux actions and action creators, and shown how to wire state to our container components. The last piece of the Redux puzzle is the *reducer*, a function that takes the current state and an action and returns a new state.

Below, we have our action creators and components for reference.

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

## Creating our reducer  

In this example, we have one action, `SELECT_USER`. Our state's structure has been defined to be an object with two keys:

* `activeUser`: an object with `name` and `bio`

* `users`: an array of objects, each with `name` and `bio`

In most applications, the list of users would come from an external resource, but in our case, it will be hard-coded in our initial state.

Create a new file called `reducer.js` in the same directory as `actions.js`. Let's put the simplest possible reducer in there:

```jsx
// ### reducer.js ###
const reducer = function (state, action) {
  return state;
}

export default reducer;
```

This reducer takes our state and an action and does nothing at all with it, which technically is a reducer.

We have a problem: we expect our state be an object with `activeUser` and `user` keys. When our application starts, Redux will call our reducer with an undefined state. We can use that to set the initial state:

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

The default argument syntax in ES6 -- the version of JavaScript we are using with React -- made this easy.

Let's make our reducer respond to our action. Our one action has a `type` of `USER_SELECTED` and a `payload` of the user object. When writing our reducer, let's assume that we will have more than one action type in the future. Assuming we've imported the action type constants, our reducer could look like this:

```jsx
const reducer = function (state = initialState, action) {
  if (action.type === USER_SELECTED) {
    // ... return a new state
  } else {
    return state;
  }
}
```

Remember that we are creating a new state. We can't use the obvious solution, which would be to write `state.activeUser = action.payload`. We will use `Object.assign`, which copies the properties of one or more objects into another. Here it is in action:

```jsx
const reducer = function (state = initialState, action) {
  if (action.type === USER_SELECTED) {
    return Object.assign({}, state, {activeUser: action.payload})
  }

  return state;
}
```

Note that we pass an empty object as the first argument to `Object.assign`. We want to copy all of `state`'s properties into this object, as well as the new `activeUser` property.

## Making immutability easier  

Let's add another action, `ADD_USER`. This action will take a user object as its payload:

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

Now to add this to our reducer:

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

`Array.prototype.concat` returns a new array, so this works well, but it would be fair to call it unwieldy. You could imagine that this would get even more unwieldy if you wanted to update the bio of an individual user, for example.

There are multiple libraries that can help make cloning an object and updating that clone easier. One that is commonly used is `immutability-helper`. This library uses syntax similar to MongoDB update operators to update state. The above reducer, rewritten to use `immutability-helper`:

```jsx
import update from 'immutability-helper';

const reducer = function (state = initialState, action) {
  if (action.type === USER_SELECTED) {
    return update(state, {activeUser: {$set: action.payload}});
  } else if (action.type === ADD_USER) {
    return update(state, {users: {$push: [action.payload]}});
  }

  return state;
}
```

As a last simplification, you can use a `switch` statement instead of `if/else`. This makes the action types stand out more, but `switch` can be tricky as you have to remember to return or use `break` in each case or "fall through" to the next case. Whether you use `switch` or `if/else` is a matter of personal preference.

```jsx
import update from 'immutability-helper';

const reducer = function (state = initialState, action) {
  switch (action.type) {
    case USER_SELECTED:
      return update(state, {activeUser: {$set: action.payload}});
    case ADD_USER:
      return update(state, {users: {$push: [action.payload]}});
    default:
      return state;
  }
}
```

## Conclusion  

* A reducer works with actions to update application state.

* A reducer is a function that takes in state and an action and returns a new state according to the action.

---

# Using State to Model Events in Redux  

When using Redux, we will have to design a model for our state that matches the events that will occur in our application. Application state is modified by firing an action and using a reducer to create a new updated copy of the state. It will be a single source of truth for our application, and we can apply it to any component that requires it. This is one of the most important concepts behind Redux and helps us manage the complicated idea of state in a singular way. Redux will require the skill to create state objects that model the events of our application.

## Finding Your Events  

As with any application, thinking ahead before you jump into programming is a must. Planning out your application will help you think logically about the pieces required to achieve the goal you are after. With that in mind, let's design a trivial application and find out what events that application will have.

Let's make an application that lets us create and delete players for a game, and keep track of their score.

What are the events we need?

* Create a player

* Delete a player

* Increment a player's score (add to it)

* Decrement a player's score (subtract from it)

## Create the State for Your Events  

With the events or actions, in place, we can then begin to understand what our state object should look like. Without writing any code at first, let's break each action/event down to understand what kind of state it may require.

### Create a Player  

To create a player we will need the following information:

* We will need an id -- a name or number -- representing each player.

* We should probably have an index of the players so that we can refer to a single player.

* We will need a score for the player.

This is starting to feel like an array of objects, right?

### Delete a Player  

Deleting a player will require the following:

* We will need the index of the player we select for deletion.

That should be the extent of the requirements for deleting someone from the score counter.

### Increment and Decrement a Score  

These are similar events, differing solely in the outcome of the score. The state for this event will need the following information:

* The index of the player whose score will be updated.

* The number of points to increment or decrement.

With that, we have laid down the groundwork to make our state.

## Turning Events into Actions  

Since our actions trigger our reducers to create a new copy of our state, we can look at how they might appear first.

First, think of the types of actions. We will want to be able to add a player, delete a player, and update a player's score. We can set up `const` values for these.

```jsx
const ADD_PLAYER = "ADD_PLAYER";
const DELETE_PLAYER = "DELETE_PLAYER";
const UPDATE_SCORE = "UPDATE_SCORE";
```

Next, let's make action creators for these:

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

We have modeled our events and the state they require. Now let's look at the reducer this application would use to see how we can create a new state object based upon the `action` that is fired.

## Reducing  

In our reducer, we will need a `switch` statement to account for each action we have created and determine how we will change our state based on the action type we receive.

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

This reducer will handle our actions and update our state. One piece of syntax to note:

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

When you have a variable that holds an object property name, you can put it in square brackets to evaluate it. As a further illustration:

```jsx
const name = 'sage';

{name: 1}
// => { name: 1 }

{[name]: 1}
// => { sage: 1 }
```

# Conclusion  

* Planning out your application will help you create the state needed to model your events in Redux.

* It is critical to think what information is necessary for each action.

* Your reducer should be pure and not alter the state but create a new copy that returns the updated state.

## References  

* [Redux Docs](http://redux.js.org/docs/basics/)

---

# Authoring a Reducer to filter data based on a User Action  

We've seen the actions, we've seen the reducers, we've seen how they all work together, but let's go a step further and see how they interact together and how we can use a reducer to filter our data based upon a given action. A perfect candidate for this scenario is the timeless classic of a Todo Application! Though we most certainly would not need to actually use Redux to help us manage the limited state of a Todo application, it provides a great platform to simply demonstrate how this process works.

We will assume the usual suspects are to be found in our file tree - we have an app, we've npm installed redux and react-redux, we will have our standard set up of actions, reducers, components, containers directories. We will have used webpack or create-react-app to scaffold our project and have a live server for editing, etc.

So let's first start with creating a list of todos. We should first start with thinking our container and presentational components, also known as our presentational (regular components) components and our container components. We should recall our container components handle the application state for our entire application, whereas our presentational components handle the state of the user interface, but are not concerned with the data.

## Creating a list of Todos...  

We will be using Bootstrap 4 for a few basic styling options, so you might see a few funky class names on our JSX elements, but just understand that those are related to styling.

Our first component we will create will be a container component. It is going to be concerned with the application state of our program because it will be supplying the list of to do items to our todo list components. So in our container folder we will create a file called "CreateTodo.js". This will involve a form that has an input and submit button that will allow us to type our todos in and submit them to our list. Let's take a look at that file:

```jsx
//############### containers/CreateTodo.js ###############

//import React
import React, { Component } from 'react';

//import connect so we can mesh React and Redux together
import { connect } from 'react-redux';

//redux imports
import { bindActionCreators } from 'redux';

//import an action creator ( we will examine this next )
import { createTodo } from '../actions/index';


class CreateTodo extends Component {
  constructor(props){
    super(props);

    this.state = {
      todoText: '',
    }
  }
  handleInput = (e) => {
    this.setState({todoText: e.target.value});
  }
  handleSubmit = (e) => {
    e.preventDefault();
    this.setState({todoText: ''});
    this.props.createTodo(this.state.todoText);
  }
  render() {
    return (
      <div className="input-form">
        <form onSubmit={this.handleSubmit} className="input-group">

          <input
            type="text"
            className="form-control"
            placeholder="Create a Todo List Item..."
            value={this.state.todoText}
            onChange={this.handleInput}/>
          <span className="input-group-btn">
            <button type="submit" className="btn btn-warning">Create Todo!</button>
          </span>


        </form>
      </div>
    );
  }
}

const mapDispatchToProps = (dispatch) => {
  return bindActionCreators({ createTodo }, dispatch);
}

export default connect(null, mapDispatchToProps)(CreateTodo);
```

The big take away from this file should be the following: We create two methods (function expression style) `handleInput` and `handleSubmit` that will handle the data that the user puts into our application. We pass in an action creator `createTodo` from our /actions/index.js file that will enable us to keep track of each todo item that is created.

At this point we should have a pretty good concept of what kind of application state our program will need. (This is something we should really flesh out in our wire framing process at the very beginning - to determine who will be a container component and who will be a presentational component, and what application state we will have).

So let's now go ahead and take a look at our action creators!

## /actions/index.js  

```jsx
//############### /actions/index.js ##################

import { FILTER_TODOS, CREATE_TODO_ITEM, TOGGLE_TODO } from './actions';

//we could have also imported our action types with a universal operator:
// import * as actions from './actions';
// and then in our type declarations we would just have:
// type: actions.FILTER_TODOS (preface actions. for each)

//start id of todos at 0, add one each time a new one is created.
let newTodoId = 0;


export const createTodo = todoText => {
  return {
    type: CREATE_TODO_ITEM,
    id: newTodoId++,
    payload: todoText,
  };
};

export const toggleTodo = id => {
  console.log('id here', id);
  return {
    type: TOGGLE_TODO,
    payload: id,
  };
};

export const toggleFilter = selectedFilter => {
  return {
    type: FILTER_TODOS,
    payload: selectedFilter,
  };
};
```

So we first import all of our action types from another file (like we would had this been a big program)... we can do that two ways and you can check out the alternative in comments in the code snippet above.

Our entire application will need three types of action creators:

1. **createTodo** : an action creator that creates a new todo when the user submits one, gives it an id, and allows us to pull it from application state to store in a list.

2. **toggleTodo** : an action creator that allows us to cross out our todo items as we complete them. We will be able to click on them and then change the text decoration to have a line through the text. This will also change the complete property from false to true on each todo item.

3. **toggleFilter** : an action creator that will allows us to select from 3 different radio button options (Show All, Show Completed, and Show Incomplete ) - these selections will sort through our todo list based on the completed property of each todo item that is toggled depending on the user's interactions with the list and which items are crossed out.

For each of our action creators we have a type and payload. For our `createTodo` action creator we also add an id property that will incrementally increase with each todo item that is created. You will notice at the beginning of our file, we declare an id property set to zero that we can incrementally increase : `let newTodoId = 0;` and then inside of our action creator we give the id property: `id: newTodoId++` which just adds one to our variable each time we create a todo item.

Now that we have our action creators defined, let's look at our reducers to handle them:

### Authoring Reducers to create todos and filter todos.  

So we really only need two reducers - because the same reducer that will be responsible for creating a new reducer will also be able to track the state of each todo being toggled to completed or incomplete.

Our fist look will be at our root reducer file (index.js) that will combine our reducers into a root file that we can pass to our store in our index.js file.

```jsx
//############## /reducers/index.js

import { combineReducers } from 'redux';
import toggleReducer from './reducer_toggleFilter';
import todoItems from './reducer_create_todos';

const rootReducer = combineReducers({
  todoItems,
  toggleReducer
});

export default rootReducer;
```

This connects everything to our index.js file that looks like this:

```jsx
//################# /src/index.js

//react imports
import React from 'react';
import ReactDOM from 'react-dom';
import registerServiceWorker from './registerServiceWorker';

//import styles
import './styles/index.css';

//redux imports
import { Provider } from 'react-redux';
import { createStore, applyMiddleware } from 'redux';
import reducers from './reducers';

//components
import App from './components/App';


const createStoreWithMiddleware = applyMiddleware()(createStore);

//switch uses most specific route that matches, top down.

ReactDOM.render(
  <Provider store={createStoreWithMiddleware(reducers)}>
    <App />
  </Provider>
  , document.querySelector('.container'));
registerServiceWorker();
```

### Our first reducer...  

Our first reducer will be `reducer_create_todos.js`:

```jsx
//############# /reducers/reducer_create_todos.js ##################

import { CREATE_TODO_ITEM, TOGGLE_TODO } from '../actions/actions';

const todoItems = (state = [], action) => {
  switch(action.type) {
    case CREATE_TODO_ITEM:
      return [
        ...state,
        {
          id: action.id,
          todoText: action.payload,
          completed: false
        }
      ]
    case TOGGLE_TODO:
      return state.map((todo) =>

        (todo.id === action.payload)
        ? {...todo, completed: !todo.completed}
        : todo
      )
    default:
      return state;
  }
}

export default todoItems;
```

At the very top we import our actions, these are simply the strings that will match the action type to the action creator and then match the reducer as well. We import these in our reducer so we have cases to create for our switch/case statement. Next we create our reducer.

We create a function, in this case that will take in state and set it to a default blank array (in the case that our state is undefined, which would throw an error in Redux). It also takes our action as an argument. When it receives all of the action from our dispatch is will pull the action through our switch statement looking at `action.type`.

In the case that the action is `CREATE_TODO_ITEM` we are going to use the spread operator `...` to map through our state and create and return a new array that contains the existing array of state as well. This means that we are able to create a NEW state and not mutate our current state (which Redux would not allow us to do). The second part of our return statement is the new state we would like to merge with the old state. This new state is looking for an id (created by our action), the text `todoText` of our todo item, and setting the completed value to false (since we have just created this todo item).

If the action that is passed through to our reducer is `TOGGLE_TODO` then inside of our return statement, we are going to approach things with the idea being that if a todo item is clicked, then we will find it's id and if the id matches the action id then we are going to toggle the completed value from false to true (if it was false) or from true to false (if it came in as true). We do this by map through our state array (all of our todo items) - we check each todo item to see if it is equal to the action payload (the payload given to the action creator). if they are equal then we use the spread operator to toggle completed (true) to not completed (false) by assigning completed `!todo.completed` or if completed is false, we give it the true value of `todo`.

The last piece of our puzzle is to return a default value of state should none of these cases be matched. We then export the todoItems reducer function.

### Filtering our Lists  

The last big chunk of our puzzle is to be able to toggle one of the radio options (Show All, Show Completed, or Show Incomplete) and be able to filter our list of todo items based on the user's choice of what they would like to see. This will be based on our `toggleFilter` action creator from above - so let's look at the reducer we need to create to make this happen.

```jsx
//############## /reducers/reducer_toggleFilter.js ################

import { FILTER_TODOS } from '../actions/actions';

const toggleReducer = (state = 'showAll', action) => {
  switch (action.type) {
    case FILTER_TODOS:
      return action.payload
    default:
      return state;
  }
}

export default toggleReducer;
```

In the same fashion as before we will import our action type `FILTER_TODOS` (in this case) and create a reducer function. This function will take in the state and set it to the default value of `showAll`, it will also take in the action. Our switch statement is only concerned with one action type `FILTER_TODOS` and in the case of that being true, we will return the action payload. The `action.payload` in this case is the filter option being passed in from the radio button. This filter option `selectedFilter` will allow us to create a switch statement to pick which todo items should be rendered. This switch statement will be part of our `FilteredTodos` container component which we will look at next. The last part of our switch statement is returning a default value of state.

### FilteredTodos container component  

The `FilteredTodos` container component is our container component that will match our filter option to a switch case statement that will toggle the view that we render based on what is selected. See the comments in the code for how this all takes place.

```jsx
//############ /containers/FilteredTodos.js ############

//import connect to connect React to Redux
import { connect } from 'react-redux';

//import our action creator for toggling todo items
import { toggleTodo } from '../actions';

//import our TodoList component concerned with rendering the Todo List
import TodoList from '../components/TodoList';

//create a switch case function named "showFilteredTodos" that receives two arguments.
//1. It receives the todoItems - a list of all items.
//2. It receives the selectedFilter which is one of three things:
// a) showAll (shows all todos without filter)
// b) showCompleted (shows only todos with completed set to true)
// c) showIncomplete (shows only todos with completed set to false)
// we pass these to our cases in the switch and return the appropriate set of list items.
const showFilteredTodos = (todoItems, selectedFilter ) => {
  switch(selectedFilter) {
    case 'showAll':
      return todoItems;
    case 'showCompleted':
      return todoItems.filter(t => t.completed);
    case 'showIncomplete':
      return todoItems.filter(t => !t.completed);
  }
}


//We then need to mapStateToProps, and do setting our todoItems state to the filtered state
//created by our switch statement from above and our toggleReducer function.
const mapStateToProps = state => {
  return {
    todoItems: showFilteredTodos(state.todoItems, state.toggleReducer)
  }
}


//We also need to map our dispatch to props which will take our toggleTodoClick function and dispatch toggleTodo action being passed the id.
const mapDispatchToProps = dispatch => {
  return {
    toggleTodoClick: id => {
      dispatch(toggleTodo(id))
    }
  }
}


//lastly we create our component FilteredTodos and connect our map properties to our TodoList.
const FilteredTodos = connect(mapStateToProps, mapDispatchToProps)(TodoList);

//export for use
export default FilteredTodos;
```

### ToggleFilters container component  

The final remaining container component we need to create is our `ToggleFilters` component that is concerned with the radio buttons that will dictate which list items are rendered. Ignore the classNames, as they are again some styling with Bootstrap 4.

```jsx
//############### /containers/ToggleFilters.js ###################

import React, { Component } from 'react';
import { connect } from 'react-redux';
import { bindActionCreators } from 'redux';
import { toggleFilter } from '../actions/index';


//create a class based component pass in props via the constructor.

class ToggleFilters extends Component {
  constructor(props){
    super(props);

    //set default UI state for the checkbox next to each radio option
    this.state = {
      selectedFilter: "showAll",
    }
  }
  handleOptionChange = (e) => {
    //pass in the event object and get the value (set to a variable for ease of use)
    let option = e.target.value;

    //set UI state of selectedFilter so the radio button will be checked depending on which option
    //is selected.
    this.setState({selectedFilter: option})

    //send the state to the action creator 'toggleFilter' which will pass the selectedFilter
    //to the reducer when called upon.
    this.props.toggleFilter(option)
  }
  render() {
    return (
      <form>
        <div className="form-check form-check-inline">
          <label className="form-check-label">
            <input
              className="form-check-input"
              onChange={this.handleOptionChange}
              checked={this.state.selectedFilter === 'showAll'}
              type="radio"
              name="inlineRadioOptions"
              id="inlineRadioShowAll"
              value="showAll" /> {"Show All"}
          </label>
        </div>
        <div className="form-check form-check-inline">
          <label className="form-check-label">
            <input
              className="form-check-input"
              onChange={this.handleOptionChange}
              checked={this.state.selectedFilter === 'showCompleted'}
              type="radio"
              name="inlineRadioOptions"
              id="inlineRadioShowCompleted"
              value="showCompleted" /> {"Show Completed"}
          </label>
        </div>
        <div className="form-check form-check-inline">
          <label className="form-check-label">
            <input
              className="form-check-input"
              onChange={this.handleOptionChange}
              checked={this.state.selectedFilter === 'showIncomplete'}
              type="radio"
              name="inlineRadioOptions"
              id="inlineRadioShowIncomplete"
              value="showIncomplete" /> {"Show Incomplete"}
          </label>
        </div>
      </form>
      );
    }
};

// mapping the application state of selectedFilter to props so we can use it set the value of our checked off radio property to true or //false and use it to filter our data.
const mapStateToProps = state => {
  return {
    selectedFilter: state.selectedFilter
  }
}

//mapping dispatch to props we bind our action creator (toggleFilter) to our dispatch function.
const mapDispatchToProps = dispatch => {
  return bindActionCreators({toggleFilter}, dispatch);
};

//we connect our ToggleFilters component to the map functions and export for use
export default connect(mapStateToProps, mapDispatchToProps)(ToggleFilters);
```

We have now wired everything up for use... this is how we filter our data based on a reducer and action creator. The remaining presentational (presentational) components are simply for displaying this information. We can look at these below with the comments in the code.

### TodoItem.js  

```jsx
//############## /components/TodoItem.js ##############

//creates a singular todo item to be passed on to the todo list:

import React from 'react'
//if completed is true, we change it's style to have a line through it,
//if completed is false - we give it no special style
//we pass an onClick function to each todo item, that comes from the parent
//component TodoList
const TodoItem = ({ onClick, completed, todoText }) => (
  <li
    onClick={onClick}
    style={{
      textDecoration: completed ? 'line-through' : 'none'
    }}
  >
    {todoText}
  </li>
)

export default TodoItem;
```

### TodoList.js  

```jsx
//################# /components/TodoList.js ###############

import React from 'react';
import TodoItem from './TodoItem';

//pass in our TodoItem component and our toggleTodoClick function, which will recieve the id
//of the todo item that is clicked.
const TodoList = ({todoItems, toggleTodoClick}) => {
  return (
    <ul>
      {todoItems.map( todo => (
        <TodoItem key={todo.id} {...todo} onClick={() =>{
          toggleTodoClick(todo.id)}} />
      ))}
    </ul>
  )
}

export default TodoList;
```

### App.js  

```jsx
//############ /components/App.js ##############

import React, { Component } from 'react';
import '../styles/App.css';


//import components
import CreateTodo from '../containers/CreateTodo';
import ToggleFilters from '../containers/ToggleFilters';
import FilteredTodos from '../containers/FilteredTodos';


class App extends Component {
  render() {
    return (
      <div className="App">
        SUPER DUPER FANCY TODO APP
        <CreateTodo />
        <ToggleFilters />
        <FilteredTodos />
      </div>
    );
  }
}

export default App;
```

Now let's checkout how this all looks on the real application!

![redux-todo-1](https://github.com/rickmurdock/notes/blob/master/ReactJS/images/redux-todo-1.gif)

![redux-todo-2](https://github.com/rickmurdock/notes/blob/master/ReactJS/images/redux-todo-2.gif)

## Conclusion  

* We use our action creators to pass our action to our reducers via dispatch inside of the store.

* The reducers are set of functions that can interpret the action creator functions and manipulate our state according to the action.

* We can use these to filter our data based on certain properties that we pass to our application state.

* Though seemingly complex, the workflow remains the same for all Redux React applications and follows the cycle we have discussed.

### References  

* [Redux Docs Todo App](http://redux.js.org/docs/basics/ExampleTodoList.html)
