## Don't Repeat Yourself (DRY)

**//Flat-fact"** The opposite of DRY is WET, which may or may not stand for "We Enjoy Typing" or "Write Everything Twice."

## Helpers Are Here To, Well, Help

Let's use Separation of Concerns to decide where to make our helper. Displaying the date of the last time the post was updated isn't an application-wide concern; it's only a post concern. So let's put it in the `PostsHelper`.

```ruby
# app/helpers/posts_helper.rb
 
def last_updated(post)
  post.updated_at.strftime("Last updated %A, %b %e, at %l:%M %p")
end
```

Now that we have our helper, let's DRY up those views.

```erb
<!-- app/views/posts/show.html.erb -->
 
<h1><%= @post.title %></h1>
<p><%= last_updated @post %></p>
<p><%= @post.description %></p>

<!-- app/views/posts/index.html.erb -->
 
<% @posts.each do |post| %>
  <div>
    <%= post.title %> - <%= last_updated post %>
  </div>
<% end %>

<!-- app/views/posts/edit.html.erb -->
 
<%= form_for(@post) do |f| %>
  <label><%= last_updated @post %></label><br>
  <label>Post title:</label><br>
  <%= f.text_field :title %><br>
 
  <label>Post Description</label><br>
  <%= f.text_area :description %><br>
 
  <%= f.submit %>
<% end %>
```

## Using Another Controller's Helpers

What do we do if we want to also show the last updated time on that page? The answer is: the same thing we just did!****

```erb
<!-- app/views/authors/show.html.erb -->
 
<h1><%= @author.name %></h1>
<p>Posts:</p>
<% @author.posts.each do |post| %>
  <div><%= post.title %> - <%= last_updated post %></div>
<% end %>
```

**Helpers** are *organized* by controller, but they **aren't *restricted* to a single controller**. Just like you can use a Rails `link_to` helper in any view, you can also use any of your own helpers in any view.

## Application Helpers

things that aren't the concern of a single controller and are applicable to the entire application, we have the `application_helper` In applications where users can log in, the application helper will often have a method to expose a `current_user`

The page `title` isn't strictly the concern of a `Post` or an `Author`, even though it may use data from either of those things. Since it's a broader concern than just one controller/model, it's a prime candidate for the `ApplicationHelper`.

```ruby
# app/helpers/application_helper.rb
 
def title(text)
  content_for :title, text
end 
```

**Note:** We use the Rails `content_for` helper within our custom helper here. Helpers on helpers on helpers! What this will do is send our `text` to the place in our application layout that is expecting some content for the `:title`.

```erb
<!--Now that we have our title helper, we can use it everywhere to change the page title based on the content.-->

<!-- app/views/layouts/application.html.erb -->
 
<head>
  <title><%= yield :title %></title>
</head>
------------------------------------------
<!-- app/views/authors/show.html.erb -->
 
<% title @author.name %>
 
<h1><%= @author.name %></h1>
<p>Posts:</p>
<% @author.posts.each do |post| %>
  <div><%= post.title %> - <%= last_updated post %></div>
<% end %>
----------------------------------------------------------
<!-- app/views/posts/show.html.erb -->
 
<% title @post.title %>
 
<h1><%= @post.title %></h1>
<p><%= last_updated @post %></p>
<p><%= @post.description %></p>
```

You can read more about `content_for` in the [Layouts and Rendering](http://guides.rubyonrails.org/layouts_and_rendering.html#using-the-content-for-method) RailsGuide.