#### 1. Start by remembering our core fact about how redux works.

```
action -> reducer -> new state
```

Ok, let's translate that into code. This means if we pass an action and a previous state to our reducer, the reducer should return the new state.

```jsx
let state;
 
function reducer(state = {count: 0}, action){
  switch (action.type) {
    case 'INCREASE_COUNT':
      return {count: state.count + 1}
    default:
      return state;
  }
}
 
function dispatch(action){
  state = reducer(state, action)
  render()
}
 
function render(){
  let container = document.getElementById('container');
  container.textContent = state.count;
}
 
dispatch({type: '@@INIT'})
```

#### 5. Integrating dispatch with a user event

we’ll write a vanilla JS event Listener 

```jsx
let button = document.getElementById('button');
 
button.addEventListener('click', () => {
  dispatch({type: 'INCREASE_COUNT'})
})
```

