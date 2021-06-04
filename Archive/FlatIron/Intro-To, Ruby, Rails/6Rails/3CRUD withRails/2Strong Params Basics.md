## Strong Params

IF we Disable Strong params then we can go in to the html document and update the edit form the the browser too. 

```erb
<br>
<label>Description:</label>
<br>
<input type="text" value="malicious description" name="post[description]" id="post_description">
```

That is the problem that strong params were created to fix. We want to make sure that when users submit a form we only let the fields we want get by.

**Rails needs to be told what parameters are allowed** to be submitted through the form to the database.

```ruby
# app/controllers/posts_controller.rb
 
def create
  @post = Post.new(params.require(:post).permit(:title, :description))
    #params must contain a key called "post", and pramas hash is allowed to have a key called "title/description"
  @post.save
  redirect_to post_path(@post)
end
 
def update
  @post = Post.find(params[:id])
  @post.update(params.require(:post).permit(:title))
  redirect_to post_path(@post)
end
#If you go and do your nefarious hack again, it won't work. Thwarted!!
```

##### Permit vs. Require

What is the deal with `#permit` vs `#require`? The `#require` method is the **most restrictive**. It means that the `params` that get passed in **must** contain a key called "post". If it's not included then it fails and the user gets an error. The `#permit` method is a bit looser. It means that the `params` hash **may** have whatever keys are in it. So in the `create` case, it may have the `:title` and `:description` keys. If it doesn't have one of those keys it's no problem: the hash just won't accept any other keys.

```ruby
#It's a standard Rails practice to remove code repetition
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
 #Now, both our create and update methods in the posts controller can simply call post_params.
private
 
def post_params
	#abstract the strong parameter call into its own method in the controller:
  params.require(:post).permit(:title, :description)
end
#by creating this post_params method we can simply make one change and both methods will automatically be able to have the proper attributes whitelisted.
```

```ruby
# app/controllers/posts_controller.rb
 
def create
  @post = Post.new(post_params(:title, :description))
  @post.save
  redirect_to post_path(@post)
end
 
def update
  @post = Post.find(params[:id])
  @post.update(post_params(:title))
  redirect_to post_path(@post)
end
 #only permit(s) updates to :title while in update action?
private
 
 
# We pass the permitted fields in as *args;
# this keeps `post_params` pretty dry while
# still allowing slightly different behavior
# depending on the controller action. This
# should come after the other methods
 
def post_params(*args)
  params.require(:post).permit(*args)
end
```

