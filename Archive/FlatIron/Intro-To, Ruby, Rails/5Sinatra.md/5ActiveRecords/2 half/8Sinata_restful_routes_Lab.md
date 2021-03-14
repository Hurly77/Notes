# db

#### /Migrations/

```ruby
class CreateRecipe < ActiveRecord::Migration
  def change
    create_table :recipes do |t|
      t.string :name
      t.string :ingredients
      t.integer :cook_time
    end
  end
end

```

# app

### /models/recipe.rb

```ruby
class Recipe < ActiveRecord::Base
  
end
```

## /views

### edit.erb

```erb
<form method="post" action="/recipes/<%= @recipe.id %>">

  <input id="hidden" type="hidden" name="_method" value="patch">
  <label for="name:">Name:</label> <input type="text" name="name" value="<%= @recipe.name %>"><br> <!--sets perivous data to be in the text box incase they don't need to update that field-->
  <label for="ingredients">Ingredients:</label><input type="text" name="ingredients" value="<%= @recipe.ingredients %>"><br>
  <label for="cook_time">Cook Time:</label><input type="text" name="cook_time" value="<%= @recipe.cook_time %>"><br>
  <input type="submit" value="submit">
</form> 
 5  app/views/index.erb 
```

### index.erb

```erb
<%@recipes.each do |res| %>
  <a href="/recipes/<%= res.id %>">
    <h2><%= res.name %></h2>
    <p><%= res.ingredients %></p>
    <p><%= res.cook_time %></p>
  </a>
<% end %>
```

### new.erb

```erb
<form action="/recipes" method="POST"> 
<div class="container">
  <label for="name">Recipe Name: </label> <input type="text" name="name" id="name"><br>
  <label for="title">Ingredeints </label><input type="text" name="ingredeints" id="ingredients"><br>
  <label for="cook time">Cook Time</label><input type="text" name="cook_time"  id="cook_time"><br>

  <button type="submit" id="submit">Submit</button>
</div>
</form>
```

### show.erb

```erb
<h1><%= @recipe.name %></h1>
<h3>Ingredients:</h3>
<p><%= @recipe.ingredients %></p>
<h3>Cook Time:</h3>
<p><%= @recipe.cook_time %> minutes</p>

<form method="post" action="/recipes/<%= @recipe.id %>">
  <input id="hidden" type="hidden" name="_method" value="delete">
  <button type="submit" value="delete"></button>
</form> 
```

###  controllers/Application_controller.rb

```ruby
class ApplicationController < Sinatra::Base
  configure do
    set :public_folder, 'public'
    set :views, 'app/views'
  end
get '/' do
end

get '/recipes' do
@recipes = Recipe.all 
erb :index
end

get '/recipes/new' do

  erb :new
end

post '/recipes' do
@recipe = Recipe.create(params)

redirect to "/recipes/#{@recipe.id}"
end

get '/recipes/:id' do
@recipe = Recipe.find_by_id(params[:id])

erb :show
end

get '/recipes/:id/edit' do
  @recipe = Recipe.find_by_id(params[:id])
  
  erb :edit
end

patch '/recipes/:id' do
@recipe = Recipe.find_by_id(params[:id])
@recipe.name = params[:name]
@recipe.ingredients = params[:ingredients]
@recipe.cook_time = params[:cook_time]
@recipe.save

redirect to "/recipes/#{@recipe.id}"
end

delete '/recipes/:id' do
  @recipe = Recipe.find_by_id(params[:id]).delete

  redirect to '/recipes'
end

end
```

