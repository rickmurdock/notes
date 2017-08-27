# Webpack for React

## Configure Webpack for React.

Configuring Webpack to transpile React code is simple. Once you know how to set up your basic **webpack.config.js** all you have to do is install a new babel preset!

Install the react preset for babel by running: `npm install --save babel-preset-react`.

Now, when you run `webpack` or `webpack-dev-server` your React code will be transpiled from either pure React syntax or JSX into pure JavaScript. If you are using es6 features, such as the es6 class syntax, you will have to use the **babel-preset-2015** to further convert your code to es5 that the browser can understand.


Using the sample webpack.config.js file from http://coursework.vschool.io/intro-to-webpack/, add your react preset to your presets.

```jsx
module.exports = {  
    entry: './main.js',
    output: {
        filename: 'bundle.js'
    },
    module: {
        loaders: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'babel',
                query: {
                    presets: ['react', 'es2015']
                }
            }
        ],
    }
};
```

It is important that the *react* preset come before the *es2015* preset as webpack will transform the code in the order that you tell it to. Your JSX and React code must first be converted to JavaScript and then from es6 to es5.

### .babelrc  

Babel is a fairly common transpiling tool. It is used by most companies and developers in one way or another. Configuring your babel loader in your *webpack.config.js* file is completely acceptable, but it is more common to move some of your babel configuration into a separate file. This will allow you to add additional configuration properties to Babel and maintain it all in one place.

Create a **.babelrc** file (the . is important here) by running: `touch .babelrc`. This will create your *.babelrc* file. Although there is no file ending like ".js" or ".json", this will default to a JSON file. Then update it as below.

```jsx
{
  "presets" : ["react", "es2015"]
}
```

The next step is to update Webpack.

open webpack.config.js file and update it as below:

```jsx
loaders: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'babel',
            }
        ]
```

We simply deleted the *query* object. Babel knows to look for the *.babelrc* file for further configuration info.

Now change your your entry file to include some react. Make sure you have the `react` and `react-dom` node modules installed and update your file as below.

```jsx
import React from 'react';
import ReactDOM from 'react-dom'

class Hello extends React.Component{
    render(){
        return (<h1>Hello World! My Webpack works with React!</h1>) 
     }
}

ReactDOM.render(<Hello/>, document.getElementById("app"))
```

Now update your index.html to include a div with an id of *app*.

```html
<!doctype html>  
<html>  
    <head>
        <meta charset="utf-8">
        <title>Webpack fun</title>
    </head>
    <body>
        <div id="app"></div>
        <script src="bundle.js"></script>
    </body>
</html> 
```

Run your webpack server by running `webpack-dev-server` and visit http://localhost:8080/webpack-dev-server/. If everything went well, you should see your h1 element and text!

### Auto Refreshing  

One of the nice things about the webpack-dev-server is that it can automatically update when you save changes to your codebase. You can configure your webpack-dev-server to do this by using the hot-reload functionality.

Enable this by opening your *package.json* file and add a new script.

```jsx
"scripts": {
    "start": "webpack-dev-server  --content-base <entry file here> --inline --hot"
  }
```

After *--content-base* you must put the name of the folder that contains your entry JavaScript file. For example, if *index.js* were my entry file and it was located in my *src* folder, my script would look like this:

```sh
"start": "webpack-dev-server  --content-base src --inline --hot"
```

If my entry is nested inside of another folder, you will need to provide the path to that folder.

```sh
"start": "webpack-dev-server  --content-base src/js --inline --hot"
```
