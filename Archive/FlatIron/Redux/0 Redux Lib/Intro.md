Our **createStore()** method will from now on be imported form the **Redux** official library. 

To install Redux we need to run `npm install redux && npm install react-redux` 

We will be building a simple shopping list.

### Step 1: Setting Up The Store

**First** we pass down our store to our top-level container component using Redux initialize.

The **Redux-(es)** `createStore()` method, returns an **instance** of the Redux **store**. we want to import that to our `src/index.js`.

```jsx
// ./src/index.js
 
import React from 'react';
import ReactDOM from 'react-dom';
import { createStore } from 'redux'; /* code change */
import shoppingListItemReducer from './reducers/shoppingListItemReducer.js';
import App from './App';
import './index.css';
 
const store = createStore(shoppingListItemReducer); /* code change */
 
ReactDOM.render(<App />, document.getElementById('root'));
```

Now we can Pass down **Redux** store anywhere in our app that would need it.

Though Redux was made so we wouldn’t have to pass down props all the dang time. we can import `Provider` **component** from `react-redux`. This **component** will **wraps** the top-level component, **App**. This is the only place where store is passed in:

```jsx
// ./src/index.js
 
import React from 'react';
import ReactDOM from 'react-dom';
import { createStore } from 'redux';
import { Provider } from 'react-redux'; /* code change */
import shoppingListItemReducer from './reducers/shoppingListItemReducer.js';
import App from './App';
import './index.css';
 
const store = createStore(shoppingListItemReducer);
 
// code change - added Provider to wrap around App
ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider> /* code change */,
  document.getElementById('root')
);
```

**By including** the `Provider`, we'll be able to access our **Redux** store and/or dispatch actions from any component we want, regardless of where it is on the component tree.



Our Reducer is in `./src/reducers/shoppinListItemReducer.js`

```jsx
// ./src/reducers/shoppingListItemReducer.js
 
export default function shoppingListItemReducer(
  state = {
    items: []
  },
  action
) {
  switch (action.type) {
    case 'INCREASE_COUNT':
      return {
        ...state,
        items: state.items.concat(state.items.length + 1)
      }
 
    default:
      return state;
  }
}
```

Our reducer is only producing a counter by adding new item to the list each time it is called.

This Time instead of encapsulating in a closure we created a reducer **pure component.** Which is then imported to `src/index.js` along with our `store` method, and `store` is then passed in as a prop to `Provider` component

There is a second function `connect` provided by `react-redux` library, this will allow us to access `store`. if we modify a component export statement including `connect` function we can take data from our **Redux** `store` and map them to a component’s props. Also we can take actions, an wrap them in a dispatch and an anonymous function, then pass them as props as well:

```jsx
// ./src/App.js
 
import React, { Component } from 'react';
import { connect } from 'react-redux';
import './App.css';
 
class App extends Component {
  handleOnClick = event => {
    this.props.increaseCount();
  };
 
  render() {//adding button to page
    return (
      <div className="App">
        <button onClick={this.handleOnClick}>Click</button>{/*invokes eventListner which will call this.props.increaseCount. but increaseCount is acctually being provided by the new function below our App coponent "mapDispatchToProps"*/}
        <p>{this.props.items.length}</p>{/*also a prop created from Redux store; as the store's items propety increases, App will display a different number!*/}
      </div>
    );
  }
}
 
const mapStateToProps = state => {
  return {
    items: state.items
  };
};
 
const mapDispatchToProps = dispatch => {
  return {
    increaseCount: () => dispatch({ type: 'INCREASE_COUNT' })
  };
};
 //modify export stament to include connect, this will allow us to map store data to components props
export default connect(
  mapStateToProps,
  mapDispatchToProps
)(App);
```

Ok, so this code places a button on the page with an `onClick` event listener pointed to `this.handleOnClick`. When `this.handleOnClick` is invoked, it calls a function, `this.props.increaseCount`. Well.. `increaseCount` is actually being provided by the new function below our App component: `mapDispatchToProps`.

Meanwhile, we've also got `this.props.items.length`, which is *also* a prop created from our **Redux** store. As the store's `items` property increases, App will display a different number!

If you boot up the app, you should see a button on the page, followed by a zero, using the core above for `index.js` and `App.js`, we can see **Redux** in action. Every button click dispatches an action to our store, causing it to change. Since data (`items`) from that store is being accessed in App, App will re-render and display the updated counter.

#### Add Logging to Our Reducer

Re-rendering our application is no easy task. lets log our action and the new state. change reducer to the following.

```jsx
// ./src/reducers/shoppingListItemReducer
 
export default function shoppingListItemReducer(
  state = {
    items: []
  },
  action
) {
  console.log(action);
  switch (action.type) {
    case 'INCREASE_COUNT':
      console.log('Current state.items length %s', state.items.length);
      console.log('Updating state.items length to %s', state.items.length + 1);
      return {
        ...state,
        items: state.items.concat(state.items.length + 1)
      };
 
    default:
      console.log('Initial state.items length: %s', state.items.length);
      return state;
  }
}
```

**Important Read to Know on Logging**

Ok, so this may look like a lot, but really all were doing is adding some logging behavior. At the top of the function, we are logging the action. After the case statement, we are storing our state as current state first. Then we are logging the updating state value. Then under the default case statement, we just can log the previous state because this state is unchanged.

You should see the correct action being dispatched, as well as an update to the state. While we aren't getting our state directly from the store, we know that we are dispatching actions. We know this because each time we click a button, we call store.dispatch({ type: 'INCREASE_COUNT' }) and somehow this is hitting our reducer. So things are happening.

#### Redux DevTools

**After Downloading Redux dev Tools** we need to tell our application to communicate with this extension. Doing so is pretty easy. Now we change the arguments to our createStore method to the following:

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import { createStore } from 'redux';
import { Provider } from 'react-redux';
import shoppingListItemReducer from './reducers/shoppingListItemReducer';
import App from './App';
import './index.css';
 
const store = createStore(
  shoppingListItemReducer,
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
); /* code change */
 
ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

Ok, notice that we are still passing through our reducer to the createStore method. The second argument is accessing our browser to find a method called `__REDUX_DEVTOOLS_EXTENSION__`. If that method is there, the method is executed. Now if you have your Chrome console opened, make sure the Redux Devtools Inspector is open (press command+shift+c, click on the arrows at the top right, and the dropdown for the extension). Now click on the tab that says state. You should see `{ items: [] }`. If you do, it means that your app is now communicating with the devtool. Click on the button in your application, to see if the state changes. Now for each time you click on it, you should see an action in the devtools that has the name of that action. If you are looking at the last state, you should see the changes in our state.

Whew!