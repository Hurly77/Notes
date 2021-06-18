## Use the Provider component from React Redux

 Run `npm install react-redux --save` to install it and add to our `package.json`

With **React Redux** we have access to component called the **Provider**. The **Provider** is a component brought to us by the **React Redux** library. This component wraps around our **App** Component. it does two things for us. the first is that it will alert out **Redux** app when there has been a change in state, and this will re-render our **React** app.

Lets add this to `src/index.js`

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import { createStore } from 'redux';
import { Provider } from 'react-redux'; /* code change */
import shoppingListItemReducer from './reducers/shoppingListItemReducer';
import App from './App';
import './index.css';
 
const store = createStore(
  shoppingListItemReducer,
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
);
 
ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>, /* code change */
  document.getElementById('root')
);
```

We just 

- Imported `Provider` from React Redux
- used `Provider` to wrap our React application
- passed our store instance into `Provider` as a prop, making it available to all of our other components.

### Step 2: Connecting The Container Component to Store

By Using `Provider` Component we “provided” store to our components. But, we don’t want every component to re-render with every change to state. We can use **connect()** function to fix this.

#### Using the `connect()` function

For us to connect to internal state and to re-render we use **connect()** function provided by **React Redux**

let add this to `./src/App.js`

```jsx
// ./src/App.js
 
import React, { Component } from 'react';
import { connect } from 'react-redux'; /* code change */
import './App.css';
 
class App extends Component {
 
  handleOnClick() {
    this.props.dispatch({
      type: 'INCREASE_COUNT',
    });
  }
 
  render() {
    return (
      <div className="App">
        <button onClick={() => this.handleOnClick()}>
          Click
        </button>
        <p>{this.props.items.length}</p>
      </div>
    );
  }
};
 
// start of code change
const mapStateToProps = (state) => {
  return { items: state.items };
};
 
export default connect(mapStateToProps)(App);//exports a new componet (App) only this time it has provieded data.
// end of code change
```

**Note:** we didn’t import **mapStateToProps** we wrote it

**We have two goals here** 

- (a.) to only re-render **App** when specific changes occur
- (b.) to only provide slice of state the we need Our **App**

**We need** 

1. an event listener for every change in **store**
2. then to filter out any change to a particular component
3. and provide to that component

The above criteria is exactly what this is doing

```jsx
export default connect(mapStateToProps)(App);
```

 The connect function takes care of task **1**  this is synced to our store, listening to each change in the state that occurs. Then if the change does occur  **connect** calls our **mapStateToProps()** method, this method is specifying what slice of state we want. Task **2** we want to provide `state.items`, and allow our component to have access to them through a prop called **items**, so we complete that. Then we say **(App)** is what we want to provide data to. Finally we return a **new** component that **looks just like App**  but this time it receives the correct data.

In **mapStateToProps()** function we are **providing** a new **prop** called **items** so in **App** that is that **prop** we reference

#### A Note on `dispatch`

This looks a little wierd

```jsx
  handleOnClick() {
    this.props.dispatch({
      type: 'INCREASE_COUNT',
    });
  }
```

**we have a dispatch prop?** This prop is **automatically** provided by `connect` if it is missing a second argument. The argument is reserved for `mapDispatchToProps`, which allows us to customize how we send action to the reducer. Even without a second argument we can still use `dispatch` with any component wrapped with `connect`



## **Conclusion** 

We learned of two new pieces of **React Redux** middleware: **connect()** and **Provider**. The two pieces work hand in hand. **Provider** ensures that our entire React application can potentially access data from the store. Then **connect()**, allows us to specify which data we are listening to (through mapStateToProps), and which component we are providing the data. So when you see lines like this:

```jsx
const mapStateToProps = (state) => {
  return { items: state.items };
};
 
connect(mapStateToProps)(App);
```

That is saying connect the data in **mapStateToProps()** (the items portion of the state) to the **App** component. And the **App** component can access that state with `this.props.items`. Don't fret if you still feel hazy on **connect()** and **mapStateToProps()**. This is a new middleware api that takes time to learn. We won't introduce any new material in the next code along, we'll just try to deepen our understanding of the material covered in this section. First, please take at least a 15 minute break before moving on.