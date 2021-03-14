## Deleting A Todo

To delete a Todo we need a button that will trigger an event Listener that will dispatch an action that delets a specific todo.

#### Modifying our TodosContainer

We don’t want to load up our presentational Todo component with any logic, per separation of concern. TodosContainer is where we’re connected to **Redux** so let’s writ in a new `mapToDispatchToProps()` fuction to include an action:

```jsx
// ./src/components/todos/TodosContainer.js
import React, { Component } from 'react';
import { connect } from 'react-redux'
import Todo from './Todo'
 
class TodosContainer extends Component {
 
  renderTodos = () => this.props.todos.map((todo, id) => <Todo key={id} text={todo} />)
 
  render() {
    return(
      <div>
        {this.renderTodos()}
      </div>
    );
  }
};
 
const mapStateToProps = state => {
  return {
    todos: state.todos
  }
}
 
const mapDispatchToProps = dispatch => {
  return {
    delete: todoText => dispatch({type: 'DELETE_TODO', payload: todoText })
  }
}
 
export default connect(mapStateToProps, mapDispatchToProps)(TodosContainer);
```

Now, Todo container will have access to `this.props.delete`, which can take in an argument and send it as the action’s `payload`. we can pass `this.props.delete` down to Todo, so that each Todo component will have access to our `DELETE_TODO` action.

```jsx
renderTodos = () => this.props.todos.map((todo, id) => <Todo delete={this.props.delete} key={id} text={todo} />)
```

#### Modifying the Todo Component

Todo is receiving `this.props.delete`, so let's update the component a little and incorporate a button:

```jsx
import React from 'react'
 
const Todo = props => {
  return (
    <div>
      <span>{props.text}</span><button>DELETE</button>
    </div>
  )
}
 
export default Todo;
```

When we click the button we want to be able to delete this particular todo. At the moment, our todos are just strings, stored in an array. Since that is all we have to work with, we add an onClick attribute to the new button. To keep this component small, we can provide an anonymous function in-line:

```jsx
<div>
  <span>{props.text}</span><button onClick={() => props.delete(props.text)}>DELETE</button>
</div>
```

We're providing a definition for an anonymous function. *Inside* the definition, we're calling `props.delete`, and passing in the only other prop available, `props.text`.

 in our connected TodosContainer, when this delete button is clicked, the value of `props.text` is passed into our dispatched action as the payload.

There is a `console.log` in our reducer that displays actions. Clicking the delete button should log an action with the todo's text content as the payload.

Ok, now we have the ability to dispatch an action to the reducer from each Todo!

## Tell the Store Which Todo to Delete

Our todos are stored as strings in an array. There are a number of ways to remove a specific string from an array, but one of the more brief options is to use `filter`. By adding a second case to our `manageTodo` reducer, we can write a `filter` that returns every todo that *doesn't* match what is contained in `action.payload`:

```jsx
export default function manageTodo(state = {
  todos: [],
}, action) {
  console.log(action);
  switch (action.type) {
    case 'ADD_TODO':
 
      return { todos: state.todos.concat(action.payload.text) };
 
    case 'DELETE_TODO':
 
      return {todos: state.todos.filter(todo => todo !== action.payload)}
 
    default:
      return state;
  }
}
```

In our browser, the delete button should now successfully cause todos to disappear!

There is a problem though. What if you have multiple todos with the same text? With this set up, every todo that matches `action.payload` will be filtered out.

To get around this, instead of filtering just text, it would be better if we gave our Todos specific IDs.

#### Give each Todo an id

A Todo should have an id the moment it gets created. So, we know that our reducer creates the Todo when a CREATE_TODO action is dispatched. Let's update the code in there so that it also adds an id.

```jsx
// ./src/reducers/manageTodo.js
import uuid from 'uuid';
 
export default function manageTodo(state = {
  todos: [],
}, action) {
  console.log(action);
  switch (action.type) {
    case 'ADD_TODO':
 
      const todo = {
        id: uuid(),
        text: action.payload.text
      }
      return { todos: state.todos.concat(todo) };
 
    case 'DELETE_TODO':
 
      return {todos: state.todos.filter(todo => todo !== action.payload)}
 
    default:
      return state;
  }
}
```

sing `uuid()` will generate a long random string each time a todo is created. Now, instead of just storing an array of strings in our store, we'll be storing an array of objects.

This causes a problem 'downstream', though: we need to update our TodosContainer to pass the correct content.

#### Update TodosContainer

In TodosContainer, our `renderTodos` method will need to change a little:

```jsx
renderTodos = () => {
  return this.props.todos.map(todo => <Todo delete={this.props.delete} key={todo.id} todo={todo} />)
}
```

The change is minimal, but this set up is actually better. Previously, `key` was based off the *index* provided by `map`. Now its using our randomly generated ID, and is less prone to errors in the virtual DOM. We'll need both `todo.id` and `todo.text` to be passed into Todo so we pass both down as the object, `todo`.

#### Update the Todo Component

Now that we've got `todo.id`, we can modify the Todo component to use `props.todo.id` on click:

```jsx
import React from 'react'
 
const Todo = props => {
  return (
    <div>
      <span>{props.todo.text}</span><button onClick={() => props.delete(props.todo.id)}>DELETE</button>
    </div>
  )
}
 
export default Todo;
```

Now, when `props.delete` is called, an action is dispatched that contains an *id* only as its payload.

#### Updating `DELETE_TODO` in the Reducer

Now that we're passing an *id* to `props.delete`, we need to modify our reducer once more:

```jsx
case 'DELETE_TODO':
 
  return {todos: state.todos.filter(todo => todo.id !== action.payload)}
```

