## Building Out Counter Application

In the example we defined a `switch` stament with one `case` and a default.

```jsx
function changeState(state, action){
  switch (action.type) {
    case 'INCREASE_COUNT':
      return {count: state.count + 1}
    default:
      return state;
  }
}
 
let state = {count: 0}
let action = {type: 'INCREASE_COUNT'}
 
changeState(state, action)
// => {count: 1}
```

## Persisting State

we currently have a problem. lets call `changeState` multiple times:

```jsx
changeState(state, {type: 'INCREASE_COUNT'})
  // => {count: 1}
changeState(state, {type: 'INCREASE_COUNT'})
  // => {count: 1}
changeState(state, {type: 'INCREASE_COUNT'})
  // => {count: 1}
```

Notice our state never Changed. It starts off as zero, and while the `changeState` does return 1 but, state itself will still returns `{count: 0}`. Now, fixing this in the console isn’t so bad. We just write 

```jsx
state = changeState(state, {type: 'INCREASE_COUNT'})
state
  => {count: 1}
state = changeState(state, {type: 'INCREASE_COUNT'})
  => {count: 2}
```

we are reassigning state to the return value of out reducer. This way, the second time `changeState` is called, it is using the updated state in its arguments.

Ok. So let’s encapsulate this procedure in a function so that we can jsust call that methosd and it will persist our changes. we’ll name that function `dispatch`.

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
  return state
}
 
dispatch({type: 'INCREASE_COUNT'})
  // => {count: 1}
dispatch({type: 'INCREASE_COUNT'})
  // => {count: 2}
dispatch({type: 'INCREASE_COUNT'})
  // => {count: 3}
```

Ok this fixes it. we define  `state` equal to an object the is as a key `count: 0`. `dispatch(action)` will pass in the action and object to `changeState` state method which will be then set equal to the method itself so that state becomes the return value of the method.

## Rendering Our State

If we **forget about REACT** for just a moment, this is how we’d update the HTML  on the page to make sure it updates when we update state.

```jsx
function render(){
  document.body.textContent = state.count
}

//call render()
=> 1
```

Though as the code is now we’d have to call dispatch in order to change the state on the page. we can fix that by doing this

```react
function render(){
document.body.textContent = state.count
}

function dispatch(action){
    state = changeState(state, action)
    render()
}

dispatch({type: 'INCREASE_COUNT'})
dispatch({type: 'INCREASE_COUNT'})
```

Just to show everything together finally:

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
 
function render(){
    document.body.textContent = state.count
}
 
function dispatch(action){
  state = changeState(state, action)
  render()
}
 
render()
```

