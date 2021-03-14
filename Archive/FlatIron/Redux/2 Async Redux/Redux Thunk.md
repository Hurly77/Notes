## Redux: Thunk

**Thunk**  handles asynchronous call when working with Redux. **With Thunk** we ca incorporate asynchronous code in with our **Redux** actions.

## Trying to Send an Asynchronous Request in Redux

In an example of a hard code asynchronous call  we would write

```jsx
function fetchAstronauts() {
  const astronauts = [
    {name: "Neil Armstrong", craft: "Apollo 11"},
    {name: "Buzz Aldrin", craft: "Apollo 11"},
    {name: "Michael Collins", craft: "Apollo 11"}
  ];
  return {
    type: 'ADD_ASTRONAUTS',
    astronauts
  };
};
```

We would send our native Fetch API we request as such

```jsx
fetch('http://api.open-notify.org/astros.json')
```

We can’t just simply write our fetch request in our action creator function. for example :

```jsx
// ./src/App.js
 
import React, { Component } from 'react'
import { connect } from 'react-redux'
import { fetchAstronauts } from '../actions/fetchAstronauts'
 
class App extends Component {
 
  handleOnClick() {
    this.props.fetchAstronauts()
  }
 
  render() {
    const astronauts = this.props.astronauts.map(astro => <li key={astro.id}>{astro.name}</li>);
 
    return(
      <div>
        <button onClick={(event) => this.handleOnClick(event)} />
        {astronauts}
      </div>
    );
  }
};
 
function mapDispatchToProps(dispatch){
  return { fetchAstronauts: () => dispatch(fetchAstronauts()) }
}
 
function mapStateToProps(state){
  return { astronauts: state.astronauts }
}
 
export default connect(mapStateToProps, mapDispatchToProps)(App)
 
// ./src/actions/fetchAstronauts.js
export function fetchAstronauts() {
  const astronauts = fetch('http://api.open-notify.org/astros.json');
  return {
    type: 'ADD_ASTRONAUTS',
    astronauts
  };
};
 
// ./src/astronautsReducer.js
function astronautsReducer(state = { astronauts: [] }, action) {
  switch (action.type) {
 
    case 'ADD_ASTRONAUTS':
      return { ...state, astronauts: action.astronauts }
 
    default:
      return state;
  }
};
```

When a user clicks on the button, we call the `handleOnClick()` function. This calls our action creator, the `fetchAstronauts()` function. The action creator then hits the API, and returns an action with our data, which then updates the state through the reducer.

But, Since our fetch requests are asynchronous, at the first line of our `fetchAstronauts` function:

```jsx
export function fetchAstronauts() {
  const astronauts = fetch('http://api.open-notify.org/astros.json');
  return {
    type: 'ADD_ASTRONAUTS',
    astronauts
  };
};
```

 The code on the second line will start running ***before** the web request **resolves** and we have a response that we **can’t** work with*.

The `fetch()` request returns a **Promise**. A Promise object represents some value, that we can access was the object ‘promise’ is resolved. using `.then()` function onto our `fetch()` call we can access that data.

```jsx
export function fetchAstronauts() {
  const astronauts = fetch('http://api.open-notify.org/astros.json')
                      .then(response => response.json())
  return {
    type: 'ADD_ASTRONAUTS',
    astronauts
  };
}
```

This won’t solve our problem though because `fetchAstronaut` will still **return before our Promise** is resolved.

**Also**, we want to represent the state of the **app** in between th use asking for data and the app receiving the data. **its almost like:** - we want to **dispatch** two actions to **update** our **state**: one to place our **state as loading**, and another to **update the state** with the data.

So **these are the steps** we want to happen when the user wishes to call the API:

1. Invoke `fetchAstronauts()`
2. Directly after invoking `fetchAstronauts()` dispatch an action to indicate that we are loading data.
3. Call the `fetch()` method, which runs, and returns a Promise that we are waiting to resolve.
4. When the Promise resolves, dispatch another action with a payload of the fetched data that gets sent to the reducer.

## We Need Middleware

**Middleware,** in this case, will allow us to slightly alter the behavior of our actions, allowing us to add in asynchronous requests. In this case, for middleware, we'll be using Thunk.

To use **Redux Thunk** you would need to install the NPM package:

```jsx
npm install --save redux-thunk
```

Then, when you initialize the store in your `index.js` file, you can incorporate your middleware like this:

