## routes

```ruby
Rails.application.routes.draw do
   resources :authors, only: [:show, :index] do
    resources :posts, only: [:show, :index, :new, :edit]
  end
  resources :posts
end
#This gives us access to /authors/:author_id/posts/new, and a new_author_post_path helper.

```

**Top-tip:** Remember to run `rake routes` if you're unsure of the URL helper name.

## controllers

###### post_controller/edit action

```ruby
  def edit
    if params[:author_id]
      author = Author.find_by(id: params[:author_id])
      if author.nil?
        redirect_to authors_path, alert: "Author not found."
      else
        @post = author.posts.find_by(id: params[:id])
        redirect_to author_posts_path(author), alert: "Post not found." if @post.nil?
      end
    else
      @post = Post.find(params[:id])
    end
  end
```

Here we're looking for the existence of `params[:author_id]`, which we know would come from our nested route. If it's there, we want to make sure that we find a valid author. If we can't, we redirect them to the `authors_path` with a `flash[:alert]`.

If we do find the author, we next want to find the post by `params[:id]`, but, instead of directly looking for `Post.find()`, we need to filter the query through our `author.posts` collection to make sure we find it in that author's posts. It may be a valid post `id`, but it might not belong to that author, which makes this an invalid request.

###### post_controller/new_action

```ruby
def new
    if params[:author_id] && !Author.exists?(params[:author_id])
      redirect_to authors_path, alert: "Author not found"
    else
      @post = Post.new(author_id: params[:author_id])
    end
  end
```

Here we check for `params[:author_id]` and then for `Author.exists?` to see if the author is real.

Why aren't we doing a `find_by` and getting the author instance? Because we don't need a whole author instance for `Post.new`; we just need the `author_id`. And we don't need to check against the `posts` of the author because we're just creating a new one. So we use `exists?` to quickly check the database in the most efficient way.

But what if `params[:author_id]` is `nil` in the example above? If we just did `Post.new` without the `(author_id: params[:author_id])` argument, the `author_id` attribute of the new `Post` would be initialized as `nil` anyway. So we don't have to do anything special to handle it. It works for us if there is or isn't an `author_id` present.

###### posts_controller

```ruby
class PostsController < ApplicationController

  def index
    if params[:author_id]
      @posts = Author.find(params[:author_id]).posts
    else
      @posts = Post.all
    end
  end

  def show
    if params[:author_id]
      @post = Author.find(params[:author_id]).posts.find(params[:id])
    else
      @post = Post.find(params[:id])
    end
  end

  def create
    @post = Post.new(post_params)
    @post.save
    redirect_to post_path(@post)
  end

  def update
    @post = Post.find(params[:id])
    @post.update(post_params)
    redirect_to post_path(@post)
  end

  private

  def post_params
    params.require(:post).permit(:title, :description, :author_id)
  end
end

```

## views

###### posts/_form

```erb
<%= form_for(@post) do |f| %>
<%= author_id_field(@post) %>
  <label>Post title:</label><br>
  <%= f.hidden_field :author_id %>
  <%= f.text_field :title %><br>

  <label>Post Description</label><br>
  <%= f.text_area :description %><br>

  <%= f.submit %>
<% end %>
```

###### posts/show

```erb
<h1><%= @post.title %></h1>
<p>by <%= link_to @post.author.name, author_path(@post.author) if @post.author %> (<%= link_to "Edit Post", edit_author_post_path(@post.author, @post) if @post.author %>)</p>
<p><%= @post.description %> </p>
```

Now we should have a selector when we browse to `/posts/new` and a hidden `author_id` field when we browse to `/authors/1/posts/new`.

## helpers

```ruby
module PostsHelper
  def author_id_field(post)
    if post.author.nil?
      select_tag "post[author_id]", options_from_collection_for_select(Author.all, :id, :name)
    else
      hidden_field_tag "post[author_id]", post.author_id
    end
  end
end
```

we want to be able to select an author at the time of posting **if we haven't** used the nested route.

Since we're already set up to handle `author_id` on the controller, all we have to do is create a helper to use in the partial to present a list of authors when none is present.