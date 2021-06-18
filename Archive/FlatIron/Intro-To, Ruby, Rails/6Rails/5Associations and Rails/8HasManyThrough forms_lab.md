Proc - Stands for program random occurrence

It is my belief that when refering to `proc` the we are talking about the functionality of setting a vairble  = to a block of code such as

`My_variable = Obj.each {|obj| obj == attribute}`

`puts My_variable` 

#### Schema

```ruby
ActiveRecord::Schema.define(version: 20160119152016) do

  create_table "categories", force: :cascade do |t|
    t.string   "name"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
  end

  create_table "comments", force: :cascade do |t|
    t.string   "content"
    t.integer  "user_id"
    t.integer  "post_id"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
    t.index ["post_id"], name: "index_comments_on_post_id"
    t.index ["user_id"], name: "index_comments_on_user_id"
  end

  create_table "post_categories", force: :cascade do |t|
    t.integer  "post_id"
    t.integer  "category_id"
    t.datetime "created_at",  null: false
    t.datetime "updated_at",  null: false
    t.index ["category_id"], name: "index_post_categories_on_category_id"
    t.index ["post_id"], name: "index_post_categories_on_post_id"
  end

  create_table "posts", force: :cascade do |t|
    t.string   "title"
    t.string   "content"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
  end

  create_table "users", force: :cascade do |t|
    t.string   "username"
    t.string   "email"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
  end

end
```

#### models

```ruby
class Category < ActiveRecord::Base
  has_many :post_categories
  has_many :posts, through: :post_categories
 end
  

----------------------------------------------------------------
 class Comment < ActiveRecord::Base
  belongs_to :user
  belongs_to :post
    accepts_nested_attributes_for :user, reject_if: :all_blank
     #instead of a Proc will create a proc that will reject a record where all the attributes are blank excluding any value for destroy
end

class Post < ActiveRecord::Base
  has_many :post_categories
  has_many :categories, through: :post_categories
  has_many :comments
  has_many :users, through: :comments
end
----------------------------------------------------------------
    

  def categories_attributes=(categories_attributes)
      #this will intercept the call to active records so that we can customize out mehtod
    categories_attributes.values.each do |cat_attr|
        #we call categories_attributes which is equal to :name attribute but could be more
      if cat_attr[:name].present? #use method present to check if the scalar has a value
        category = Category.find_or_create_by(cat_attr)
          #set a proc I think to obj
        if !self.categories.include?(category)
         #if this this returns false than this will return true
          self.post_categories.build(category: category)
            #then we call the Song.proxycollection for, post_categories
            #since Post has_many post_categories and we build, which allows us to both make and object and display it to the user without recording it to the database.
        end
      end
    end
  end
  #the categories_attriburest=() could also be more dry like this:
    def categories_attributes=(categories_attributes)
    categories_attributes.values.each do |cat_attr|
      if cat_attr[:name].present? && !self.categories.include?(cat_attr)
        self.categories << Category.find_or_create_by(cat_attr)
      end
    end
  end
end
    
    
--------------------------------------------------------------------------------------    
  class PostCategory < ActiveRecord::Base
  belongs_to :post
  belongs_to :category
end

class User < ActiveRecord::Base
  has_many :comments
  has_many :posts, through: :comments
end
```

### views

###### app/views/categories

show:

```erb
<!---->
<h1><%= @category.name %></h1>
<% @category.posts.each do |post| %>
<li><%= link_to post.title, post_path(post) %></li>
<% end %>
```

###### app/views/posts

new:

```erb
<%= form_for(@post) do |f| %>
<%= f.label :title %><br>
<%= f.text_field :title %><br>

<%= f.label :content %><br>
<%= f.text_area :content %><br>

<%= f.collection_check_boxes :category_ids, Category.all, :id, :name %><br>
<!--we don't have to say :post, for this because it is implied in the form and would be redunant and a syntext error, because it would be read as :post, :post-->

<%= f.label "new category" %>
<%= f.fields_for :categories, @post.categories.build do |categories_fields| %><br>
<%= categories_fields.text_field :name  %><br>
<% end  %><br>
<!---->

<%= f.submit %>
<% end %>
```

show:

```erb
<!---->
<h1><%= @post.title %></h1>
<p><%= @post.content %></p>
<ul>
<% @post.categories.each do |cat| %>
<li><%= cat.name %></li>
<% end %>
</ul>

<ul>
  <% @post.users.uniq.each do |user| %>
    <li><%= link_to user.username, user_path(user) %></li>
  <% end %>
</ul>

<% @post.comments.each do |comm| %>
<p><%= comm.user.username %> says: <%= comm.content %></p>
<% end %> <br>

<%= form_for Comment.new do |f| %>
  <%= f.hidden_field :post_id, value: @post.id %> <br>
	<!--This automatically associate a new comment with a post-->
  <%= f.label :comment %> <br>
  <%= f.text_area :content %>
  <br>
  <%= f.label "User" %>
  <%= f.collection_select :user_id, User.all, :id, :username, include_blank: true %>
<!--include blank: will set the default value to blank instead of the first username, also include_blank will allow you to pass a black in-->
  <br>
  <h4>New user</h4>
  <%= f.fields_for :user, @post.users.build do |u| %>
    <%= u.text_field :username %>
  <% end %>
  <br><br>
  <%= f.submit %>
<% end %>
```

###### app/views/users

show:

```erb
<h1><%= @user.username %></h1>
<% @user.comments.each do |comm|%>
<p><%= link_to comm.post.title, post_path(comm.post) %></p>
<% end %>
<!---->
```

