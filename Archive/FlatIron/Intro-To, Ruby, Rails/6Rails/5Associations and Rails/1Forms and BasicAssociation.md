**ternary** - using three as a base

The ternary operator is like an if else statement with two possible out comes

```ruby
if money < 100
	:do_not_buy_stuff
else
	:buy_stuff
end
#this with the help of the ternary operator can be used like so
money < 100 ? :do_not_buy_stuff : :buy_stuff
```



### The problem 

```ruby
# app/models/post.rb
class Post < ActiveRecord::Base
  belongs_to :category
end
 
# app/models/category.rb
class Category < ActiveRecord::Base
  has_many :posts
end
#We're going to need a form for the Post's content, and some way to represent what Category the Post belongs to.
```

## Using the category ID

We could build a form like this...

```erb
<%= form_for @post do |f| %>
  <%= f.label :category_id, :category %>
<!--need to use Id because where useing a @post object that has a method or CollectionProxy I believe its called-->
  <%= f.text_field :category_id %>
<!--this is also using the the category_id attr to to assign the value of what ever data is entered to the attr value duh-->
  <%= f.text_field :content %>
  <%= f.submit %>
<% end %>
```

If our `PostsController` looks something like this, then our form will work

```ruby
class PostsController < ApplicationController
  def create
    Post.create(post_params)
  end
 
  private
 
  def post_params
    params.require(:post).permit(:category_id, :content)
  end
end
#THis would not be very user friendly, because a user would have to know the id of what ever instance they wanted such as the category_id
```

If we rewrite our code like this:

```ruby
class PostsController < ApplicationController
  def create
    category = Category.find_or_create_by(name: params[:post][:category_name])
      #this will look through the db to see if a post with the same name and category name exist and if not it'll create one if it does not.
    Post.create(content: params[:post][:content], category: category)
  end
end

```

The problem is now whenever we want to set a category for a post we''ll have to do this.

## Defining a custom setter and getter (convenience attributes on models)

We can define our own methods our ruby models

```ruby
# app/models/post.rb
class Post < ActiveRecord::Base
   def category_name=(name)
       #This setter method is called whenever Post obj is initialized with a category_name field
     self.category = Category.find_or_create_by(name: name)
   end
 
   def category_name
      self.category ? self.category.name : nil
       #this is know as a ternary operator and accencially boils down to 
       #if self.category is true, self.category.name else if false nil
   end
end
```



```ruby
#Post.create(post_params) expanded 
Post.create({
  category_name: params[:post][:category_name],
    #this is just using the method we built for category_name
    #if no value is passed then it will return false
  content: params[:post][:content]
})
```

Our `#category_name=` is called now instead of setting the value of `category_name` through Active Record. Doing this will intercept the DB call. These Kinds of attributes "virtuals". 

now Our PostsController is simpler

```ruby
class PostsController < ApplicationController
  def create
    Post.create(post_params)
      #post_params prompts a call to the category_name=
  end
 
  private
 
  def post_params
    params.require(:post).permit(:category_name, :content)
      #now we use the name instead of the ID
  end
end
```

Change the view to fit our new code

```erb
<%= form_for @post do |f| %>
  <%= f.label :category_name %>
	<!--now a user can enter a name instead of an ID-->
  <%= f.text_field :category_name %>
  <%= f.text_field :content %>
  <%= f.submit %>
<% end %>
```

## Selecting from existing categories

to pock from existing categories we can use [Collection Select](http://apidock.com/rails/ActionView/Helpers/FormOptionsHelper/collection_select) helper to render a `<select>` tag:

```erb
<%= form_for @post do |f| %>
  <%= f.collection_select :category_name, Category.all, :name, :name %>
  <%= f.text_field :content %>
  <%= f.submit %>
<% end %>
```

This will make a drop down menu for the user to pick from. Though now our users can no longer create a new category

we can create autocompletion, which using a [`datalist`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/datalist):

```erb
<%= form_for @post do |f| %>
  <%= f.text_field :category_name, list: "categories_autocomplete" %>
  <datalist id="categories_autocomplete">
    <% Category.all.each do |category| %>
      <option value="<%= category.name %>">
    <% end %>
  </datalist>
  <textarea name="post[content]"></textarea>
  <%= f.submit %>
<% end %>
```

`datalist` is a new element in the HTML5 spec that allows for easy autocomplete. Check below in [Resources](https://learn.co/tracks/online-software-engineering-structured/rails/associations-and-rails/forms-and-basic-association#resources) for further reading.

## Updating multiple rows

The reverse association

```ruby
# app/models/category.rb
class Category < ActiveRecord::Base
  has_many :posts
end
```

Users can't specify many different posts to categorize with just `<select> ` because we can have many posts in that category

### Using array parameters

Rails uses a [naming convention](http://guides.rubyonrails.org/v3.2.13/form_helpers.html#understanding-parameter-naming-conventions) to let you submit an array of values to a controller.

If you put this in a view, it looks like this.

```erb
<%= form_for @category do |f| %>
  <input name="category[post_ids][]">
  <input name="category[post_ids][]">
  <input name="category[post_ids][]">
  <input type="submit" value="Submit">
<% end %>
```

when submitted controller will have access to a post_ids param, which will be an array of strings

we can write a setter method for this, just like we did for `CATEGORY_NAME:`

```RUBY
# app/models/category.rb
class Category < ActiveRecord::Base
   def post_ids=(ids)
       #ids is and array wich is accessed in our controller useing the param, wich is and array of strings
     ids.each do |id|
       post = Post.find(id)
         #seaches throughthe Post-obj_ids  to find all the ids that match the elements in our ids-array
       self.posts << post
         #then shovels them into the Category.posts CollectionProxy wich is similar to an array
     end
   end
end
```

```ruby
# app/controllers/categories_controller.rb
class CategoriesController < ApplicationController
  def create
    Category.create(category_params)
      #now when create is used post_ids= will intercept the call to DB so instead of using AR
  end
 
  private
 
  def category_params
    params.require(:category).permit(:name, post_ids: [])
  end
end
```

