A **hashing** algorithm manipulates data in such a way that it cannot be un-manipulated.

A **salt** is simply a random string of characters that gets added into the hash.

## Starter Code

```ruby
configure do
		set :views, "app/views"
		enable :sessions
		set :session_secret, "password_security"
	end

	get "/" do
		erb :index
	end

	get "/signup" do
		erb :signup
	end

	post "/signup" do
		#your code here!
	end

	get "/login" do
		erb :login
	end

	post "/login" do
		#your code here!
	end

	get "/success" do
		if logged_in?
			erb :success
		else
			redirect "/login"
		end
	end

	get "/failure" do
		erb :failure
	end

	get "/logout" do
		session.clear
		redirect "/"
	end

	helpers do
		def logged_in?
			!!session[user_id]
		end

		def current_user
			User.find(session[user_id])
		end
	end

end
```

## Password Encryption with BCrypt

**BCrypt**- will store a salted, hashed version of our users' passwords in our database in a column called `password_digest`. Essentially, **once a password is salted** and hashed, there is **no way for anyone to decode it**

### Implementing BCrypt

```ruby
#ussually we'd just use change method instead of both up and down
class CreateUsers < ActiveRecord::Migration[5.1]
  def up
    create_table :users do |t|
      t.string :username
      t.string :password_digest
    end
  end
 
  def down
    drop_table :users
  end
end
```

### ActiveRecord's `has_secure_password`

**A macro** is a method that when called, **creates methods** for you

This **ActiveRecord *macro*** gives us access to a few new methods. **It works in conjunction with a gem called** `bcrypt` and gives us all of those abilities in a secure way that doesn't actually store the plain text password in the database.

```ruby
class User < ActiveRecord::Base
  has_secure_password
end
```

Note that even though our database has a column called `password_digest`, we still access the attribute of `password`. This is given to us by `has_secure_password`. You can **read more about** that in the [Ruby Docs](http://api.rubyonrails.org/classes/ActiveModel/SecurePassword/ClassMethods.html#method-i-has_secure_password).

```ruby
post "/signup" do
    #makes a new instance of our user class
  user = User.new(:username => params[:username], :password => params[:password])
end
```

Because our user has `has_secure_password`, **we won't be able to save this to the database *unless*** our user filled out the password field. Calling `user.save` will **return false if the user can't be persisted.**

```ruby
post "/signup" do
  user = User.new(:username => params[:username], :password => params[:password])
 #redirect to '/login' if the user is saved, or '/failure' if the user can't be saved.
  if user.save
    redirect "/login"
  else
    redirect "/failure"
  end
end
```

Next, create at least one valid user, then let's build out our login action. In `post '/login'`, let's find the user by username.

```ruby
post "/login" do
  user = User.find_by(:username => params[:username])
end
```

Next, we need to check two conditions: first, did we find a user with that username? This can be written as `user != nil` or simply `user`.

```ruby
#This can be written as user != nil as well
post "/login" do
  user = User.find_by(:username => params[:username])
  if user
    redirect "/success"
  else
    redirect "/failure"
  end
end
```

We also need to check if that user's password matches up with the value in `password_digest`

Users must have both an account *and* know the password. We **validate** password match by using a **method** called `authenticate` on our `User` model 

**THAT IS WHAT THIS IS FOR**

```ruby
class User < ActiveRecord::Base
  has_secure_password
end
#THE AUTHENTICATE METHOD IS INVISIBLE
```

we told Ruby to add an `authenticate` method to our class (**invisibly**!) when the program runs. While we, as programmers can't see it, **it will be there**.

##### how `User`'s `authenticate` method works: (it...)

1. Takes a `String` as an argument e.g. `i_luv@byron_poodle_darling`
2. It turns the `String` into a salted, hashed version (`76776516e058d2bf187213df6917a7e`)
3. It compares this salted, hashed version with the user's stored salted, hashed password in the database
4. If the two versions match, `authenticate` will return the `User` instance; if not, it returns `false`

**IMPORTANT** At no point do we look at an unencrypted version of the user's password.

```ruby
post "/login" do
  user = User.find_by(:username => params[:username])
# ensure that we have a User AND that that User is authenticated
  if user && user.authenticate(params[:password])
    session[:user_id] = user.id #sets the session hash key "user_id" equal to user.id 
    redirect "/success"
  else
    redirect "/failure"
  end
end
```

