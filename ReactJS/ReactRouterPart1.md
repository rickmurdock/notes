# React Router and Single Page Applications  

We've discussed single page applications before, (sometimes referred to as SPAs), and the benefit they present to making web applications. They give us the ability to quickly load and retain data without the need to refresh, making for a much simpler user experience. React Router is a tool that will help with design and implementation of SPAs.

## React Router  

Think of a router as an air traffic controller. The air traffic controller makes sure that all of the planes know when to take off and land. Similarly, a router helps organize the routes a web application should follow, and directs the user or data inputs to the appropriate portions of our web application.

Air traffic controllers also make sure things run efficiently at the airport and try to maximize the flow of traffic so things keep moving––our router does the same thing. A router is designed to intercept information that would normally go to the server, decide what to do with it, and (in the case of React) determine which components should be rendered. That ability to stop communication from going all the way to the server really speeds up our applications and lets us work much more efficiently.

Now let's look at the React Router, and how to integrate it into our applications.

### Installing React Router  

Assuming we have already set up a `create-react-app` application, or used Webpack with the necessary implementations for a React application, we'll start there with an empty application.

First thing we will need to do is go into our terminal and `cd` into our project directory. For this mock up, lets pretend we are in `/TIY/code/router-project/`.

We are going to use npm to install React Router with the following code (again, in our project directory):

```
npm install --save react-router-dom
```

This will install all of the React Router dependencies for a web application that we'll need for our project. This lesson is based off of the current version of React Router, **version^4.1.1**.

### Our Application  

There are a couple of ways we can utilize React Router in our SPA, but here are the most important details:

1. Whether we are using a component or our `index.js`, React Router is the single source of output for our application.

   * That means that all of our application will flow through our routing components in order for React Router to work.

2. React Router will require the use of a few different key components working together.

   * We will examine the `<BrowserRouter>`, `<Route>`, `<Switch>`, and the `<Link />` components employed by React Router v4.

Since this is a simple application, we'll begin in our `index.js`. Normally we'd have a `ReactDOM.render()` statement that plugs our app into the container `<div>` in our HTML.

### index.js  

Inside of our `index.js` file let's look at the set up for our app and how we can incorporate the beginnings of our React Router app.

First we'll see our normal list of React dependencies (React and ReactDOM) being imported, as well as our style sheet. Underneath our styles import, we see the importing of React Router with the `{BrowserRouter, Route, Switch}` dependencies specifically.

```jsx
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

ReactDOM.render(
  <BrowserRouter>
    <Switch>
      <Route path="/page_one" component={PageOne} />
      <Route path="/page_two" component={PageTwo} />
      <Route path="/" component={App}/>
    </Switch>
  </BrowserRouter>
  ,
  document.getElementById('root')
);

registerServiceWorker();
```

Looking at the code above, we can see that inside of our `ReactDOM.render` statement we have a component wrapping our entire application called `<BrowserRouter>`. The `<BrowserRouter>` is a router that uses the HTML5 history API to keep the UI of your application in sync with the URL the user is on. The `<BrowserRouter>` allows us to return a single container for our application as is required in JSX.

The next component we see inside of `<BrowserRouter>` is the `<Switch>` component. `<Switch>` allows us to render a `<Route>` (which we will discuss shortly) based on a specific set of criteria, i.e. an exact URL match.

The `<Route>` component specifies the URL route that should exist for a given component. It has two major attributes; the `path=` and `component=`. The path attribute takes the name of the route that should exist for a given component, for example, `path='/page_one'`. Because `Switch` will match the route to the first criteria, the `Route` components should be listed in order of specificity, with the most complex routes listed first and the least complex listed last.

### The Order of Routes  

Let's pretend that we had written the first route component with a `path='/'`.

```jsx
<Route path="/" component={App}/>
<Route path="/page_one" component={PageOne} />
<Route path="/page_two" component={PageTwo} />
```

