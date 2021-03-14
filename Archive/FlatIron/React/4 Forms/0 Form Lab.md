1. Open the `components/TwitterMessage.js` file.
2. This component takes one prop: `maxChars` which is a number — the maximum amount of characters a message can have. This prop is being passed in from the App component with the value `280`.
3. You'll find an `<input type="text">` in this component. Make this a controlled component by adding the attributes to the `<input>` element. Its value should be saved in the component's state and should update on *every* change to the input.
4. Show the *remaining* characters in the component. It doesn't matter how you render it, as long as the number is correct. No need to guard against input that is too long — you can let the counter reach negative values.

1. Open the `components/TwitterMessage.js` file.
2. This component takes one prop: `maxChars` which is a number — the maximum amount of characters a message can have. This prop is being passed in from the App component with the value `280`.
3. You'll find an `<input type="text">` in this component. Make this a controlled component by adding the attributes to the `<input>` element. Its value should be saved in the component's state and should update on *every* change to the input.
4. Show the *remaining* characters in the component. It doesn't matter how you render it, as long as the number is correct. No need to guard against input that is too long — you can let the counter reach negative values.
5. Remember that you can retrieve the input `name` and `value` of an `event.target` from the JS event.
6. Add the necessary event handler to the `<form>` element in order to call the `onSubmit` callback prop.
7. The `onSubmit` callback prop should only be called if *both* fields are filled in (with any value).

**Note**: In the starter code are `id` attributes - these are used in the tests, so make sure to leave them as they are.

```jsx
//src/LoginForm.js
import React from "react";

class LoginForm extends React.Component {
  constructor() {
    super();

    this.state = {
      UserName: '',
      Password: '',
      submitData: []// I'm pretty sure this was useless.
    };
  }

  handleUserName = (event) => {
    this.setState({
      UserName: event.target.value
    })
  }

  handlePassword = (event) => {
    this.setState({
      Password: event.target.value
    })
  }

  handleSubmit = event => {
    event.preventDefault()
    if(this.state.UserName == '' || this.state.Password == '')return
    this.props.handleLogin(this.state)

  }	  

  render() {
    return (
      <form onSubmit={event => this.handleSubmit(event)}>
        <div>
          <label>
            UserName
            <input id="username" name="username" type="text"  
              onChange={event => this.handleUserName(event)}
              value={this.state.UserName}
            />
          </label>
        </div>
        <div>
          <label>
            Password
            <input id="password" name="password" type="password"
              onChange={event => this.handlePassword(event)}
              value={this.state.Password}
            />
          </label>
        </div>
        <div>
          <button type="submit" >Log in</button>
        </div>
      </form>
    );
  }
}

export default LoginForm;
```

```jsx
//src/TwitterMessage.js
import React from "react";

class TwitterMessage extends React.Component {
  constructor() {
    super();
    this.state = {
      message: "",
      chars: 0
    };
  }

  handleMessageChange = event => {
    this.setState({
      message: event.target.value
    })
  }

  render() {
      //for some reason this has to be done before return, then put variable in return statement
    let chars = this.props.maxChars - this.state.message.length
    return (
      <div>
        <strong>Your message:</strong>
        <input type="text" name="message" id="message" 
        onChange={event => this.handleMessageChange(event)}
        value={this.state.message}
        />
        {chars}
      </div>
    );
  }
}

export default TwitterMessage;
```

```jsx
//src/App.js
import React, { Component } from 'react'
import LoginForm from "./components/LoginForm";
import TwitterMessage from "./components/TwitterMessage";

class App extends Component {

  login = ({ username, password }) => {
    console.log(`Logging in ${username} with password ${password}`);
  };

  render() {
    return (
      <div>

        <h1>
          <pre>LoginForm</pre>
        </h1>
        <LoginForm handleLogin={this.login} />

        <h1>
          <pre>TwitterMessage</pre>
        </h1>
        <TwitterMessage maxChars={280} />



      </div>
    )
  }
}

export default App
```

