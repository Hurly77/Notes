#  Blog Categories

`Post` objects can be created, associated with `Category` objects, and listed by `Category`.

# The Models

```ruby
class Post < ActiveRecord::Base
  belongs_to :category
end
```

```ruby
class Category < ActiveRecord::Base
  has_many :posts
end
```

# Seed Data

```ruby
# db/seeds.rb
 
clickbait = Category.create!(name: "Motivation")
clickbait.posts.create!(title: "10 Ways You Are Already Awesome")
clickbait.posts.create!(title: "This Yoga Stretch Cures Procrastination, Maybe")
clickbait.posts.create!(title: "The Power of Positive Thinking and 100 Gallons of Coffee")
 
movies = Category.create!(name: "Movies")
movies.posts.create!(title: "Top 20 Summer Blockbusters Featuring a Cute Dog")
```

# The Views

## Posts

```erb
<!-- app/views/posts/show.html.erb -->
 
<h1><%= @post.title %></h1>

 <!--@post.category is the Category model itself, so we can use it anywhere we would use @category in a view for that object.-->
<h3>Category: <%= link_to @post.category.name, 

category_path(@post.category) if @post.category %></h3>
<!-- the if ensures that the view doesn't try to call @post.category.name if the post has not been associated with a category.-->

 
<p><%= @post.description %></p>
```

## Categories

In this domain, the primary use of a category is as a bucket for posts

```erb
<!-- app/views/categories/show.html.erb -->
 
<h1><%= @category.name %></h1>
 
<h3><%= pluralize(@category.posts.count, 'Post') %></h3>
<ul>
  <% @category.posts.each do |p| %>
    <li><%= link_to p.title, post_path(p) %></li>
  <% end %>
</ul>
```

The object returned by an association method (`posts` in this case) is a [CollectionProxy](http://edgeapi.rubyonrails.org/classes/ActiveRecord/Associations/CollectionProxy.html)

it responds to **most of the methods** you can use on an **array**. Think of it like an array.

we wrote a loop very similar to the loops we've been writing in `index` actions, which makes sense since a category is essentially an index of its posts.

```erb
<!-- app/views/categories/show.html.erb -->
 
...
 
<% @category.posts.each do |p| %>
  <li><%= link_to p.title, post_path(p) %></li>
<% end %>
 
...

<!-- app/views/posts/index.html.erb -->
 
...
 
<% @posts.each do |p| %>
  <li><%= link_to p.title, post_path(p) %></li>
<% end %>
 
...
<!--In fact, the only difference is what we call each on-->
```

