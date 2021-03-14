```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import {createStore} from 'redux'
import {Provider} from 'react-redux'
import App from './App'

import manageUsers from './reducers/manageUsers'


const store = createStore(
  manageUsers,
  window._REDUX_DEVTOOLS_EXTENSION_&& window._REDUX_DEVTOOLS_EXTENSION_()
)


ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

```jsx
import React, { Component } from 'react';
import UserInput from './components/UserInput'
import ConnectedUsers from './components/Users'

class App extends Component {
  render() {
    return (
      <div className="App">
        <UserInput />
        <ConnectedUsers />
      </div>
    );
  }
}

export default App;
```

```jsx
export default function manageUsers(state = {
  users: [],
}, action){
  switch (action.type) {
    case 'ADD_USER':
      console.log('adding ', action.user);
      return {
        ...state,
        users: [...state.users, action.user]
      }

    default:
      return state;
  }
};
```

```jsx
import React, { Component } from 'react';
import { connect } from 'react-redux'

class UserInput extends Component {

  state = {
    username: '',
    hometown: ''
  }

  handleInputChange = (event) => {
    this.setState({
      [event.target.id]: event.target.value
    });
  }

  handleOnSubmit = (event) => {
    event.preventDefault();
    this.props.dispatch({type: 'ADD_USER', user: this.state})
  }

  render() {
    return(
      <form onSubmit={this.handleOnSubmit}>
        <p>
          <input
            type="text"
            id="username"
            onChange={this.handleInputChange}
            placeholder="username"
          />
        </p>
        <p>
          <input
            type="text"
            id="hometown"
            onChange={this.handleInputChange}
            placeholder="hometown"
          />
        </p>
        <input type="submit" />
      </form>
    )
  }
}

export default connect()(UserInput);
```

```jsx
import React, {Component} from 'react';
import {connect} from 'react-redux';

class Users extends Component {
	render() {
		let users = this.props.users;
		return (
			<div>
				<ul>
					Users!
					{users.map((user, id) => {
						return <li key={id}>{user.username}</li>;
					})}
					{users.length > 0 ? <div>{users.length}</div> : null}
				</ul>
			</div>
		);
	}
}

const mapStateToProps = (state) => {
	return {users: state.users};
};

export default connect(mapStateToProps)(Users);
```

