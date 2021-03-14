# Structuring Actions

With action the reducer uses the type property to see what it should do. ex:

```js
const increaseCount = { type: 'INCREASE_COUNT' }
```

The store has a dispatch method which we can now use to dispatch this action to be handled by the reducer.

```js
store.dispatch(increaseCount)
```

The dispatch method passes the action to the reducer, which runs a switch statement :

```js
 
function dispatch(action) {
  reducer(state, action)
}
 
function reducer(state = {
  count: 0,
}, action) {
  switch (action.type) {
 
    case 'INCREASE_COUNT':
      return { count: state.count + 1 };
 
    default:
      return state;
 
  }
}
```

# Action Creators

Ok, using our action looks like `store.dispatch({type: 'INCREASE_COUNT'})`, but what if we did this:

```js
function increaseCount() {
  return { type: 'INCREASE_COUNT' };
}
 
store.dispatch(increaseCount());
```

We define a method called `increaseCount()` which returns an action. Then we pass the method to `store.dispatch()` as an argument.

we put our action in a function because over time actions will need to have parts changed. Ex:

```js
function addTodo(todo) {
  return {
    type: 'ADD_TODO',
    todo: todo
  }
}
```

Imagine if actions have different payload properties, depending on what we pass to the `addTodo` function

```js
addTodo('buy groceries');
// -> { type: 'ADD_TODO', todo: 'buy groceries' }
 
addTodo('watch netflix');
// -> { type: 'ADD_TODO', todo: 'watch netflix' }
```

By wrapping our action in a function where able to keep some properties the same while changing others

```js
store.dispatch(addTodo('buy groceries'));
```

That would return the action `{ type: 'ADD_TODO', todo: 'buy groceries' }`, which we then send to the dispatch function.