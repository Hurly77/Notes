# how to setup AUTH with React and Rails

​	It doesn’t seem obvious at the time but, it really was quite simple to set up authentication, or at least the basics of it. As most Know, as your application grows it will become increasingly more difficult to balance authentication for that I would recommend a third party authentication, or even a backend api like firebase or or auth0, but for now if your just building a small application, which I wouldn’t realize it at the time but I was, so, lets get started. 

FIRST THINGS FIRST, I will assume that if you’ve made it far enough to be looking up authentication your quite familiar with generators. Run the generators for both react and Rails then will want to install,

```
gem install bcrypt
gem install rack-cors
```

and what ever else gems you may need for you project

on the react side after running npx, your should now have an `index.js `that looks like this 

```
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(
        <App />,
    document.getElementById('root')
);
```

Good, now in your `src` directory will want to create a new subdirectory for `redux`, along with that will make two other subfolders actions and reducers, than you’ll want to make a new index.js file for your “REDUX”. 

EX.

```
App.js
index.js
src
	-Redux_
	   |
		-actions
			-<empty>
				|
		-reducers
				|
			-<empty>
		-index.js
	-comppnents
	
```

inside your index.js file in  your REDUX DIR, is where all the magic happens. we’ll set up our store and as well some middleware and thunk. Yep where using THUNK !! 

of course you’ll need to install

`reat-redux` and  `thunk`

This is how I  set up mine

```
import { createStore, combineReducers, compose, applyMiddleware } from 'redux'
import thunk from 'redux-thunk'

const rootReducer = combineReducers({
	\*leave empty for now*\
})

const middleware = [thunk, reduxLogger]
const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;

const store = createStore(
  rootReducer,
  composeEnhancers(applyMiddleware(...middleware))
);

export default store
```

notice I export the default as store that will com in handy in our index.js file at the top level.

but first we need to set up a reducer

```
const authReducer = (
	state = {
		loggedIn       : false,
		currentUser    : {},
	},
	action,
) => {
	switch (action.type) {
		case 'AUTH_SUCCESS':
			return {
				...state,
				loggedIn    : action.payload.loggedIn,
				currentUser : action.payload.currentUser,
			};
		case 'LOGOUT':
			return {
				...state,
				loggedIn    : false,
				currentUser : {},
			};
			
		default:
			return state;
	}
};

export default authReducer;
```

next will want to set our action creator 

with thunk it looks a little like this 

```
export const signup = (user) => {
	return (dispatch) => {
		fetch(`${apiUrl}/users`, { // will set this up in the future
			method      : 'POST',
			headers     : {'Content-Type': 'application/json'},
			credentials : 'include',
			body        : JSON.stringify({user: user}),
		})
			.then((res) => res.json())
			.then(
				(data) =>
					dispatch({
						type    : 'AUTH_SUCCESS',
						payload : {loggedIn: data.logged_in, currentUser: data.user},
					}),
			);
	};
};
```

now lets set up our store shall we, 

```
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
 
import { Provider } from 'react-redux';
import store from './Redux'

ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>,
    document.getElementById('root')
```

boom, now we just need to do a little rails backend and we should be ready to go

Now will want to a `sessions_store.rb` and a `cors.rb` if your don’t already.

We need to give access to `cross orgin` so that cors will know what we are working with for our front end and backend

```ruby
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins 'http://localhost:3000'

    resource '*',
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head],
      credentials: true 
      # absoultly remember to set this in to true, and in your ajax to set  credentials: 'include'
  end

  allow do
    origins 'http://localhost:5000' #5000 is the port your can set yours to what ever you want.

    resource '*',
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head],
      credentials: true
  end
end

```

and in your session_store 

```ruby
Rails.application.config.session_store :cookie_store, key: "_your_app_name_here"
```

then will need to make sure we set has_secure_password in our user model, and that we also use password_digest in our users table. You should also set up a currentUserConcern to help out a bit

```
module CurrentUserConcern 
  extend ActiveSupport::Concern

  included do
    before_action :set_current_user
  end

  def set_current_user
    if session[:user_id]
      @current_user = User.find(session[:user_id])
    end
  end
end
```

Than boom all you need to do from there is set up your componet from end UI and build out your user controller to create  new user. boom bang. You did good kid, nice work….