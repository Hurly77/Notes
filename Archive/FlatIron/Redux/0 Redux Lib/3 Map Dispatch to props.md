## Identifying the Problem

We want to separate concerns. This means we want our to mange **State** in one part of our code and logic in a different part of our code.

Though we did solve this problem with our `CreateStore` by allowing all components to access `store` and using `mapToStateProps` and exporting with `connect()` method from **React-Redux**, we still haven’t done this with our dispatch method.

```jsx
// ./src/app.js
...
 
handleOnClick(){
  this.props.store.dispatch(addItem())
}
 
...
```

This line of code make our component reliant on **Redux**.

We can fix this problem, the same way we fixed or `mapToStateProps` by using the `connet` method. we can pass `connect` a second argument *action creator* as props. Then reference this action creator as a prop to call it from our component.

#### Using `mapDispatchToProps`

We use **Redux store’s state** as a first argument to our `connet()` function and returns and object, which was created using all or some of that state. The key/values return by the object will become accessible with the component we passed in to `connect()` function. 

```jsx
const mapStateToProps = state => {
  return state//this would make entire state availbe as a prop to the passed in component.
}
```

This is `mapStateToProps` The function is passed is passed into `connect` as first argument and passing the current state to the function. So for our `mapDispatchToProps` function will be passed in as a secound argument only this time it will be passed **dispatch()** *function* as and argument

`./src/App.js` will look like so:

```js
// src/App.js
 
import React, { Component } from 'react';
import './App.css';
import { connect } from 'react-redux';
import { addItem } from  './actions/items';
 
class App extends Component {
 
  handleOnClick = event => {
    this.props.addItem() // Code change: this.props.store.dispatch is no longer being called
  }
 
  render() {
    return (
      <div className="App">
        <button onClick={this.handleOnClick}>
          Click
          </button>
        <p>{this.props.items.length}</p>
      </div>
    );
  }
};
 
const mapStateToProps = (state) => {
  return {
    items: state.items
  };
};
 
// Code change: this new function takes in dispatch as an argument
// It then returns an object that contains a function as a value!
// Notice above in handleOnClick() that this function, addItem(),
// is what is called, NOT the addItem action creator itself.
const mapDispatchToProps = dispatch => {
  return {
    addItem: () => {
      dispatch(addItem())
    }
  };
};
 
export default connect(mapStateToProps, mapDispatchToProps)(App);
```

We can place a debugger here and see what happens

```jsx
// src/App.js
...
render() {
    debugger
    return (
      <div className="App">
        <button onClick={this.handleOnClick}>
          Click
          </button>
        <p>{this.props.items.length}</p>
      </div>
    );
  }
...
```

If we boot up the console and type `this.props.addItem` a function will be return, because our `mapDispatchToProps` has a key `addItems` whose value points to a function.

So now when `handOnClick()` function is called our action create `this.props.addItem()` is executed because we referenced it as props.

```jsx
// ./src/App.js
 
...
 
handleOnClick = event => {
  this.props.addItem()
}
 
...
```

## Alternative Method

We can simplify this even more, by **passing** in an **object** instead of a function **to** `connent` function, our `connect` function will **handle** the **dispatch** step for us. The object would look like so

```json
{
  addItem: addItem
}

//with ES6 if we have a key/value with the same name we can use short hand and just write it like this
{
    addItem
}
```

This is all we need to pass in as a second argument for `connet()`.

`App`

```jsx
import React, { Component } from 'react';
import './App.css';
import { connect } from 'react-redux';
import { addItem } from  './actions/items';
 
class App extends Component {
 
  handleOnClick = event => {
    this.props.addItem()
  }
 
  render() {
    debugger
    return (
      <div className="App">
        <button onClick={this.handleOnClick}>
          Click
          </button>
        <p>{this.props.items.length}</p>
      </div>
    );
  }
};
 
const mapStateToProps = (state) => {
  return {
    items: state.items
  };
};
 
export default connect(mapStateToProps, { addItem })(App); // Code change: no mapDispatchToProps function required!
```

> **Aside:** We *could* do the same with `mapStateProps` but the first argument must be a function but it can be an anonymous arrow function

```jsx
export default connect(state => ({ items: state.items }), { addItem })(App);
```

**This is the same as** 

```jsx
const mapStateToProps = state => {
  return {
    items: state.items
  }
}
 
const mapDispatchToProps = dispatch => {
  return {
    addItem: () => { dispatch(addItem()) }
  }
}
 
export default connect(mapStateToProps, mapDispatchToProps)(App);
```

## Default Dispatch Behavior

in addition to this, as per Dan Abramov, **the creator of Redux**:

> By default `mapDispatchToProps` is just `dispatch => ({dispatch})`. so if you don’t specify the second argument to `connect()`, you’ll get dispatch injected as a prop in your component.

this means that if we were to simply write

```jsx
export default connect(state => ({ items: state.items }))(App);
```

...we would *still* have `this.props.dispatch()` available to us in App. If you would rather write `this.props.dispatch({ type: 'INCREASE_COUNT' })` in App, or pass `dispatch` down to children, you can!