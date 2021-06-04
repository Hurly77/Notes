## Let's take a look at an example:

```jsx
let state = {count: 0}
let action = {type: 'INCREASE_COUNT'}
```

Somehow I want to send this action to the state so that at the end our state is updated to look like the following: `state -> {count: 1}`.

## Functions to the Rescue

This seems easy enough. Why not just write a function that takes in our previous state, takes in our action, and depending on that action produces a new state. Here's what it could look like:

```jsx
function changeState(state, action) {
  if (action.type === 'INCREASE_COUNT') {
    return {count: state.count + 1 }
  }
}
```

Actions always need a `type` property so the function knows what to do. If you can imagine a whole bunch of different actions that change the state in different ways, `'DECREASE_COUNT'`, `'INCREASE_COUNT_BY_TEN'` and so on, it shouldn't be hard to see how that code could become very messy with a bunch of `if`s and `else if`s. Instead, it is customary to use a `switch case` statement.

```jsx
function changeState(state, action){
  switch (action.type) {
    case 'INCREASE_COUNT':
      return {count: state.count + 1}
  }
}
```

This makes it very explicit and clear that `action.type` is the information we are switching on to make our decision on how to change the state.

**important** that when we change the state we never return `null` or `undefined`. We'll cover this by adding a `default` case to our function.

```jsx
function changeState(state, action){
  switch (action.type) {
    case 'INCREASE_COUNT':
      return {count: state.count + 1}
    default:
      return state;
  }
}
```

This way, no matter what, when accessing the Redux state we'll always get some form of the state back.

```jsx
let state = {count: 0}
let action = {type: 'INCREASE_COUNT'}
 
changeState(state, action)
// => {count: 1}
```

Now let's have this function respond to another action, decrease count. Give it a shot, the answer is below.

```jsx
function changeState(state, action){      
  switch (action.type) {
    case 'INCREASE_COUNT':
      return {count: state.count + 1}
    case 'DECREASE_COUNT':
      return {count: state.count - 1}
    default:
      return state;
  }
}
 
let state = {count: 0}
 
changeState(state, {type: 'INCREASE_COUNT'})
// => {count: 1}
 
changeState(state, {type: 'DECREASE_COUNT'})
// => {count: -1}
```

Ok! That my friends, is the crux of redux. To summarize:

```jsx
Action -> Function -> Updated State
```

And let's give this function a name. Because it is combining two pieces of information, our current state and an action, reducing this combination into one value, we'll say that it *reduces* the two into one updated state. For this reason, we call this function a reducer:

```
Action -> Reducer -> Updated State
```

As you learn more about redux, things may become more complex. Just remember that at the core of redux is always this flow. An action gets sent to a reducer which then updates the state of the application.

# TWIST

You may notice a problem. While we can call the changeState reducer to increase the count from zero to one, if we call change state again we keep returning a count of one. In other words, we are not persisting this change of state. We'll tackle how this works in an upcoming section.

## Reducers are pure functions

```jsx
function reducer(state, action){      
  switch (action.type) {
    case 'INCREASE_COUNT':
      return {count: state.count + 1}
    case 'DECREASE_COUNT':
      return {count: state.count - 1}
    default:
      return state;
  }
}
```

characteristics of pure functions:

1. Pure functions are only determined by their input values
2. Pure Functions have no side effects. By this we mean pure functions do not have any effect outside of the function. They only return a value.

> **Note**: The reason we like pure functions so much is because if a function has no effect outside of the object, and if the function always returns the same value given a specific input, this means that our functions become really predictable. In addition, the lack of side effects means that the functions are also contained, and can be used safely without affecting the rest of your application.

Ok, so the first characteristic of pure functions means that given the same input of the function, I will always receive the same output from that function. That seems to hold, given a specific state object like `{count: 2}` and an action object like `{type: 'DECREASE_COUNT'}` will I always get back the same value? Yes. Given those two arguments, the output will always be `{count: 1}`.

As for the 'no side effects' characteristic, there's something pretty subtle going on in our reducer. The object returned is not the same object that is passed as an argument to the function, but rather a new object that is constructed each time our reducer is called. Do you see why? Take a close look at the line that says `return {count: state.count + 1}`. This line is constructing a new JavaScript object and setting its count attribute to equal the previous state's count plus one. So we adhere to the constraints of a pure function by not changing any value that is defined outside of the function.

## Summary

1. We hold our application's state in one plain old JavaScript object, and we update that state by passing both an action and the old state to our reducer. Our reducer returns to us our new state.
2. So to change our state we (1) create an action (an **action** is just a plain object with a type key); and (2) and pass the action as an argument when we call the **reducer** (which is just a function with a switch/case statement). This produces a new state.
3. Our reducer is a pure function which means that given the same arguments of state and action, it will always produce the same new state. Also it means that our reducer never updates the previous state, but rather creates a new state object.