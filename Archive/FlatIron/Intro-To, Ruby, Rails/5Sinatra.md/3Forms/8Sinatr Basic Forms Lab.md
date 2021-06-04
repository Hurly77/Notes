# lab

##### views /index.erb

```erb
<h1> Welcome to the puppy adoption site!</h1>

<a href="/new">List A Puppy</a>
```



##### views /create_puppy.erb

```erb
<form method="post" action="/puppy"> #will post info to '/puppy'
  <label for="name"> Puppy Name</label>
  <input type="text" id="name" name="name">
  <label for="breed"> Puppy Breed</label>
  <input type="text" id="breed" name="breed">
  <label for="age"> Puppy Age</label>
  <input type="text" id="age" name="age">
  <input type="submit" value="submit">
</form>
```



##### views /display_puppy.erb

```erb
<body>
  <p>Puppy Name: <%= @puppy.name %></p>
  <p>Puppy Breed: <%= @puppy.breed %></p>
  <p>Puppy Age: <%= @puppy.age %> months</p>
</body>
```



##### models /puppy.rb

```ruby
class Puppy
  attr_accessor :name, :breed, :age

  def initialize(name, breed, age)
    @name = name
    @breed = breed
    @age = age
  end
end
```



##### app.rb

```ruby
require_relative 'config/environment'
require_relative 'models/puppy.rb'
require 'pry'
class App < Sinatra::Base
get '/' do
  erb :index
end

get '/new'do
erb :create_puppy
end

post '/puppy' do
@puppy = Puppy.new(params[:name], params[:breed], params[:age])
    #above use to be (params[:name, :breed, :age]) that didn't work though, Becuase then I'd be calling @puppy name, breed, age all in one agrument instead of the three
#Hurly = Puppy.new("hurlylab8", nil, nil) "if" That was even posible
erb :display_puppy
end
pry
end

```