```jsx
// src/index.js
 
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import { createStore, applyMiddleware } from 'redux'; //<=== HERE,
import thunk from 'redux-thunk'; // <==== HERE
import rootReducer from './reducers';
 
const store = createStore(rootReducer, applyMiddleware(thunk)); // <=== 'AND' HERE
 
ReactDOM.render(
  <Provider store={store} >
    <App />
  </Provider>, document.getElementById('container')
)
```

## Using Redux-Thunk Middleware

**Thunk** **middleware** will do a couple of interesting things:

1. Thunk **allows** us to return **a function inside of our action creator.** Normally, our action creator returns a plain JavaScript object, so returning a function is a pretty big change.
2. That **function** **receives** the **store's dispatch function as its argument**. With that, we can dispatch multiple actions from inside that returned function.

Let's see the code and then we'll walk through it.

```jsx
// actions/fetchAstronauts.js
export function fetchAstronauts() {
  return (dispatch) => {// use dispatch as argument/ callback
    dispatch({ type: 'START_ADDING_ASTRONAUTS_REQUEST' }); //pass in action Type:"START_ADDING_ASTRONAUTS_REQUEST" To dipatch function
    fetch('http://api.open-notify.org/astros.json')//then write our fectch request
      .then(response => response.json())
      .then(astronauts => dispatch({ type: 'ADD_ASTRONAUTS', astronauts }));
  };
}
```

### Reviewing Everything Together

Let's review the whole application now with Redux and Thunk configured.

`index.js` imports `thunk` and `applyMiddleware` 

```jsx
// ./src/index.js
 
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import rootReducer from './reducers';
 
const store = createStore(rootReducer, applyMiddleware(thunk)); //<=== Uses Thunk and applyMiddleware when creating Redux Store
 
ReactDOM.render(
  <Provider store={store} >
    <App />
  </Provider>, document.getElementById('container')
)
```

`App.js`

remains the same

although we've called a function `fetchAstronauts()`, no actual asynchronous code is in the component. The component's main purpose is to render JSX. It uses data from Redux via `mapStateToProps()` and connects an `onClick` event to an action through `mapDispatchToProps()`:

```jsx
// ./src/App.js
 
import React, { Component } from 'react'
import { connect } from 'react-redux'
import { fetchAstronauts } from '../actions/fetchAstronauts'
 
class App extends Component {
 
  handleOnClick() {
    this.props.fetchAstronauts()//<==={THIS: Is not asynchrounous code, App only renders JSX}
  }
 
  render() {
    const astronauts = this.props.astronauts.map(astro => <li key={astro.id}>{astro.name}</li>);
 
    return(
      <div>
        <button onClick={(event) => this.handleOnClick(event)} />
        {astronauts}
      </div>
    );
  }
};
 
function mapDispatchToProps(dispatch){
  return { fetchAstronauts: () => dispatch(fetchAstronauts()) }
}
 
function mapStateToProps(state){
  return {astronauts: state.astronauts}
}
 
export default connect(mapStateToProps, mapDispatchToProps)(App)
```

when the `onClick` event is fired

logic is taken care of outside of the component, in our `fetchAstronauts()` action:

```jsx
// actions/fetchAstronauts.js
export function fetchAstronauts() {
  return (dispatch) => {//<=== passes callbak 'dispatch' argument
    dispatch({ type: 'START_ADDING_ASTRONAUTS_REQUEST' });//? we call Dispatch twice [ONCE]
      //then passes in an objecty whose key is type and value is the action we want to fire
    fetch('http://api.open-notify.org/astros.json')
      .then(response => response.json())
      .then(astronauts => dispatch({ type: 'ADD_ASTRONAUTS', astronauts }));//[TWICE]
  };
}
```

By having both `dispatch()` calls, it is possible to know just **before** our application sends a **remote request**, and **then** immediately **after** that request is **resolved**.

We can update our reducer to include both `type`s and to also change a bit of state to indicate if data is in the process of being fetched. We'll modify the initial state to do this:

```jsx
// ./src/astronautsReducer.js
function astronautsReducer(state = { astronauts: [], requesting: false }, action) {//Modified intial State
  switch (action.type) {
 
    case 'START_ADDING_ASTRONAUTS_REQUEST':
      return {
        ...state,
        astronauts: [...state.astronauts],
        requesting: true
      }
 
    case 'ADD_ASTRONAUTS':
      return {
        ...state,
        astronauts: action.astronauts,
        requesting: false
      }
 
    default:
      return state;
  }
};
```

Now, we have a way to indicate in our app when data is being loaded! If `requesting` is true, we could display a loading message in JSX!