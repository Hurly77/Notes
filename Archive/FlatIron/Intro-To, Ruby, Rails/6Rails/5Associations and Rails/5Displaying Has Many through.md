### has_many, through

We can **set up** a **many-to-many** relationship **using a join table**.**** **Any** table that contains **two foreign keys** can be **thought of as a join table**

| id   | content              | post_id | user_id |
| ---- | -------------------- | ------- | ------- |
| 1    | "I loved this post!" | 5       | 3       |

#### migrations

```ruby
class CreatePosts < ActiveRecord::Migration
  def change
    create_table :posts do |t|
      t.string :title
      t.string :content
      t.timestamps null: false
    end
  end
end

class CreateUsers < ActiveRecord::Migration
   def change
    create_table :users do |t|
      t.string :username
      t.string :email
      t.timestamps null: false
    end
  end
end

class CreateComments < ActiveRecord::Migration
  def change
    create_table :comments do |t|
      t.string :content
      t.belongs_to :user
      t.belongs_to :post
      t.timestamps null: false
    end
  end
end
```

#### models

```ruby
# app/models/post.rb
 
class Post < ActiveRecord::Base
  has_many :comments
  has_many :users, through: :comments
    #posts table doesn't have a foreign key called user_id. Instead Active Record to look through the comments table to figure out this association by declaring that our User has_many :posts, through: :comments
end

# app/models/user.rb
 
class User < ActiveRecord::Base
  has_many :comments
  has_many :posts, through: :comments
end

# app/models/comment.rb
 
class Comment < ActiveRecord::Base
  belongs_to :user
  belongs_to :post
end
```

#### controllers

```ruby
# app/controllers/posts_controller.rb
 
class PostsController < ApplicationController
 
  def show
    @post = Post.find(params[:id])
  end
end
```

#### views

```erb
 <!--app/views/posts/show.html.erb-->
<h2><%= @post.title %></h2>
<p>
  Content: <%= @post.content %>
</p>
Comments:
  <% @post.comments.each do |comment| %>
    <%= link_to comment.user.username, user_path(comment.user) %> said
<!--We can then call any method that our user responds to, such as username-->
    <%= comment.content %>
<!-- Calling comment.user returns for us the User object associated with that comment-->
  <% end %>

<!--app/views/users/show.html.erb-->
<h2><%= @user.username %> </h2> has commented on the following posts:
 
<% @user.posts.each do |post| %>
<!--simply call the posts method on our user and iterate through.-->
  <%= link_to post.title, post_path(post) %>
<% end %>
```

**Displaying** data via a `has_many, through` relationship looks identical to displaying data through a normal relationship.