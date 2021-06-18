```ruby
<h1>welcome, <%= @current_user %></h1>
<%= form_tag ({controller: "sessions", action: "destroy", method: "post"}) do %>

<input type="submit" name="submit" value="logout">
<% end %>
```

```ruby
<%= form_tag ({controller: 'sessions', action: 'create', method: 'post'}) do  %>

<input type="text" name="name">
<input type="submit" id="submit" value="login">
<% end %>
```

```ruby
class SessionsController < ApplicationController
  before_action :current_user
  skip_before_action :current_user, only: [:new]
  
  def new
  end

  def create
    if params[:name].nil? || params[:name].empty?
      redirect_to '/login'
    else
    session[:name] = params[:name]
    redirect_to '/welcome'
    end
  end

  def destroy
    session.delete :name
    redirect_to '/login'
  end
end
```

```ruby
class ApplicationController < ActionController::Base
  # Prevent CSRF attacks by raising an exception.
  # For APIs, you may want to use :null_session instead.
  protect_from_forgery with: :exception
  
  def current_user
    session[:name]    
  end

  def welcome
    @current_user = current_user
    if session[:name].nil? || session[:name].empty?
      redirect_to '/login'
    end
  end
end

```

```ruby
class SecretsController < ApplicationController
  before_action :logged_in?

  def show
  end

  private
  def logged_in?
    if session[:name].nil? || session[:name].empty?
      redirect_to '/login'
    end
  end
end
```

```

```

