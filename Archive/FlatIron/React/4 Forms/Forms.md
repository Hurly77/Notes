# code along

## Controlling Form Values From State

forms in React are practically identical to html forms. With React we want to set up **controlled form**

 A *controlled form* is **a form that derives its input values from state**. Consider the following:

```jsx
import React from 'react';
 
class Form extends React.Component {
  state = {
    firstName: "John",
    lastName: "Henry"
  }
 
  render() {
    return (
      <form>
        <input type="text" name="firstName" value={this.state.firstName} />
        <input type="text" name="lastName" value={this.state.lastName} />
      </form>
    )
  }
}
 
export default Form;
```

Though this is incomplete. To completely control a form, we also need our form to *update* state.

## Updating State via Forms

To update the state change of a form will use an event listener, `onChange`.

```jsx
<input type="text" onChange={event => this.handleFirstNameChange(event)} value={this.state.firstName} />
<input type="text" onChange={event => this.handleLastNameChange(event)} value={this.state.lastName} />
```

Above we pass in event as an argument, `event` data is automatically provided by the `onChange` event listener.

`handleFirstNameChange and handleLastNameChange`:

```jsx
handleFirstNameChange = event => {
  this.setState({
    firstName: event.target.value
  })
}
 
handleLastNameChange = event => {
  this.setState({
    lastName: event.target.value
  })
}
```

The `event` has data about `target` Which **is** whatever **DOM element the** `event` was triggered on.

`event.target.value` is not the State value but is the value of the last key stroke. so `this.firstName` is equal to *plus* the last key stroke.

our full component would look like:

```jsx
import React from 'react';
 
class Form extends React.Component {
  state = {
    firstName: "John",
    lastName: "Henry"
  }
 
  handleFirstNameChange = event => {
    this.setState({
      firstName: event.target.value
    })
  }
 
  handleLastNameChange = event => {
    this.setState({
      lastName: event.target.value
    })
  }
 
  render() {
    return (
      <form>
        <input type="text" onChange={event => this.handleFirstNameChange(event)} value={this.state.firstName} />
        <input type="text" onChange={event => this.handleLastNameChange(event)} value={this.state.lastName} />
      </form>
    )
  }
}
 
export default Form;
```

 when we run  **First and Last NameChange** methods we update state based on `event.target.value`. This, in turn, causes a re-render and the cycle completes. The *new* state values we just set are use to set the `value` attributes of out two `input`s .

## Submitting a Controlled Form

To submit the form will use `onSubmit` event listener.

```jsx
render() {
  return (
    <form onSubmit={event => this.handleSubmit(event)}>
      <input
        type="text"
        onChange={event => this.handleFirstNameChange(event)}
        value={this.state.firstName}
      />
      <input
        type="text"
        onChange={event => this.handleLastNameChange(event)}
        value={this.state.lastName}
      />
    </form>
  )
}
```

`handleSubmit` function:

```jsx
handleSubmit = event => {
  event.preventDefault()
  let formData = { firstName: this.state.firstName, lastName: this.state.lastName }
  this.sendFormDataSomewhere(formData)
}
```

**Break Down**

- `event.preventDefault()`: The default action for **form submit** is to redirect, because we don’t have a path the form will redirect to the page, `preventDefault()` will stop the default action.
- `let formData = {}`: values for submit
- `this.sendFormDataSomeWhere(formData)`: Code that handles sending data

Though we don’t have a Server we could simulate on like so:

```jsx
import React from 'react';
 
class Form extends React.Component {
  state = {
    firstName: "John",
    lastName: "Henry",
    submittedData: []
  }
 
  handleFirstNameChange = event => {
    this.setState({
      firstName: event.target.value
    })
  }
 
  handleLastNameChange = event => {
    this.setState({
      lastName: event.target.value
    })
  }
 
  handleSubmit = event => {
    event.preventDefault()
    let formData = { firstName: this.state.firstName, lastName: this.state.lastName }
    let dataArray = this.state.submittedData.concat(formData)
    this.setState({submittedData: dataArray})
  }
 
  listOfSubmissions = () => {
    return this.state.submittedData.map(data => {
      return <div><span>{data.firstName}</span> <span>{data.lastName}</span></div>
    })
  }
 
  render() {
    return (
      <div>
        <form onSubmit={event => this.handleSubmit(event)}>
          <input
            type="text"
            onChange={event => this.handleFirstNameChange(event)}
            value={this.state.firstName}
          />
          <input
            type="text"
            onChange={event => this.handleLastNameChange(event)}
            value={this.state.lastName}
          />
          <input type="submit"/>
        </form>
        {this.listOfSubmissions()}
      </div>
    )
  }
}
 
export default Form;
```

