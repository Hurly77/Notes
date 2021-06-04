## Displaying todos

The `CreateTodo` component is handling the creation side of things, so lets make a new component where we’ll be getting todos from the store. we’ll call this `todosContainer` and connect it to **Redux**.

```jsx
// ./src/components/todos/TodosContainer.js
 
import React, { Component } from 'react';
import { connect } from 'react-redux'
 
class TodosContainer extends Component {
 
  render() {
    return(
      <div></div>
    );
  }
};
 
export default connect()(TodosContainer);
```

Now, we aren't worried about dispatching actions here, only getting state from **Redux**, so we'll need to write out a `mapStateToProps()` function and include it as an argument for `connect()`:

```jsx
...
const mapStateToProps = state => {
  return {
    todos: state.todos
  }
}
 
export default connect(mapStateToProps)(TodosContainer);
```

## Creating a Presentational Todo Component

rendered as a list item. Inside the `./src/components/`

```jsx
import React from 'react'
 
const Todo = props => {
  return (
    <li>{props.text}</li>
  );
};
 
export default Todo;
```

Now we need to call that component from a map function in the **TodosContainer** component:

```jsx
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
 
export default connect(mapStateToProps)(TodosContainer);
```

Now our TodosContainer is mapping over the todos it received from **Redux**, passing the value of each todo into a child component, Todo. Todo in this case doesn't have any **Redux** related code, and is a regular, functional component.

## Cleanup Todo Input

Each time we submit a todo, we want to clear out the input. Ok, so remember that each time we submit a form, we call **handleSubmit**. Inside that **handleSubmit** function let's reset the *component's* state by changing our function to the following:

```jsx
// change this 
handleSubmit = event => {//old code
    event.preventDefault();
    this.props.addTodo(this.state)
  }
  
//To this
handleSubmit = event => {// new code
  event.preventDefault();
  this.props.addTodo(this.state)
  this.setState({
    text: '',
  })
}
```

That's it! We've got a working app that takes in form data and displays it on a list.