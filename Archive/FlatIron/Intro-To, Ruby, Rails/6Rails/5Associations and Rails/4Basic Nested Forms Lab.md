```ruby
class Recipe < ActiveRecord::Base
  has_many :ingredients 
  accepts_nested_attributes_for :ingredients
    #this allows our CollectionProxy :ingredients or Recipe.ingredients to allow attributes to be in our array, accencially turning it into a hash.
    #also it is important to rember that this is not much different then saying 
    #Recipe_attributes=
end
```

```ruby
class RecipesController < ApplicationController
  def show
    @recipe = Recipe.find(params[:id])
  end

  def index
    @recipes = Recipe.all
  end

  def new
    @recipe = Recipe.new
      @recipe.ingrients.build 
    @recipe.ingredients.build#each one of these is an unintilized obj we call build on the Proxy method to allow us to display the ingredients attributes with out having to presist to the record
    @recipe.ingredients.build
      #last time we passed and argument like so 
      #@obj.proxy_collection.build(attribute: "value") 
      #because we wanted to create and attribute the would already be in the text field.
  
  end

  def create
    @recipe = Recipe.new(recipe_params)

    if @recipe.save
      redirect_to recipes_path
    else
      render :new
    end
  end

  private
  
  def recipe_params
    params.require(:recipe).permit(
      :title,
      ingredients_attributes: [
        :name,
        :quantity
      ]
    )
	#again #permit will not allow singular data types such as and array or a hash
      #so we pass in ingredients_attributes: as and array with to attributes which do have singular values
  end
```

```erb
<%= form_for(@recipe) do |f| %>
<%= f.label :title %> <br> 
<%= f.text_field :title %> <br>
<br>
<%= f.fields_for :ingredients do |ingr| %>
<%= ingr.label :name %> <br>
<%= ingr.text_field :name %> <br>

<%= ingr.label :quantity %> <br>
<%= ingr.text_field :quantity %> <br> <br>
<% end %>

<%= f.submit %>

<% end %>
```

