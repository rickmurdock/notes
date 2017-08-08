# Testing React applications using Jest  

When working with Node, you wrote tests for your back-end applications. You may have used [Mocha](https://mochajs.org/) or [Jest](https://facebook.github.io/jest/). To test React applications, we will use Jest, as it was created by Facebook, the creators of React, to test all JavaScript code, including JSX.

We'll start with a review of Jest, and then move into how it works with React code.

## An overview of Jest  

Jest uses three functions, `describe`, `test`, and `expect`, to define tests. The important thing to remember about these is that you *do not have to require them*. They are made available to your tests by Jest.

```jsx
// test/sample.test.js
describe("A string", function () {
  test("it can be uppercased", function () {
    expect("Hello".toUpperCase()).toBe("HELLO");
  });

  test("it can be lowercased", function () {
    expect("Hello".toLowerCase()).toBe("hello");
  })
});
```

```sh
$ jest
PASS  test/sample.test.js
 ✓ it can be uppercased (4ms)
 ✓ it can be lowercased

Test Suites: 1 passed, 1 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        1.069s
Ran all test suites matching "sample.test.js".
```

`describe` is used to set a context for tests. It can be nested, with multiple `describe` calls inside a parent. `test` is used to write individual test cases. You can use `test` outside of a describe block with no problem.

`expec`t sets up [matchers](https://facebook.github.io/jest/docs/en/using-matchers.html#content), which allow you test values. Read [an introduction to matchers](https://facebook.github.io/jest/docs/en/using-matchers.html#content), and then [see a list of all matchers](https://facebook.github.io/jest/docs/en/expect.html#content).

## Running Jest  

If you are using create-react-app, Jest is set up for you! If you are not using create-react-app, [see the instructions on setting up Jest](https://facebook.github.io/jest/docs/en/expect.html#content).

If you are using create-react-app, run `npm test` or `yarn test`. If you are not using create-react-app, run `jest`.

With create-react-app, running `jest` alone will not work. `npm test` sets up configuration you need for jest to work.

By default, Jest runs all files under a `__tests__` directory or that end in `.test.js`.

## Unit testing  

Testing JavaScript code that does not touch the DOM or is not a React component is as easy as you'd expect. You can import modules you need and write tests for them. Here is an example of testing a Redux reducer:

```jsx
// src/reducer.test.js

import reducer from "./reducer";
import { createTodo } from "./actions";

describe("CREATE_TODO", function () {
    test("creates a new todo", function () {
        const initialState = {todos: [], nextId: 1}
        const state = reducer(initialState, createTodo("Test"));
        expect(state.todos).toHaveLength(1);
        expect(state.todos[0]).toEqual({id: 1, done: false, text: "Test"});
    })

    test("updates nextId", function () {
        const initialState = {todos: [], nextId: 1}
        const state = reducer(initialState, createTodo("Test"));
        expect(state.nextId).toEqual(2);
    })
})
```

## Snapshot testing  

[Snapshot testing](https://facebook.github.io/jest/docs/snapshot-testing.html#content) is a type of testing specifically for UI where your UI is rendered and compared to a reference to make sure it did not change. Jest gives us the ability to write snapshot tests for React components. Snapshot testing is easier demonstrated than described, so let's see an example first.

We have a `<Todo>` component in our application, defined below:

```jsx
// src/components/Todo.js
import React, {Component} from 'react';

class Todo extends Component {
    render() {
        const {todo, handleToggle} = this.props;

        return (
            <div className="Todo">
                <input type="checkbox" checked={todo.done} 
                       onChange={event => handleToggle(todo.id)}/>
                <span className={todo.done ? "todo-done" : ""} 
                      onClick={event => handleToggle(todo.id)}>{todo.text}</span>
            </div>
        )
    }
}

export default Todo;
```

We want to write a test that renders our Todo component and compares it to a reference snapshot.

```jsx
// src/components/Todo.test.js
import React from 'react';
import renderer from 'react-test-renderer';
import Todo from "./Todo";

test('Todo renders correctly', () => {
    const tree = renderer.create(
        <Todo todo={{text: "Test", id: 1, done: false}} />
    ).toJSON();
    expect(tree).toMatchSnapshot();
});
```

Run `npm test` at this point. You may get an error about react-test-renderer. If so, run `npm install --save react-test-renderer` before continuing.

Once you have this running successfully, you will see a new directory under `src/components/` called `__snapshots__`. In there, we have a snapshot:

```jsx
// src/components/__snapshots__/Todo.test.js.snap
// Jest Snapshot v1, https://goo.gl/fbAQLP

exports[`Todo renders correctly 1`] = `
<div
  className="Todo"
>
  <input
    checked={false}
    onChange={[Function]}
    type="checkbox"
  />
  <span
    className=""
    onClick={[Function]}
  >
    Test
  </span>
</div>
`;
```

This file contains the rendered view of our Todo component. It isn't fully rendered to HTML, as you can see by the `checked={false}` statement among other things.

This seems to break the concept of test-driven development! We wrote a test that generated its correct answer. Why is this useful?

Snapshot testing is used to prevent unwanted changes. If our Todo component changes at all, we will fail this test. If I change `Todo.js` to add a class to todos that are not done, I get this failure from `npm test`:

```jsx
 FAIL  src/components/Todo.test.js
  * Todo renders correctly

    expect(value).toMatchSnapshot()

    Received value does not match stored snapshot 1.

    - Snapshot
    + Received

    @@ -5,11 +5,11 @@
         checked={false}
         onChange={[Function]}
         type="checkbox"
       />
       <span
    -    className=""
    +    className="todo-not-done"
         onClick={[Function]}
       >
         Test
       </span>
     </div>

      at Object.<anonymous>.test (src/components/Todo.test.js:9:18)
      at Promise.resolve.then.el (node_modules/p-map/index.js:42:16)
```

If this were an inadvertent change, this test would catch that. In our case, it's intentional. To fix it, we update our snapshot by running `npm test -- --updateSnapshot` or by pressing `u` in the test watcher that `npm test` starts.

If you run `npm test -- --updateSnapshot`, make sure to exit after it updates. Otherwise, the watcher will continue to update snapshots automatically when they fail, which eliminates their use.

## Testing React components with Enzyme  

You have seen how to write snapshot tests, but often we need to interact with components in our tests. To do that, we will use a tool called Enzyme. Enzyme lets us render our components in our tests and use a simple set of methods to manipulate them.

First, install enzyme: `npm install --save enzyme`.

Then, import shallow from Enzyme:

```jsx
import { shallow } from 'enzyme';
```

`shallow `is a function that renders a React component and returns it in a container, as if it were mounted in the DOM. Enzyme comes with another function to do this, mount, but we will use shallow, as it does not render sub-components.

To get a component wrapped with Enzyme, write code like the following:

```jsx
import { shallow } from 'enzyme';
import Todo from "./todo";

test("it can be set to done", function () {
  const wrapper = shallow(<Todo text="Testing" />);
  // TODO finish test
})
```

Once you have that, you have all the methods of the Enzyme renderer at your disposal. An example:

```jsx
import { shallow } from 'enzyme';
import Todo from "./todo";

test("it can be set to done", function () {
  const wrapper = shallow(<Todo text="Testing" />);
  expect(wrapper.state('done')).toEqual(true);
  wrapper.find('.done-button').simulate('click');
  expect(wrapper.state('done')).toEqual(true);
})
```

One last tip: if you need to call a method on the component, you cannot call it on your wrapped component directly. You have to call wrapper.instance() to get access to the method:

```jsx
import { shallow } from 'enzyme';
import Todo from "./todo";

test("it changes color when done", function () {
  const wrapper = shallow(<Todo text="Testing" />);
  expect(wrapper.state('done')).toEqual(true);
  wrapper.find('.done-button').simulate('click');

  expect(wrapper.instance().todoColor()).toEqual("green");
})
```

## Testing Redux  

Many parts of Redux are easy to test with unit tests, specifically action creators and reducers. Testing stores and asynchronous code is harder, but [the Redux documentation covers how to test all parts of Redux with Jest](http://redux.js.org/docs/recipes/WritingTests.html).

## Further Reading  

* [Jest: Mock Functions](https://facebook.github.io/jest/docs/mock-functions.html)

* [Snapshot Testing: Use with Care](http://randycoulman.com/blog/2016/09/06/snapshot-testing-use-with-care/)
