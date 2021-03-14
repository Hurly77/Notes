## Create The Form in React

`./src/App.js` file we reference a createTodo form located at

`./src/components/todos/createTodo.js`. That's where we need to build our form.

So in that file we want to change our component to look like the following:

```jsx
// ./src/components/todos/CreateTodo.js
 
import React, { Component } from 'react'
 
class CreateTodo extends Component {
  render() {
    return(
      <div>
        <form>
          <p>
            <label>add todo</label>
            <input type="text" />
          </p>
          <input type="submit" />
        </form>
      </div>
    );
  }
};
 
export default CreateTodo;
```

## Plan for Integrating into Redux

Now let's think about how we want to integrate this into **Redux**. Essentially, upon submitting the form, we would like to dispatch the following action to the store:

```jsx
{
  type: 'ADD_TODO',
  todo: todo
}
```

So if the user has typed in buy groceries, our action would look like:

```jsx
{
  type: 'ADD_TODO',
  todo: 'buy groceries'
}
```

**Step one** will be updating the component state whenever someone types in the form.

### 1. Create a Controlled Form Using State and an `onChange` Event Handler

```jsx
// ./src/components/todos/CreateTodo.js
 
import React, { Component } from 'react';
 
class CreateTodo extends Component {
 
  constructor() {
    super();
    this.state = {
      text: '',
    };
  }
 
  handleChange = event => {
    this.setState({
      text: event.target.value
    });
  }
 
  render() {
    return(
      <div>
        <form>
          <p>
            <label>add todo</label>
            <input
          type="text"
          onChange={this.handleChange} value={this.state.text}/>
          </p>
          <input type="submit" />
       </form>
       {this.state.text}
     </div>
   );
  }
};
 
export default CreateTodo;
```

**Note**: Inside the render function, we wrapped our form in a `div`, and then at the bottom of that `div` we've added the line `{this.state.text}`. This isn't necessary for functionality, but we do this just to visually confirm that we are properly changing the state. If we see our DOM change with every character we type in, we're in good shape.

It's on to step 2.

### 2. On Submission of Form, Dispatch an Action to the Store

```jsx
import React, { Component } from 'react';
import { connect } from 'react-redux';
 
class CreateTodo extends Component {
  state = {
    text: ''
  };
 
  handleChange = event => {
    this.setState({
      text: event.target.value
    });
  };
 
  handleSubmit = event => {
    event.preventDefault();
    this.props.addTodo(this.state);
  };
 
  render() {
    return (
      <div>
        <form onSubmit={event => this.handleSubmit(event)}>
          <p>
            <label>add todo</label>
              <input
                type="text"
                onChange={event => this.handleChange(event)}
                value={this.state.text}
              />
          </p>
          <input type="submit" />
        </form>
      </div>
    );
  }
}
 
const mapDispatchToProps = dispatch => {
  return {
    addTodo: formData => dispatch({ type: 'ADD_TODO', payload: formData })
  };
};
 
export default connect(
  null,
  mapDispatchToProps
)(CreateTodo);
```

Now, when the form is submitted, whatever the `this.state` is will be dispatched to the reducer with the action.

#### Alternate `export` statement

```jsx
import React, { Component } from 'react';
import { connect } from 'react-redux';
 
class CreateTodo extends Component {
  state = {
    text: ''
  };
 
  handleChange = event => {
    this.setState({
      text: event.target.value
    });
  };
 
  handleSubmit = event => {
    event.preventDefault();
    this.props.dispatch({ type: 'ADD_TODO', payload: this.state });
  };
 
  render() {
    return (
      <div>
        <form onSubmit={event => this.handleSubmit(event)}>
          <p>
            <label>add todo</label>
            <input
              type="text"
              onChange={event => this.handleChange(event)}
              value={this.state.text}
            />
          </p>
          <input type="submit" />
        </form>
      </div>
    );
  }
}
 
export default connect()(CreateTodo);
```

Now, if you start up the app and click the submit button, you should see your actions via a `console.log` in our reducer.

### 3. Update State

 Well remember our crux of redux flow: Action -> Reducer -> New State.

Our initial state should be an empty list of todos, { todos: [] }.

```jsx
// ./src/reducers/manageTodo.js
 
export default function manageTodo(state = {
  todos: [],
}, action) {
  switch (action.type) {
    case 'ADD_TODO':
 
      console.log({ todos: state.todos.concat(action.payload.text) });
 
      return { todos: state.todos.concat(action.payload.text) };
 
    default:
      return state;
  }
}
```

Ok, once you change the `manageTodo()` reducer to the above function, open up the console in your browser, and try clicking the submit button a few times. The log will show that our reducer is concatenating new values every time the form is submitted!

## Summary

There's a lot of typing in this section, but three main steps.

- First, we made sure the React component of our application was working. We did this by building a form, and then making sure that whenever the user typed in the form's input, the state was updated.
- Second, We connected the component to **Redux** and designed our `mapDispatchToProps`
- Third, we built our reducer such that it responded to the appropriate event and concatenated the payload into our array of todos.