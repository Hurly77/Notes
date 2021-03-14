## Validations

1. Add validations to `Author`

   such that...

   1. `name` is not blank
   2. `email` is unique
   3. `phone_number` is exactly 10 digits long

2. Add validations to `Post`

    

   such that...

   1. `title` is not blank
   2. `content` is at least 100 characters long
   3. `category` is either `"Fiction"` or `"Non-Fiction"`

## Basic Routes & Controllers

1. Create controllers for both models.
2. Create `show`, `new`, `edit`, `create`, and `update` routes for both models.
3. Define controller actions for `show`, `new`, and `edit`.
4. Define the "valid path" for the `create` and `update` controller actions.
5. Define the "invalid path" for the `create` and `update` controller actions.

## Forms

1. Create forms with `form_tag` for both models' `new` and `edit` actions.

2. Prefill already-submitted forms with the invalid data when re-rendering.

3. Display a list of errors at the top of forms when an invalid action is attempted. They should be contained in an element with id `error_explanation`, and each error should have its own `<li>`.

4. Conditionally wrap each field in a `.field_with_errors` div if it has errors.

   ### routes

   ```ruby
   Rails.application.routes.draw do
     resources :authors
     resources :posts
     # For details on the DSL available within this file, see http://guides.rubyonrails.org/routing.html
   end
   ```

   ### model

```ruby
class Author < ActiveRecord::Base
  validates :name, presence:true
  validates :email, uniqueness: true
  validates :phone_number, length: {is: 10}
end
```

```ruby
class Post < ActiveRecord::Base
  validates :title, presence: true
  validates :content, length: { minimum: 100}
  validates :category, inclusion: {in: %w(Fiction Non-Fiction)}
end
```

### controllers

```ruby
class AuthorsController < ApplicationController
  def show
    @author = Author.find(params[:id])
  end

  def new
    @author = Author.new
  end

  def create
    @author = Author.new(author_params)
    
    if @author.valid?
      @author.save

    redirect_to author_path(@author)
    else
      render :new
    end
  end

  private

  def author_params
    params.permit(:name, :email, :phone_number)
  end
```

```ruby
class PostsController < ApplicationController
  def show
    @post = Post.find(params[:id])
  end

  def edit
    @post = Post.find(params[:id])
  end

  def update
    @post = Post.find(params[:id])
    @post.update(post_params)

    if @post.valid?
      

    redirect_to post_path(@post)
  else
    render :edit
    end
  end

  private

  def post_params
    params.permit(:title, :category, :content)
  end
end
```

### views

##### posts/edit

```erb
<h2>Editing "<%= @post.title %>"</h2>
<%= form_tag post_path(@post), method: "patch" do %>

<% if @post.errors.any? %>
    <div id="error_explanation">
      <h2>There were some errors:</h2>
      <ul>
        <% @post.errors.full_messages.each do |message| %>
          <li><%= message %></li>
        <% end %>
      </ul>
    </div>
  <% end %>

  <div class="field<%= ' field_with_errors' if @post.errors[:title].any? %>">
    <%= label_tag "title", "Title" %>
    <%= text_field_tag "title", @post.title %>
  </div>

  <div class="field<%= ' field_with_errors' if @post.errors[:category].any? %>">
    <%= label_tag "category", "Category" %>
    <p>Must be either "Fiction" or "Non-Fiction".</p>
    <%= text_field_tag "category", @post.category %>
    <p>
      Please type carefully as our top scientists are working around the clock to
      enable state-of-the-art dropdown technology for this form field.
    </p>
  </div>

  <div class="field<%= ' field_with_errors' if @post.errors[:content].any? %>">
    <%= label_tag "content", "Content" %>
    <br />
    <%= text_area_tag "content", @post.content %>
  </div>
  <%= submit_tag "Update" %>
<% end %>
```

##### author/edit

```erb
<h2>Create an Author</h2>
<%= form_tag authors_path do %>
  <% if @author.errors.any? %>
  <div id="error_explanation">
    <h2>There were some errors:</h2>
    <ul>
      <% @author.errors.full_messages.each do |message|%>
        <li><%= message %></li>
      <% end %>
    </ul>
  </div>
<% end %>

  <div class="field<%= ' field_with_errors' if @author.errors[:name].any?%>">
    <%= label_tag "name", "Name" %>
    <%= text_field_tag "name", @author.name %>
  </div>

  <div class="field<%= ' field_with_errors' if @author.errors[:email].any? %>">
    <%= label_tag "email", "Email" %>
    <%= text_field_tag "email", @author.email %>
  </div>

  <div class="field<%= ' field_with_errors' if @author.errors[:phone_number].any? %>">
    <%= label_tag "phone_number", "Phone Number" %>
    <%= text_field_tag "phone_number", @author.phone_number %>
  </div>

  <%= submit_tag "Create" %>
<% end %>

```

