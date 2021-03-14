#### The differences between `form_for` and `form_tag`

The **biggest difference** between these two helpers is that `form_for` creates a form specifically **for** a model object. `form_for` is full of convenient features.

`form_for` automatically performs a route lookup to find the right URL for post.

`form_for` takes a block  It passes an instance of [FormBuilder](http://api.rubyonrails.org/classes/ActionView/Helpers/FormBuilder.html) as a parameter to the block, which is what `f` is below.

A basic implementation looks like this:

```erb
<!-- app/views/posts/edit.html.erb //-->
 
<%= form_for @post do |f| %>
  <%= f.text_field :title %>
  <%= f.text_area :content %>
  <%= f.submit %>
<% end %>

<!--Here's what we would need to do with form_tag to generate the exact same HTML:-->
<!-- app/views/posts/new.html.erb //-->
<%= form_tag post_path(@post), method: "patch", name: "edit_post", id: "edit_post" do %>
  <%= text_field_tag "post[title]", @post.title %>
  <%= text_area "post[content]", @post.content %>
  <%= submit_tag "Update Post" %>
<% end %>
<!--form_tag doesn't know what action we're going to use it for, because it has no model object to check.-->
```

This creates the HTML:

```html
<form class="edit_post" id="edit_post" action="/posts/1" accept-charset="UTF-8" method="post">
  <input name="utf8" type="hidden" value="&#x2713;" />
  <input type="hidden" name="_method" value="patch" />
  <input type="hidden" name="authenticity_token" value="nRPP2OqVKB00/Cr+8EvHfYrb5sAkZRtr8f6dzBaJAI+cMceR0fUatcLWd4zdwYCpojW2J3QLK6uyBKeFAgZvmw==" />
  <input type="text" name="post[title]" id="post_title" value="Existing Post Title"/>
  <textarea name="post[content]" id="post_content">Existing Post Content</textarea>
  <input type="submit" name="commit" value="Update Post" />
</form>
```

## Using `form_for` to generate empty forms

To wire up an empty form in our `new` view, we need to create a blank object:

```ruby
# app/controllers/posts_controller.rb
 
  def new
    @post = Post.new
  end
```

Here's our usual vanilla `create` action:

```ruby
# app/controllers/posts_controller.rb
 
  def create
    @post = Post.create(post_params)
 
    redirect_to post_path(@post)
  end
```

### Re-Rendering With Errors

```ruby
# app/controllers/posts_controller.rb
 
  def create
    @post = Post.new(post_params)
 
    if @post.save
      redirect post_path(@post)
    else
      render :new
    end
  end
```

### Full Messages with Prepopulated Fields

To get some extra verbosity, we can add the snippet from the previous lesson to the top of the form:

```erb
<!-- app/views/posts/new.html.erb //-->
 
<% if @post.errors.any? %>
  <div id="error_explanation">
    <h2>
      <%= pluralize(@post.errors.count, "error") %>
      prohibited this post from being saved:
    </h2>
 
    <ul>
    <% @post.errors.full_messages.each do |msg| %>
      <li><%= msg %></li>
    <% end %>
    </ul>
  </div>
<% end %>
```

Let's look at another nice feature of `FormBuilder`. Here's our `form_for` code again:

```erb
<!-- app/views/posts/edit.html.erb //-->
 
<%= form_for @post do |f| %>
  <%= f.text_field :title %>
  <%= f.text_area :content %>
  <%= f.submit %>
<% end %>

<!--The `text_field` call generates this tag:-->

<input type="text" name="post[title]" id="post_title" value="Existing Post Title"/>

<!--Not only will FormBuilder pre-fill an existing Post object's data, it will also wrap the tag in a div with an error class if the field has failed validation(s):-->
<div class="field_with_errors">
  <input type="text" name="post[title]" id="post_title" value="Existing Post Title"/>
</div>
```

# Recap

`form_for` gives us a lot of power!

Our challenge as developers is to keep track of the different layers of magic that make this tool so convenient. The old adage is true: we're responsible for understanding not only *how* to use `form_for` but also *why* it works. Otherwise, we'll be completely lost as soon as a sufficiently unusual edge case appears.

When in doubt, **read the HTML**. Get used to hitting the "View Source" and "Open Inspector" hotkeys in your browser (`Ctrl-u` and `Ctrl-Shift-i` on Chrome Windows; `Option-Command-u` and `Option-Command-i` on Chrome Mac), and remember that most browsers let you [examine `POST` data in their developer network tools](http://superuser.com/questions/395919/where-is-the-post-tab-in-chrome-developer-tools-network).

