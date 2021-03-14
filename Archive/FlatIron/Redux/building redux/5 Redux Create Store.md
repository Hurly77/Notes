## Encapsulate our application's state by wrapping our code in a function

**The code so far….**

```js
let state;
 
function reducer(state = { count: 0 }, action) {
  switch (action.type) {
    case 'INCREASE_COUNT':
      return { count: state.count + 1 };
 
    default:
      return state;
  }
};
 
function dispatch(action){
  state = reducer(state, action);
  render();
};
 
function render() {
  let container = document.getElementById('container');
  container.textContent = state.count;
};
 
dispatch({ type: '@@INIT' })
let button = document.getElementById('button');
 
button.addEventListener('click', () => {
    dispatch({ type: 'INCREASE_COUNT' });
})
```

As our code is now we have out state in side the global scope, this will leave our code very susceptible to being overwritten. 

if we just wrap our state in a function we can create closure so nothing can overwrite that method.

```jsx
function createStore() {
  let state;
}
// ...
 
function dispatch(action) {
  state = reducer(state, action);
  render();
};
 
function render() {
  let container = document.getElementById('container');
  container.textContent = state.count;
};
```

  However this does give us an error which points to where we dispatched the initial action; this is because dispatch nor render() has access to our `state`.

## Move Code Common to Every JavaScript Application Inside Our New Function

we want  our applications following the Redux Patten to be able to use our new function.

`action -> reducer -> New State`

Our `Dispatch` function already goes through this flow or pattern. so we can move it inside of our new method

```jsx
function createStore() {
  let state;
  // state is now accessible to dispatch
 
  function dispatch(action) {
    state = reducer(state, action);
    render();
  }
}
```

> **NOTE**:  above we use closure. closure allows us to access to all variables inside that scope at definition. So the function will carry those variables with it when invoked

`dispatch` is now private to our new function. By **returning** a JS **object** **containing** `dispatch` method we can grant access to state, when the function is called. In **Redux** this is called **store** hence the name `createStore()` 

```jsx
function createStore() {
  let state;
 
  function dispatch(action) {
    state = reducer(state, action);
    render();
  };
 
  return { dispatch };
};
```

We can get access to dispatch method by creating a variable equal to the result of calling `createStore`

```jsx
let store = createStore();
store.dispatch({type: '@@INIT'})
```

The store contains all the info about state, but we need to be able to access that data. If we **create** a **method** that **returns state** and call that in our return value **we can use state in other parts of our application.**

```jsx
function createStore() {
  let state;
 
  function dispatch(action) {
    state = reducer(state, action);
    render();
  }
 
  function getState() {
    return state;
  }
 
  return {
    dispatch,
    getState
  };
};
```

to make code work.

```jsx
function render() {
  let container = document.getElementById('container');
  container.textContent = store.getState().count;
};
```

...and then updating our button event listener to use `store.dispatch`:

```jsx
let button = document.getElementById('button');
 
button.addEventListener('click', () => {
    store.dispatch({ type: 'INCREASE_COUNT' });
})
```

`createStore` can work with any JavaScript application...almost.

## Abstract away the reducer

Our dispatch method does follow Redux flow, but our `createStore` method just isn’t generic enough to be used with any JS application. we  want to make reducer an argument to our `createStore` function then pass through our reducer function when invoking the `createStore` method.

```jsx
function createStore(reducer) {// create an argument so we can pass in reducer
  let state;
 
  function dispatch(action) {
    state = reducer(state, action);
    render();
  }
 
  function getState() {
    return state;
  };
 
  return {
    dispatch,
    getState
  };
};
 
function reducer(state = { count: 0 }, action) {
  switch (action.type) {
    case 'INCREASE_COUNT':
      return { count: state.count + 1 };
 
    default:
      return state;
  }
}
 
 
function render() {
  let container = document.getElementById('container');
  container.textContent = store.getState().count;
};
 
let store = createStore(reducer) // createStore takes the reducer as an argument
store.dispatch({ type: '@@INIT' });
let button = document.getElementById('button');
 
button.addEventListener('click', () => {
  store.dispatch({ type: 'INCREASE_COUNT' });
});
```