`Switch` would then look at our application, and since every route begins with a forward slash `/` they would all match the first route component. Therefore, no other route would rendered no matter what the URL was. Since we list the `path="/"` `<Route>` last, allows each component to be read and matched before finding a component that is just a slash.

We could make one change to this by using the `exact` path keyword. In the above example, where our routes are ordered from least complex to most, we could add `<Route exact path="/" component={App}/>` to our first route and therefore only if the path was a `/` would it be matched. However, there are problems with this logic when dealing with complex routes that are dynamic in nature (changing with data or input). The dynamic routes might fail to yield the expected results when using `exact`, so it's best practice to order our routes from most to least complex, to avoid issues.

```jsx
<Route path="/page_one" component={PageOne} />
<Route path="/page_two" component={PageTwo} />
<Route path="/" component={App}/>
```

### Rendering Components  

As you can see from our very first code snippet, we also imported the components from our other files in our list of import statements.

These import statements allow us to utilize that component inside of our router (just as we could render them normally before a router). The second part of the route component is the `component={}` attribute. Passing in the name of the component (as we imported it to be) tells the router to render the component when the correct path is met as a URL. So when we have the URL `"example.com/page_one"` we expect our path to match the first `<Route>` component and display the `<PageOne>` component to the user.

## Inside Components  

Now lets check out our `page_one.js` file (in our mock app) where our `<PageOne>` component is being exported from.

At the top of the page you'll see our normal imports of React and React component.

Then next major part of React router is seen in the following import state `import { Link } from 'react-router-dom';`.

The `<Link />` component is exactly what is sounds like, a link. This takes the place of our normal anchor tags `<a>` (but actually renders an anchor tag in HTML) and comes with some extra features.

```jsx
//######### page_one.js #############

import React, { Component } from 'react';
import { Link } from 'react-router-dom';

export default class PageOne extends Component {
  render() {
    return (
      <div className="App one"><div className="header">Page One!</div>
        <p className="para">Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
        <div><button className="btn"><Link to="/">Main Page</Link></button></div>
        <div><button className="btn"><Link to="/page_two">Page Two</Link></button></div>
      </div>
    );
  }
}
```

For UI purposes the `<Link />` component is wrapped inside of a button element. The real take away here is what goes on with `<Link />`:

```jsx
<Link to="/page_two">Page Two</Link>
```

The `<Link />` component has one primary attribute: `to=`. It simply specifies the route to which that link should take the user via React router. Here on our `PageOne` component we have links to our `MainPage` component and our `PageTwo` component inside of the two `<Link />` components. We can use any method we have already learned to provide text for the links between the opening and closing `<Link />` tags. We kept it simple here with plain text, but could have added some JavaScript by using `{}`.

It's important to know that what is actually rendered by the `<Link />` component is an anchor tag `<a>` for styling purposes, though we could pass a `className` to each link as well. In this instance, we could have styled the anchor tags by doing the following:

```css
/*******STYLE SHEET********/
.btn a {
  text-decoration: none;
  font-size: 20px;
  color: black;
}
```

Here we have a `className="btn"` for each of our `<button>` elements. Inside of those we have the anchor tag which we style via regular CSS in the above code snippet (we could also nest this using Sass).

## The End Result  

Here's a simple three page application (with minimal styling for demonstration) built with React router:

