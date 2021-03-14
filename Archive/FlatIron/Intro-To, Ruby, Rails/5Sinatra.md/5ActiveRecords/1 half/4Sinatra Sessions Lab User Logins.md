# lab

```ruby
class CreateUser < ActiveRecord::Migration[5.1]
  def change
    create_table :users do |t|
    t.string :username
    t.string :password
    t.float :balance
    end
  end
end
```

```ruby
require_relative '../../config/environment'
class ApplicationController < Sinatra::Base
  configure do
    set :views, Proc.new { File.join(root, "../views/") }
    enable :sessions unless test?
    set :session_secret, "secret"
  end

  get '/' do
    erb :index
  end

  post '/login' do
   @user = User.find_by(username: params[:username], password: params[:password]) #to find a key by a key
  if @user 
    session[:user_id] = @user.id #used session hash to set user_id = @user.id
  redirect to '/account'
  end
  erb :error
end

  get '/account' do
    if Helpers.is_logged_in?(session)
      @user = Helpers.current_user(session)
      erb :account
        #checks to see if the user is loggin
    else 
    erb :error    #if there not feed them error
    end# also needed an else statment to work
  end

  get '/logout' do 
session.clear #user .clear to delete the session hash so the user would no logger be consider to be loggin
redirect to '/' then redirect back to home
  end


end

```

```ruby
class Helpers
  def self.current_user(session)
    User.find_by(id: session[:user_id]) #finds the user be attribute id: session[:user_id]
  end

  def self.is_logged_in?(session)
    true if self.current_user(session)      
  end
   #logged in IS true if self.current_user(session)
    #so if there id is in the sessions hash then they are logged in 
end
```

```erb
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>title</title>
    <link rel="stylesheet" href="stylesheets/bootstrap.css" type="text/css">  
  </head>
  <body id="page-top">
    <h1>Welcome <%=@user.username%></h1> <!--@user is equal to current user method in the helper class-->
    <h3>Your Balance: <%=@user.balance%></h3>
    
<a href="/logout">Log Out</a>
  </body>
</html>
```

```erb
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>title</title>
    <link rel="stylesheet" href="stylesheets/bootstrap.css" type="text/css">  
  </head>
  <body id="page-top">
    <h1>You Must <a href="/">Log In</a> to View Your Balance</h1>
#did not make this doc
  </body>
</html>
```

```erb
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>title</title>
    <link rel="stylesheet" href="stylesheets/bootstrap.css" type="text/css">  
  </head>
  <body id="page-top">
    <h1>Welcome to FlatBank</h1>
    <h3>Log In Please</h3>
    <form action="/login" method="POST">
    <label for="username">Username:</label><input type="text" name="username"><br>
    <label for=""></label>Password:<input type="text" name="password"><br>
    <input id="submit" type="submit" value="Login">
        <!--Just made a simple form to submit to the params hash-->
    </form>
  </body>
</html>
```

