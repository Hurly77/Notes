## Setting up our Posts and Categories

```ruby
# app/models/post.rb
 
class Post < ActiveRecord::Base
  has_many :post_categories
  has_many :categories, through: :post_categories
end

# app/models/category.rb
 
class Category < ActiveRecord::Base
  has_many :post_categories
  has_many :posts, through: :post_categories
end

# app/models/post_category.rb
 
class PostCategory < ActiveRecord::Base
  belongs_to :post
  belongs_to :category
end
```

```erb
# app/views/posts/_form.html.erb
<!--this will create a checkbox field for each Category in our database--> 
<%= form_for post do |f| %>
  <%= f.label "Title" %>
  <%= f.text_field :title %>
  <%= f.label "Content" %>
  <%= f.text_area :content %>
  <%= f.collection_check_boxes :category_ids, Category.all, :id, :name %>
  <%= f.submit %>
<% end %>

<!--The html will look like this-->
<input type="checkbox" value="1" name="post[category_ids][]" id="post_category_ids_1">
```

n our controller, we've setup our `post_params` to expect a key of `:category_ids` with a value of an array.

```ruby
# app/controllers/post_controller.rb
 
class PostsController < ApplicationController
 
  ...
 
  private
 
  def post_params
    params.require(:post).permit(:title, :content, category_ids:[])
      #we need to set the value of category_ids to and empty array so that is can map through each value in the category_ids proxycollection
  end
end

#our post_params will look something like this
{"title"=>"New Post", "content"=>"Some great content!!", "category_ids"=>["2", "3", ""]}
```

```ruby
def create
  post = Post.create(post_params)
  redirect_to post
end
#this is the sql that will fire off when we create our new post
INSERT INTO "posts" ("title", "content", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["title", "New Post"], ["content", "Some great content!!"], ["created_at", "2016-01-15 21:25:59.963430"], ["updated_at", "2016-01-15 21:25:59.963430"]]
 #First, we're creating a new row in our posts table with title and content.
    
INSERT INTO "post_categories" ("category_id", "post_id", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["category_id", 2], ["post_id", 6], ["created_at", "2016-01-15 21:25:59.966654"], ["updated_at", "2016-01-15 21:25:59.966654"]]
 #Next, we create a row in our post_categories table for each ID number that was stored in our category_ids array
    
INSERT INTO "post_categories" ("category_id", "post_id", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["category_id", 3], ["post_id", 6], ["created_at", "2016-01-15 21:25:59.968301"], ["updated_at", "2016-01-15 21:25:59.968301"]]

```

This functions just like it did with a `has_many` relationship, but, instead of creating a new record in our `categories` table, Active Record is creating two new rows in our `post_categories` table.

## Creating New Categories

The value of the name should be nested under our `post_params`, so we don't have to add too much code to our controller. We can use the `fields_for` helper to do this very easily.

```erb
# app/views/posts/_form.html.erb
 
<%= form_for post do |f| %>
  <%= f.label "Title" %>
  <%= f.text_field :title %>
  <%= f.label "Content" %>
  <%= f.text_area :content %>
  <%= f.collection_check_boxes :category_ids, Category.all, :id, :name %>
  <%= f.fields_for :categories, post.categories.build do
    
    |categories_fields| %>
    <%= categories_fields.text_field :name %>
  <% end %>
<!--The fields_for helper takes two arguments: the associated model that we're creating and an object to wrap around-->
  <%= f.submit %>
<% end %>

<!--this will give us this-->
<input type="text" name="post[categories_attributes][0][name]" id="post_categories_attributes_0_name">
```

Our params hash will now have a key of `:categories_attributes` nested under the key of `post`.

```ruby
# app/controllers/post_controller.rb
 
class PostsController < ApplicationController
 
  ...
 
  private
 
  def post_params
    params.require(:post).permit(:title, :content, category_ids:[], categories_attributes: [:name])
  end
end
#Now, when we do mass assignment, our Post model will call a method called categories_attributes=.
```

```ruby
class Post < ActiveRecord::Base
  has_many :post_categories
  has_many :categories, through: :post_categories
  accepts_nested_attributes_for :categories
    #this macro lets us add the method to our model
 
end
```

```ruby
#we can see that it's creating new instances of PostCategory without us ever having to interact with them.

(0.1ms)  begin transaction
  SQL (0.4ms)  INSERT INTO "posts" ("title", "content", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["title", "A New Post!"], ["content", "It was the best of times, it was the worst of times"], ["created_at", "2016-01-15 22:08:37.271367"], ["updated_at", "2016-01-15 22:08:37.271367"]]
  SQL (0.1ms)  INSERT INTO "categories" ("name", "created_at", "updated_at") VALUES (?, ?, ?)  [["name", "Really Neat!"], ["created_at", "2016-01-15 22:08:37.277421"], ["updated_at", "2016-01-15 22:08:37.277421"]]
  SQL (0.3ms)  INSERT INTO "post_categories" ("post_id", "category_id", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["post_id", 9], ["category_id", 5], ["created_at", "2016-01-15 22:08:37.279564"], ["updated_at", "2016-01-15 22:08:37.279564"]]
   (1.0ms)  commit transaction
```

**Still, there's a problem** We're creating a new category each time, regardless of whether or not it exists.

```ruby
class Post < ActiveRecord::Base
  has_many :post_categories
  has_many :categories, through: :post_categories
  # accepts_nested_attributes_for :categories
 
  def categories_attributes=(category_attributes)
    category_attributes.values.each do |category_attribute|
      category = Category.find_or_create_by(category_attribute)
        #we're only creating a new category if it doesn't already exist with the current name
      self.categories << category
        #First, we call self.categories, which returns an array of Category objects, and then we call the shovel (<<) method to add our newly found or created Category object to the array
    end
  end
end
```

 We could imagine later calling `save` on the `Post` object and this then creating the `post_categories` join record for us. In reality, this is syntactic sugar for the `categories<<` method. That's the actual method name, and behind the scenes it will create the join record for us. It's one of the methods dynamically created for us whenever we use a `has_many` association. The end result is this method doing exactly what Active Record was doing for us before; we're just customizing the behavior a little bit.

## Conclusion/So What?

As you can see, it doesn't really matter how complex our associations are –– Active Record is really good at managing that complexity for us. We can always drop down a level of abstraction if needed to customize the way our application behaves.