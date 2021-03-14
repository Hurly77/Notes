### URL As Data

Representational state transfer (**REST**) is a software architectural style 

## routes

things we could do

```ruby
 get 'authors/:id/posts'
  get 'authors/:id/posts/:post_id'
  #The above won't work because we have to be explicity with controller actions
  

  #this is how it is done, ...son
  get 'authors/:id/posts', to: 'authors#posts_index'
#here we say get the url, authors, with id = to(passing id), and the posts that belong to that author, got to authors controller and run posts_index action
  get 'authors/:id/posts/:post_id', to: 'authors#post'
```

**another way would be** to nest the routes.

We can still do things to the nested resources that we do to a non-nested resource, like limit them to only certain actions. In this case, we only want to nest `:show` and `:index` under `:authors`.

```ruby
# config/routes.rb
 
Rails.application.routes.draw do
 
  resources :authors, only: [:show] do
    # nested resource for posts
    resources :posts, only: [:show, :index]
  end
 
  resources :posts, only: [:index, :show, :new, :create, :edit, :update]
 
  root 'posts#index'
end
#Now we have the resourced :authors route, but by adding the do...end we can pass it a block of its nested routes.
```

## controllers

**authors**

```ruby
# app/controllers/authors_controller.rb


#all this code will be render useless once we create our posts#index action
  def posts_index
    @author = Author.find(params[:id])
    @posts = @author.posts
    render template: 'posts/index'
   #normally this would be implicit but, we want to leverage th e templates we're already useing for posts, so we call render explicitly with a template path
  end
 
  def post
    @author = Author.find(params[:id])
 
    # Note that because ids are unique by table we can go directly to
    # Post.find — no need for @author.posts.find...
    @post = Post.find(params[:post_id])
    render template: 'posts/show'
  end
```

**WE CAN DELETE** THE ABOVE, because `posts#show` route is going to render the *same* information — data concerning a single post — regardless of whether it is accessed via `/authors/:id/posts/:id` or `/posts/:id`.

**posts**

```ruby
# app/controllers/posts_controller.rb
 
  def index
    if params[:author_id]
        #Rails provides the :author_id for us through the nested route, so we don't have to worry about a collision with the :id parameter that posts#show is looking for.
      @posts = Author.find(params[:author_id]).posts
        #this will return all the post that belong to an author
    else
      @posts = Post.all
        #but if that is not what we are asking for, it will return all the posts
    end
  end
 
  def show
    @post = Post.find(params[:id])
  end
```

### Nested Route URL Helpers

We know the URL is `/authors/:author_id/posts`, so we can combine the two conventions and use `author_posts_path(author_id)`. Remember it's the singular `author` because we are getting one by `id`.

It stands to reason that a single post for an author would combine the conventions for the single author path and single post path, leaving us with `author_post_path(author_id, post_id)`

if you want to double-check your routes, you can run `rake routes`

you can also view in you browser anytime with `rails/info/routes` If you add `_path` or `_url` to any of the names under "Prefix", you'll have the helper for that route.

you can `grep` the output of any command to search for what you want. So in the example above, if you just wanted to search for routes related to authors, you could type `rake routes | grep authors` to get a filtered list.

## views

we already show the author's name, so let's add a link to the list of the author's posts:

```erb
<!-- app/views/posts/index.html.erb -->
 
  ...
 
  <h2><%= post.title %></h2>
 
  <!-- change the name to a link -->
  <h3>by: <%= link_to post.author.name, author_posts_path(post.author) %></h3>
  <p><%= post.description %></p>
 
  ...
```

### Caveat on Nesting Resources More Than One Level Deep

You can nest resources more than one level deep, but that is generally a bad idea.

Imagine if we also had comments in this blog. This would be a perfectly fine use of nesting:

```ruby
resources :posts do
  resources :comments
end
```

We could then link to a post's comments with `post_comments_path` or `/posts/1/comments`. That makes a lot of sense.

But if we then tried to add to our already nested `posts` resource...

```ruby
resources :authors do
  resources :posts do
    resources :comments
  end
end
```

Now we're getting into messy territory. Our `comments_path` helper is now `author_post_comments_path`, our URL is `/authors/1/posts/1/comments`, and we have to handle that filtering in our controller.

But if we lean on our old friend Separation of Concerns, we can conclude that a post's comments are not the concern of an author and therefore don't belong nested two levels deep under the `:authors` resource.

In addition, the reason to put the ID of the resource in the URL is so that we have access to it in the controller. If we know we have the post with an ID of `1`, we can use our Active Record relationships to call:

```ruby
  @post = Post.find(params[:id])
  @post.author # This will tell us who the author of the post was! We don't need this information in the URL
```