![routing.gif](https://github.com/rickmurdock/notes/blob/master/ReactJS/images/routing.gif)

## Conclusion  

* React router allows us to create simple and efficient single page applications.

* We can use npm to install React router for web applications by typing: `npm install --save 'react-router-dom'` in our project folder in terminal.

* The `<BrowserRouter>` component allows us to use the history API for the browser, which in later projects we will examine the importance of.

* The `<Switch>` component allows us to match a `path=` to a `component=` and render that component only when the path matches the exact router specified.

* We organize our `<Route>` components by complexity of their `path=""` attribute. The more complex routes are listed at the top, and the least complex are listed at the bottom.

* Each `<Route>` component has two main attributes - the `path` and `component`. The path attribute is the URL endpoint we which to match against for our component to render. The component attribute is the component we wish to render when the paths match.

* Inside of our application components, we can import the `<Link />` component which will create an anchor tag element when rendered. `<Link />` takes a `to=""` attribute and specifies the path to which the user should be taken to upon some sort of event (click, etc.).

### References  

* [React Router](https://reacttraining.com/react-router/web/guides/quick-start)

---

# Hash Based Routing vs. Resource Routing  

Routing simply means your app responds in some way to a change in the browser's URL. By now you are familiar with the client-server model: the web browser is a *client* program that requests a resource (web page) from the *server*. Resource routing refers to routing done on the back-end, for example using NodeJS. Hash-based routing is done on the front-end. Let's review what you know about resource routing.

## Resource Routing  

Resource routing refers to back-end routing. We've already covered a bit of this using NodeJS. We used the POST and GET request methods to communicate with the server. In resource routing, those methods send a request to the server, the server then searches for the resource you requested, and responds back with a static file that is stored on the server. Routing, in this case, is handled by the server or back-end.

## What is Hash Based Routing?  

Hash based routing is a method of routing that uses the hash symbol to designate the path that the router should direct the browser onto. These symbols are placed after the base URL and before the route. `http://myrecipes.com/#/hashbrowncasserole`. This essentially separates the URL and creates a fragment of the remaining piece of URL, called a fragment identifier. The baseURL is sent to the server, while the fragment identifier is stored in local storage. The server processes the request for the base URL and returns the default `index.html`. JavaScript is returned to the browser, the code is run, and a query is sent to the server with the fragment. This is shown to the user in the view. To be able to process the hash-bang URL, the browser it's being sent to needs to be using JavaScript. We'll go into more detail about the issues this may cause in the next lesson.

With the URL example from above: Given `http://myrecipes.com/#/hashbrowncasserole` the base URL `myrecipes.com` will be sent to the server, the fragment identifier `/hashbrowncasserole` will be stored in local storage. JavaScript code is returned to the browser after the initial page is pulled, prompting the browser to then send back the fragment as a query to the server.

### hash-bang  

You'll sometimes see the hash symbol followed by the exclamation mark: `#!`. This is referred to as *hash-bang*. This was created to allow for Ajax crawlable URL strings, meaning search engines could find your content. However this has since been deprecated by Google, and it's recommended to use the **[History API](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/API/history)**.

## Drawbacks of Hash-Based Routing  

If the user has JavaScript disabled in their browser, the routing won't work and the app will not be navigable. Another issue with using hash-based routing is that the URL fragments don't appear in browser history.

A side effect of this is that URLs that differ only in fragments will appear as one entry in browser history, which means users can't hit the back button to navigate on your page. They also can't bookmark a specific page that's pulled from a fragment.

## Conclusion  

When you hear the terms resource routing and hash routing, you now know that they refer to back-end vs front-end routing methods. There are advantages and disadvantages to each which we will cover more fully in the next lesson.

### References  

* [Basic resource routing](https://expressjs.com/en/starter/basic-routing.html)

* [Client Server](https://en.wikipedia.org/wiki/Client%E2%80%93server_model)

* [Simple summary front-end vs back-end routing](https://medium.com/@kennedyjt88/frontend-routing-vs-backend-routing-874b2bc41e5a)

---

# Using BrowserRouter to Enable Browser History  

In the past with React Router to avoid using hash based routing and to use resource routing we had to jump through a few hoops. We don't need to go into specifics but just understand it was not as easy as it is now. React Router version 4 has made life much simpler for us.

## Enabling Browser History  

React makes life simple by taking the best of both worlds. What this gives us is the ability to track the history of our application within the browser and at the same time render our routes dynamically and with speed like hash-based routing.

By enabling the history API in the browser, we can track a user's endpoints and allow them to get back to a previous page by using the back button and send links to other people that will direct those people to the exact contact the original user wanted them to see.

### The Setup  

React Router's new `<BrowserRouter />` component has the history API included. To use it, we must wrap all of our routes with it. If we want to render one route at a time on our page, we will need to use the `<Switch />` component. The `<Switch />` component is an exclusive route provider, or `exclusive rendering`, meaning it seeks the route that matches the path and once found it will not render anything else. This will be our focus as we learn to render multiple pages as a SPA. The alternative would be to use `inclusive rendering`, in which all routes are displayed on a page. Let's check the code:

```jsx
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

We now have access to the browser's history API. We can let users navigate our application and be able to return to each endpoint or direct other users to the same endpoint.

## Conclusion  

* Using React Router 4's `<BrowserRouter />` component, we automatically gain the ability to use the history API.

* **Exclusive rendering** is rendering a route based on the strict matching of a path.

* **Inclusive rendering** is rendering of routes based on meeting a partial match of the path allowing for multiple routes to be displayed.

* Use the `<Switch />` component to ensure that only one route will be displayed at a time. This is *exclusive rendering* as opposed to *inclusive rendering* where all routes would be displayed.

### References  

* [Switch Component](https://reacttraining.com/react-router/web/api/Switch)

* [BrowserRouter](https://reacttraining.com/react-router/web/api/BrowserRouter)

---

Hash-Based Routing Drawbacks  

In the previous lesson, you learned about hash-based routing. Let's use an analogy to think about how routing works. Imagine you want to read a book, so you go to the library.

Depending on what you're looking for there two ways you can find what you're looking for:

You walk in and tell the librarian you would like to read something by Maya Angelou. The librarian pulls up a cart with all 100,000 of his letters. Then the librarian asks you which title you would like to read. You tell them, and they pull out and hand you the book.
You walk in and tell the librarian the title of the book you would like to read. They grab that specific book and hand it to you.
The first scenario would be hash-based routing. Using the hash, you pull the main URL of the website (in the analogy - the cart of all things written by the author). After the server grabs the code, the browser pulls the fragment identifier (specifying exactly which page you want) out of local storage and sends a query back to the server. Then, the server returns the exact page matching the query.

The second scenario is resource routing. You send the full URL with a query string to the server, and it fetches the exact resource you require. They work in different ways, but both methods work.

There's also a third scenario. Suppose you tell the librarian the name of the author you'd like to read. They come back with the cart, ask you the title of the book, but is speaking a language you don't understand. Your inability to communicate with the librarian is similar to what happens when the browser you're using isn't running JavaScript. The returned code can't be read, which means you end up with a broken link.

Hash It Out  

Why use hash routing at all? The hash-bang was originally intended as a means to help search engines index dynamically loaded AJAX websites. It can help to speed up page loading time since the browser will load the static HTML content of the base URL once, then download the dynamic content related to the fragments as they are requested. If the bulk of your site is made up of static HTML (with dynamic content being a minor component), then hash routing will help your pages load faster.

Another issue with using hash-based routing is that the URL fragments don't appear in browser history. The browser loads the page faster because it ignores URL fragments where the base URL is the same and loads the HTML from the cache. A side effect of this is that URLs that differ only in fragments will appear as one entry in the browser history, which means users can't hit the back button to navigate on your page. They also can't bookmark a specific page that's pulled from a fragment.

Conclusion  

mp-newt-got-better.jpg
There are other ways of routing that we can use to give the best possible user experience. One such example we have already covered, using the HTML5 History API.

More Reading  

Before search engines, people used 'crawling' URLs to be able to search for things on the web. In 2011, when Gawker/Lifehacker redesigned their sites with hashbangs resulting in site-wide outages. You can read about it in detail here but the gist is that the URL with the hashbang depends on JavaScript to retrieve the content, and when the JavaScript failed to load properly, every URL on their page was broken.

References  

Broken Links

Breaking the Web with HashBangs