This will render submissions on to current Page

## More on Forms

Form elements include `<input>`, `<textarea>`, `<select>`, and `<form>` itself. When we talk about inputs in this lesson, we broadly mean the form elements (`<input>`, `<textarea>`, `<select>`) and not always specifically just `<input>`.

To control the value of these inputs, we use a prop specific to that type of input:

- For `<input>`, `<textarea>`, and `<option>`, we use `value`, as we have seen.
- For `<input type="checkbox">` and `<input type="radio">`, we use `checked`

Each of these attributes can be set based on a state value. Each also has an `onChange` event listener, allowing us to update state when a user interacts with a form.

## Uncontrolled vs Controlled Components

 If the component has a `value` prop, it is controlled (the state of the component is being controlled by React). If it doesn't have a `value` prop, it's an uncontrolled component.

#### Uncontrolled Components

Uncontrolled components’ value is kept in the DOM itself like and HTML form. To set the initial value for the element, we’d use the `defaultValue` prop.

to Submit an uncontrolled from, we can use the `onSubmit` handler:

```jsx
<form onSubmit={ event => this.handleSubmit(event) }>
  ...
</form>
```

all data from an uncontrolled form is accessible within the `event`,  this ends up looking like this

```jsx
handleSubmit = event => {
  event.preventDefault()
  const firstName = event.target.children[0].value
  const lastName = event.target.children[1].value
  this.sendFormDataSomewhere({ firstName, lastName })
}
```

#### Controlled Component

in controlled component we, explicitly set the value of a component using state, and update the value in response to any changes the user makes. this makes out `handleSubmit()` relatively simple:

```jsx
handleSubmit = event => {
  event.preventDefault()
  this.sendFormDataSomewhere(this.state)
}
```

 if we expanded our form to have *20* controlled inputs, this `handleSubmit` doesn't change. It just sends all *20* state values wherever we need them to go upon submission.

A **controlled form** does not **need to be in the same component** where the **state** is. to demonstrate

```jsx
// src/components/ParentComponent
import React from 'react';
import Form from './Form'
 
class ParentComponent extends React.Component {
  state = {
    firstName: "",
    lastName: "",
  }
 
  handleFirstNameChange = event => {
    this.setState({
      firstName: event.target.value
    })
  }
 
  handleLastNameChange = event => {
    this.setState({
      lastName: event.target.value
    })
  }
 
  render() {
    return (
      <Form
        formData={this.state}
        handleFirstNameChange={this.handleFirstNameChange}
        handleLastNameChange={this.handleLastNameChange}
      />
    )
  }
}
 
export default ParentComponent;
```

Then `Form` can become:

```jsx
// src/components/Form
import React from 'react';
 
class Form extends React.Component {
  render() {
    return (
      <div>
        <form>
          <input
            type="text"
            onChange={event => this.props.handleFirstNameChange(event)}
            value={this.props.formData.firstName}
          />
          <input
            type="text"
            onChange={event => this.props.handleLastNameChange(event)}
            value={this.props.formData.lastName}
          />
        </form>
      </div>
    )
  }
}
 
export default Form;
```

We were rendering `Form` inside `src/index.js`. Now we can have a component the renders `Form` as a child.

**Aside**: Submission functionality is omitted here for simplicity. Also, If you're following along in the example files, don't forget to update `index.js` to point to `ParentComponent`.

![image-20201008170731235](C:\Users\camer\AppData\Roaming\Typora\typora-user-images\image-20201008170731235.png)

We could also, create a sibling of `Form`, that displays our data.

```jsx
// src/components/DisplayData
import React from 'react';
 
class DisplayData extends React.Component {
  render() {
    return (
      <div>
        <h1>{this.props.formData.firstName}</h1>
        <h1>{this.props.formData.lastName}</h1>
      </div>
    )
  }
}
 
export default DisplayData
```

