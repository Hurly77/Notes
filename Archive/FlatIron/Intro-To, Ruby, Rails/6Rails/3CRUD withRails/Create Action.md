```ruby
post = Post.new
post.title = "Title Goes Here"
post.description = "Desc goes here..."
post.save
```

This syntax will let you manually create a new `Post` record with `title` and `description` attributes. 

```ruby
def create
  post = Post.new
  post.title = params[:title]
  post.description = params[:description]
  post.save
end
```

**Remember**, Rails tries to map each controller action directly to a template. However, with actions like `create`, we don't want a view template –– all we want is for the action to communicate with the database and then redirect to a different page.

```ruby
def create
  @post = Post.new
  @post.title = params[:title]
  @post.description = params[:description]
  @post.save
  redirect_to post_path(@post)
end
```

In this refactored `create` action, we're following the convention of redirecting to the new resource's `show` page. 