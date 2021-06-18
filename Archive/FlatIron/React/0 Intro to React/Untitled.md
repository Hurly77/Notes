# Intro React

Using **JSX**, an extension of vanilla JavaScript with a specific syntax, we can write code that looks very similar to HTML. Snippets of this JSX get separated out into components, sort of like building blocks.

 React includes:

- a **virtual DOM**, that allows for fast, efficient, content rendering. Great for highly interactive apps;
- a **declarative writing** structure, allowing you to simply express how your app should look and let React handle updates and underlying data changes;
- **Babel**: an included transpiler that converts modern JavaScript and custom code like JSX into more widely compatible JavaScript;
- **Webpack**: a 'bundler' that takes all our work, along with any required dependency code, and packages it all up in a single, transferable bundle
- Built in ESLint functionality to help improve our code;

React also has a recommended tool, `create-react-app`, that makes it easy to create a new React project from scratch. The `create-react-app` tool sets up a preconfigured, default, project, ready to launch with `npm start`. This package includes functionality built to be mobile friendly and progressive. That means apps will work on all modern browsers and mobile devices.

# React app Ex.

to start up a server for the app to run on:

```
npm start
```

```jsx
<div className="App">
  <header className="App-header">
    {moment().format('MMMM Do YYYY, h:mm:ss a')}
  </header>
  <p className="App-intro">
    In React apps, we write JSX - it looks like HTML, and uses a lot of HTML syntax.
    JSX lets us include JavaScript functions right along with the HTML, and also
    allows us to add in components, which are separate, self-contained chunks of JSX.
  </p>
  <ExampleComponent />
</div>
```

JSX code, we've got one `div` that contains three child elements, `<header>`, `<p>` and `<ExampleComponent />`. In your browser, *these* are the elements being displayed! The `<header>` provides a timestamp of the exact time the app was loaded. The `<p>` section includes the brief text on JSX.

The `ExampleComponent` contains the sunglasses GIF. In the `src` folder, take a look at `ExampleComponent.js`. You'll see a file very similar to `App.js`, containing `<img>` and `<p>` elements.

 Moving out from the middle, we see this JSX code is the *return* value of a function, `render()`. This function is contained within a `class`:

```jsx
class App extends Component {
  render() {
 
    return (
      // JSX goes here!
    )
  }
}
```

## Importing, Exporting, and the Component Chain

```jsx
import React, { Component } from 'react'
import moment from 'moment'
import ExampleComponent from './ExampleComponent'
import TestComponent from './TestComponent'
 
// class blah blah extends whatever render something etc...
 
export default App
```

`App.js` is *pulling in* specific content from these two packages! You can see in the App class that `Component` and `moment` are both being used in the `render()` method. They are being *imported* from the `node_modules` folder.

The imports for `ExampleComponent` and `TestComponent` are slightly different. In this case, `App.js` is importing files in the same directory, like `./ExampleComponent`, which allows it to use `<ExampleComponent />` in the `render()` method.

By including the `export` line, we are allowing *other* files to *import* things from the `App.js` file.

**You can have only one default export per file**

```js
import App from './App';
```

This structure of importing and exporting allows for files to 'chain' together. `ExampleComponent.js` has an `export` statement as well (take the time to locate it), and is imported into `App.js`. Additionally, `App.js` is imported into `index.js`.

The `index.js` file doesn't have an export. It is the 'top' of this chain. Inside `index.js` is some regular JavaScript, `document.getElementById('root')`. Even though React is a modern, complex framework, it still relies on a regular `index.html` file to load the JavaScript!

# useful tools

1. If you're using Atom as your text editor, download a JSX plugin (a quick search for 'JSX' will reveal the most popular ones). This will give you some pretty great syntax highlighting for code that is specific to React, and will make coding easier. After the package is installed, open up your `preferences` in Atom. Depending on which package you choose, you may have to edit its settings so that it is the default syntax highlighter for files appended with `.jsx`
2. Install the [React Devtools Chrome Extension](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en). This gives you access to some pretty great tools that make it a lot easier to debug your programs in React. Here is the [repo](https://github.com/facebook/react-devtools#faq) for the extension.
3. Most importantly, the official [React Documentation](https://reactjs.org/), courtesy of Facebook. While you won't have the time to read over all of it now, it might be good to have bookmarked while you learn React.

```js
fetch("/api/foo")
  .then(response => {
    if (!response.ok) { throw response }
    return response.json()  //we only get here if there is no error
  })
  .then(json => {
    doSomethingWithResult(json)
  })
  .catch(err => {
    err.text().then(errorMessage => {
      displayTheError(errorMessage)
    })
  })
```

