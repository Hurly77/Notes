gems-

-  `cors` latest version
- `bcrypt`



Config files need to be changed, inside top level of config inside `application.rb`

```ruby 
config.api_only = false 
```

inside `config/initializers` add,

```ruby
 credentials: true
```

and,

```ruby
...
allow do
    origins 'react-app url'

    resource '*',
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head]
    credentials: true
  end
```

create file inside `config/initializers` called `session_store.rb`

```ruby
Rails.application.config.session_store :cookie_store, key: "_authentication_app", domain: "host_name"
```

## authentication features

User needs attribute `password_digest`

needs a confirmation password in the `form`

In model `User`, use `has_secure_password`

**validations**

`validates_presence_of :email`

`validates_uniqness_of :email`

`we will need a sessions controller`

with resource `:sessions`, `only: [:attributes]`

Inside `SessionsController < ApplicationController`

```ruby
def create
user = User
    	.find_by(email: params["user"]["email"])
    #with has_secure_password we get this helpful :authenticate method
    	.try(:authenticate, params[:user][:password])
    #.try is built-in method 
    if user
        sessions[:user_id] = user.id
        render json: {
            status: :created,
            logged_in: true,
            user: user
            }
    else
        render json: { status: 401 }
    end
end
```

In `config/routes`

crate another resource `registrations, only: [:attributes]`

Inside `resgistrationsController.rb`

```ruby
def create
	user = User.create!(
			email: params[:user][:email],
			password: params[:user][:password]
			password_confirmation: params[:user][:password_confirmation]
			)
    if user
        session[:user_id] = user.id
        render json: {
            status: created,
            user: user
            }
    else
        render json: { status: 500 }
    end
end
```

Inside `application_controller`

```ruby
skip_before_action :verify_authenticity_token
```

Update `routes` in config file

```ruby
Rails.application.routes.draw do
	resources :sessions, only: [:create]
    resources :registrations, only: [:create]
    delete :logout, to:  "sessions#logout" #added route
    get :logged_in, to: "sessions#logged_in" #added route
    root to: "static#home"
end
```

Will need to create a concern for `current_user_concern.rb`

```ruby 
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
```

In `sessionsController`

```ruby
include CurrentUserConcern

...
    
#this will check to see if a user is logged in
def logged_in
    if @current_user
        render json: {
            logged_in: true,
            user: @current_user
            }
    else
        render json: {
            logged_in: false
            }
    end
end

def logout
    reset_session
    render json: {status: 200, logged_out: true}
end
```

In order to get this to work with react and development

In `config/sessions_store.rb`

```ruby
if Rails.env == "production"
...
else
Rails.application.config.session_store :cookie_store, key: "_authentication_app"
end
```

## React side of things

  Build a Registration `component`

for password form

```jsx
 <input
 type="password"
 name="password"
 placeholder="Password" //impotant
 value={this.state.password}
 onChange={this.handleChange}
 required
 />

<input
 type="password"
 name="password_confirmation"
 placeholder="Password Confirmation" //impotant
 value={this.state.password}
 onChange={this.handleChange}
 required
 />

<button type="submit"></button>
```

when create `onhandleSubmit function in regstration component`

use `thunk` dependency to make a post request with fetch() API method. Use dispatch method in container to handle this in your reducers

```js
handleSubmit(event) {
const userData = {...this.state}
this.props.postUser(userData)
event.preventDefault()
}
{ withCredentials: true}

//with fetch() api you set crentials like so 
fetch('https://example.com', {
  credentials: 'include'
}).then(r => {
    console.log(r)
}).catch(error => {
    console.log(error)
})
```

