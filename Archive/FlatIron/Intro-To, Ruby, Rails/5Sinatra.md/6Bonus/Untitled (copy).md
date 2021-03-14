Get Started Building Your first Sinatra app.

​	Creating a web application is no easy task, I mean not for you average beginner, and If your like me you might have a hard time trying to figure out where to begin. So where do we begin or better yet where did I begin?

## 1. NUMERO UNO

If your anything like me, you'll want to cut out is much monotonous work as possible this includes building a File structure. Luckily for us there is a nice little gem Know as called [corneal](https://github.com/thebrianemory/cornealthat) will do the the bulk of the work for us. Just like any other gem you'll want to install said gem like so:

```ruby
gem install corneal
#run
corneal new App-Name
bundle install
```

This should give access to a ton of important gems that we'll need for our app such as [activerecords](https://guides.rubyonrails.org/index.html), [bcrypt](https://www.npmjs.com/package/bcrypt), [rake](https://github.com/ruby/rake), and, most importantly [Sinata]() and, more; check out the corneal link above for full details.

​	Also thanks to [corneal](https://github.com/thebrianemory/cornealthat) our we  have a file structure that should look something like this

```
├── config.ru
├── Gemfile
├── Gemfile.lock
├── Rakefile
├── README
├── app
│   ├── controllers
│   │   └── application_controller.rb
│   ├── models
│   └── views
│       ├── layout.erb
│       └── welcome.erb
├── config
│   ├── initializers
│   └── environment.rb
├── db
│   └── migrate
├── lib
│   └── .gitkeep
└── public
|   ├── images
|   ├── javascripts
|   └── stylesheets
|       └── main.css
└── spec
    ├── application_controller_spec.rb
    └── spec_helper.rb
```

## NEXT

Now we can start by building out our migrations table. 

first you'll want first we start with our has_many and belongs to relationship, It is important to remember that, has_many is an [activerecords](https://guides.rubyonrails.org/active_record_querying.html) method. Let say that we were building a simple user application. we could create our rake comands such as:

```bash
rake db:create_migration NAME=user
rake db:create_migration NAME=Item
```

 

This should give us two migrations that look like this

```ruby
#db/migrate/10001_create_users.rb
class CreateUsers < ActiveRecord::Migration
  def change
    create_table :user do |t|
     t.timestamps null: false
    end
  end
end
#db/migrations/10002_create_Items.rb
class CreateItems < ActiveRecord::Migration
  def change
    create_table :user do |t|
     t.timestamps null: false
    end
  end
end
```

we'll want to build out the the table with the appropriate colums like so:

```ruby
#db/migrate/10001_create_users.rb
class CreateUsers < ActiveRecord::Migration
  def change
    create_table :user do |t|
     t.string :user_name
     t.email
     t.password_digest
     t.string :item_name
     t.timestamps null: false
    end
  end
end
#db/migrations/10002_create_Items.rb
class CreateItems < ActiveRecord::Migration
  def change
    create_table :user do |t|
    t.string  :name
    t.integer :item_id 
     t.timestamps null: false
    end
  end
end
```

Now in our models class we want to make our assosiaction that way we can use assoaction methods that we inhert from activerecords.

```ruby
#app/model/user.rb
class User 
has_many :items
end

#app/model/item.rb
class Item 
belongs_to :user
end
```

Now, in a real world situation we would'nt want to do something lilke "user has_many: through: :user_items" but for the purpose of this demonstration we'll keep it pretty simple. 

## and Then

Now after running `rake migrate` we can start building our routes lets start with the login process

##### authentication 

we can't very well build an app lication  that just allows our user's password to be passed around with no security. so we'll use some validation methods inherited from active records

```ruby
#app/model/user.rb
class User 
has_many :items
 has_secure_password #mehtod, for salting our password aka password digest
  validates :user_name, :email, presence: true,  uniqueness: true
    #validates user information to make sure  it is "valid"
  validates :password, presence: true
  validates :password_confirmation, confirmation: true
end

#app/model/item.rb
class Item 
    #for this example we won't need to validate our belongs_to method
belongs_to :user
end
```



```ruby
require './config/environment'
require 'sinatra/flash'

class ApplicationController < Sinatra::Base

  configure do
    set :public_folder, 'public'
    set :views, 'app/views'
    enable :sessions #this will set a session so that we can store the user id in a hash and grab the current_user's ID
    register Sinatra::Flash #this is so we can display error messages to the user.
    set :session_secret, "no_peeping_jerrys" #makes** it **harder** for **someone to create a fake session_id and hack into your site** without signing up or signing in.
      
  end
  
  get "/" do
    erb :index
  end
  
  helpers do
    def logged_in? # checks to see if user is a user is logged in
      !!session[:user_id]
    end

    def current_user #returns true if the user making changes is current user.
      user.find(session[:user_id])
    end
  end
end
```

This will All of our classes in our models folder will inherit this class

Now we can build our user and items Route :

```ruby
class usersController < ApplicationController

  get '/signup' do #renders the signup page for the client
    if !logged_in? #checks to make sure they are not already logged in

      erb :'/users/new.html' 
     else
      redirect to '/users'
    end
  end
	
   post '/signup' do 
   @user = User.new(name:#obj information goes here)
       if @user.save # if true
           session[:user_id] = @user.id #sets the id attribute to be equal to session id
   		redirect to "users/#{@user.id}" #send to a spicific user page       
       end
   end  
       get '/login' do #if the user has an existing account this renders the login
    if !logged_in?

      erb:'users/index.html'
     else
      redirect to'/users'
    end
  end
        post '/login' do #this will be passed into params so we can minuplate in our controller with "post"
    @user = User.find_by(user_name: params[:user_name]) #AR dynamic method allows us to grab the user by attribute input
    if @user && @user.authenticate(params[:password])
      session[:user_id] = @user.id

      redirect to "/users/#{@user.id}"
     else
      redirect to '/login'
    end
  end

  get '/logout' do
    if logged_in?
      session.clear #clears session whe the user would like to leave
      redirect to '/'
    end
  end
end
```

This will give us access to all the user's via instace variable



```ruby
class ItemsController < ApplicationController
get "/items/new" do
    erb :"/items/new.html"
  end

  # POST: /items
  post "/items" do
    params.delete_if {|p| p == "submit"}
    if !item.find_by(name: params[:name])
      @item = item.new(params)
      @item.save
      current_user.items << @item
      current_user.save
        
      redirect to "/items/#{@item.id}"    
  end
end
```

Vuala, now you have the start of a working web app Congrats!! Now I know there is alot of unpacking to do here but, unpacking and, disecting is our jobs its what, I believe to be a good engineer in a general since, so if your reading this and you find youself over whelmed, Just keep in mind that this is how you truly learn to understand code.

If you would like to view a Sinatra project that I built useing this [GoRobo](https://github.com/Hurly77/gorobo)

RESOUCES

[AR assosiations](https://guides.rubyonrails.org/association_basics.html)

[AR query interface](https://guides.rubyonrails.org/active_record_querying.html)

