# What is a Single Page Application?  

At this point, you've learned the basics of front-end development. You've also learned to create multi-page websites 
combining HTML content, CSS styling, and JavaScript functionality. Multi-page websites make up the majority of websites 
visited, but there is a growing trend that favors *single-page applications* over the traditional *multi-page applications*. 
This lesson will define and explore the benefits of *SPAs*, or single-page applications.

### Multi-Page Application Architecture  

Most websites built as a MPA, or multi-page application, use HTML to define the architecture and JavaScript for providing 
functionality. A MPA also displays and submits data back to the server by sending multiple requests. With each request made 
a new page is rendered by the browser. This is why you’ll notice a distinct page change followed by loading time when 
navigating from page to page within most websites. The loading times will vary from site to site and are directly tied to 
the amount of resources needed to render the page.

In addition, each request translates to a full re-render of the page on the server side. Therefore each request might 
consist of multiple AJAX calls, loading multiple files (such as images), and so on. In conclusion, each request triggers a 
chain of resource consuming events that directly affect performance. These behaviors can make for a less than ideal user 
experience.

### JavaScript in SPAs  

Let's dive deeper into the role JavaScript plays when creating SPAs. You've already learned how to add functionality to a website by using JavaScript to interact with the user and modify existing page content. In a SPA, JavaScript is used to do all the heavy lifting, from creating HTML content to handling AJAX requests and making API calls.

#### Object-Oriented Design  

You can think of a SPA as a collection of interactive pieces or pages––all coming together on the client-side in a single HTML document. When it comes to object-oriented design, properly creating and managing these pieces depends on a well-architected code base. Many people use existing frameworks for creating a SPA. Some of the most popular frameworks are:

* React

* Angular

* Ember

* Backbone

### Benefits to the User  

SPAs are known for providing a seamless user experience. They allow the user to enjoy an experience similar to a native 
desktop application. The experience is fast and responsive, and provides immediate feedback (information flow, filtering, 
faster searches, etc). This is because most of the changes and updates to the UI occur via JavaScript and CSS, instead of 
taking place on the server-side.

#### Load and Forget  

In a SPA there is only an initial page load, with the server acting as a service layer responding to HTTP request for data 
and returning data, not markup (usually as JSON data). Because the data is initially loaded, the browser isn't slowed down 
with additional request when using the application.

#### Keeping it Light  

A well designed SPA can make fewer resource requests than its MPA equivalent. A small update on an MPA page requires a 
request followed by a full render of that page. Let's pretend that each page shares the same header with images, a sidebar 
with a video, and a footer––with the only difference in each page is an article element.

When the user navigates between pages, or something within the article changes, then everything is re-rendered. In a SPA 
the only thing that would require rendering would be the article element itself.

### Conclusion  

While a lot of the web is built using MPAs, a new trend of programmers using SPAs for their projects is emerging. SPAs can 
provide a desktop-like experience for the user from within the browser. SPAs can be a fast, dynamic, and pleasant experience 
for the user.

---

# Using React to Create a Single Page Application  

### What Is React?  

*React* is not really a framework, but a library for creating user interfaces. Unlike frameworks that rely on the MVC 
pattern in order to separate the view from the code, React focuses primarily on the concerns of the view using *components*. 
Components let you divide the UI into smaller, reusable elements. These components are used to display data as it changes 
over time.

### Why Use React?  

React is a JavaScript library that allows the view to change when data changes. Only the necessary number of components are 
updated and rendered when data changes. Components are encapsulated and only care about their own state. Multiple components 
can be composed into more complex UIs.

What that does this mean? If we are authoring a messaging app, we would want a message to be rendered every time a new 
message hits the database. Most of the UI components don't need to be refreshed every time a message is added. React makes 
sure only the component with new data is refreshed, leaving the rest of the components as they are.

#### Components & Reusability  

Think of a React app as a larger component made up of smaller 'smart' components. These components hold the logic needed 
to process data. Each 'smart' component is made up of even smaller 'simple' components. These 'simple' components are 
presentational components, responsible for rendering the 'smart' components' data. Each component is responsible for *one 
thing*, only. This is known as the [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single_responsibility_principle). Using small components creates separation of concerns 
and maximizes code reuse. This also generates modular testable code.

#### Component Basic Architecture and Hierarchy

In the example below, each box represents a component:

* FilterableProductTable (orange): contains the entirety of the example.

* SearchBar (blue): receives all user input.

* ProductTable (green): displays and filters the data collection based on user input.

* ProductCategoryRow (teal): displays a heading for each category.

* ProductRow (red): displays a row for each product.

![react_components](https://github.com/rickmurdock/notes/blob/master/ReactJS/images/reactComponent.png)

> Read more about component architecture and hierarchy on [Thinking in React](https://facebook.github.io/react/docs/thinking-in-react.html)

#### High Performance Virtual DOM  

When it comes to dynamically updating the DOM with JavaScript, React is very powerful. Updating the DOM is normally the 
Achilles' heel of web performance. Updating the DOM can create bottlenecks which negatively affect web performance and user 
experience. React does not update the actual DOM in its entirety with every view update. It maintains a virtual DOM where 
only the parts of a component in which the data has changed are updated in the view.

React does this by keeping the DOM in memory. After a view update is reflected in the virtual DOM, React compares the 
previous state of the component with the current state of the virtual DOM and decides how to apply these changes using the 
minimum amount of updates. These updates are then applied to the DOM, maximizing read/write performance.

#### Routing & Back-end  

React does need help with two things; loading data from the back-end, and client-side routing. As a developer you will need 
to incorporate other libraries, such as `Flux` to have a fully functional and robust app.

> It's worth mentioning that React has other internal processes that make it all work. You will soon learn about JSX, 
rendering a component's life cycle, props and state.

### Use Case Scenarios  

#### When to Use React  

We've alluded to this already, but you should consider building an SPA with React when the view has to dynamically and 
automatically update based on frequent data changes. You also want to use React to create a high performance SPA that can 
consume large amounts of data in a fluid, responsive, and seamless manner.

#### When Not to Use React  

React is not ideal for projects where data change is minimal or when building a static site where the view is not 
manipulated by data.

### Conclusion  

React is a powerful view library that makes it possible to build high performance SPAs. React lends itself to scenarios 
where data changes frequently. It's component-based nature also makes the code highly reusable and testable.
