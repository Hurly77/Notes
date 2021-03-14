## 0Dispatch an Initial Action to Render the View

Previously on Code Along we built:

```jsx
let state = {count: 0};
 
function changeState(state, action){
  switch (action.type) {
    case 'INCREASE_COUNT':
      return {count: state.count + 1}
    default:
      return state;
  }
}
 
function dispatch(action){
    state = changeState(state, action)
    render()
}
 
function render(){
    document.body.textContent = state.count
}
```

**As our code is now** we never see the initial value of state displayed on the screen. An easy fix is to just all the Dispatch function with a meaningless action, this will cause `changeState` method to revert to out default `switch` statement

```jsx
dispatch({ type: '@@INIT' })// this is meaningless 
// type could have been anything you wanted as long as it does'nt trigger out case: statement.
```

## Dispatch an initial Action to Set up out Initial State

```js
let state = {count: 0}
```

Preferably we’d want our **reducer** to mange the state. We can start to fix this by simply calling state and not setting it to equal anything

```jsx
let state;
```

```jsx
function chageState(state, action){

    switch (action.type){
		
        case 'INCREASE_COUNT':
            return {count: state.count + 1}
        default:
            return state;
    }
}

function dispatch(action){
    state = changeState(state, action)
    render()
}

function render(){
    document.body.textContent = state.count
}

dispatch({type: '@@INIT'})
```

But, we find that dispatching the action of type `@@INIT` gives us an error

```jsx
Uncaught TypeError: Cannot read property 'count' of undefined(…)
```

Our `render()` function is breaking because now that `state` is not set to anything it returns `undfinded`.

Thank you **ES6** because we can easily fix this by by giving our state agrument in `changeStament` function a default value.

```jsx
function changeState(state = { count: 0 }, action) {
 
    switch (action.type) {
 
      case 'INCREASE_COUNT':
        return { count: state.count + 1 }
 
      default:
        return state;
    }
  }
```

Now:

```jsx
  dispatch({ type: '@@INIT' })
    -> { count: 0 }
  dispatch({type: 'INCREASE_COUNT'})
    -> { count: 1 }
```

BADA BANG BADA BOOM BABY!! 