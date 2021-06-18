Let's look at what the simplest possible login system might look like in Rails.

The flow will look like this:

- The user GETs `/login`
- The user enters their username. There is no password.
- The user submits the form, POSTing to `/login`.
- In the create action of the `SessionsController` we set a cookie on the user's browser by writing their username into the session hash.
- Thereafter, the user is logged in. `session[:username]` will hold their username.

`SessionsController`

action/**new**

```ruby
get "/login", to: "sessions#new"
```

action/**create**

```ruby
post "/login", to: "sessions#create"
```

Typically, a create method will look up a user in the DB verify their login credentials, and then store the authenticated user's id in the session

But, for now sessions controller will just have to trust a user

```ruby
class SessionsController < ApplicationController
    def new
        # nothing to do here!
    end
 
    def create
        session[:username] = params[:username]
        redirect_to '/'
    end
end
```

small login for using new.html.erb

```erb
<form method='post'>
  <input name='username'>
  <input type='submit' value='login'>
</form>
```

 

## Logging out

`SissionsController#destroy` will  clear the username  out of the session

```ruby
def destroy
  session.delete :username
end
```

