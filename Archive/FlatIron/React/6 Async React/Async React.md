#### Using `fetch` Within React

`componentDidMount` happens to be a great place for making fetch requests. By putting a `fetch()` within `componentDidMount`, when the data is received, we can use `setState` to store the received data. This causes an update with that remote data stored in state. A very simple implementation of the App component with `fetch` might look like this:

```jsx
import React, { Component } from 'react'
 
class App extends Component {
 
  state = {
    peopleInSpace: []
  }
 
  render() {
    return (
      <div>
        {this.state.peopleInSpace.map(person => person.name)}
      </div>
    )
  }
 
  componentDidMount() {
    fetch('http://api.open-notify.org/astros.json')
      .then(response => response.json())
      .then(data => {
        this.setState({
          peopleInSpace: data.people
        })
      })
  }
}
 
export default App
```

If you have JSX content reliant on that state information, when `setState` is called and the component re-renders, the content will appear.

Since `componentDidMount` is also commonly used to initialize intervals, it is ideal to set up any repeating fetch requests here as well.

#### Using `fetch` With Events

We aren't limited to sending fetch requests when a component is mounted. We can also tie them into events:

```jsx
handleClick = event => {
  fetch('your API url')
    .then(res => res.json())
    .then(json => this.setState({data: json}))
}
 
render() {
  return (
    <button onClick={this.handleClick}>Click to Fetch!</button>
  )
}
```

#### Using State with POST Requests

Say we were building a user sign up form. When we send the data, our server is expecting two values within the body of the POST, `username` and `password`.

Setting up a React controlled form, we can structure our state in the same way:

```jsx
state = {
  username: "",
  password: ""
}
 
//since the id values are the same as the keys in state, we can write an abstract setState here
handleChange = event => {
  this.setState({
    [event.target.id]: event.target.value
  })
}
 
render() {
  return (
    <form onSubmit={this.handleSubmit}>
      <input type="text" id="username" value={this.state.username} onChange={this.handleChange}/>
      <input type="text" id="password" value={this.state.password} onChange={this.handleChange}/>
    </form>
  )
}
```

Then, when setting up the fetch request, we can just pass the entire state within the body, as there are no other values:

```jsx
handleSubmit = event => {
  event.preventDefault()
  fetch('the server URL', {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify(this.state)
  })

```

Notice how we're **not bothering to worry about** `event.target` when posting the data. **Since** the **form** is **controlled**, state contains the most up-to-date form data, and it is already in the right format!