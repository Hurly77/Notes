## **Rails URL Helpers**

- **Hard-coded path:** `"/posts/#{@post.id}"`

Here you're saying: "I know exactly the GPS coordinates of my meeting, driver. Do exactly as I say."

- **Route helper:** `post_path(@post)`

Here you're saying: "Can you find the best way to a controller that knows how to work with this thing called a `Post` based on looking at this instance called `@post`.

It's more natural to be able to pass arguments into a method as opposed to using string interpolation. For example, `post_path(post, opt_in: true)` is more readable than `"posts/<%= post.id %>?opt_in=true"`

## Implementing Route Helpers

The route call looks like this:

```ruby
# config/routes.rb
resources :posts, only: [:index, :show]
```

This will create routing methods for posts that we can utilize in our views and controllers.

I terminal put `rails routes`

```irb
C1		C2        C3				C4
posts   GET  /posts(.:format)       posts#index
post    GET  /posts/:id(.:format)   posts#show
```

- **Column 1** - This column gives the **prefix for the route helper methods**(*accentually this means that when you say "resources :posts," **you are telling rails that the method is called posts***). In the current application, `posts` and `post` are the prefixes for the methods that you can use throughout your applications. The two most popular method types are `_path` and `_url`. So if we want to render a link to our posts' index page, the method would be `posts_path` or `posts_url`. **The difference between** `_path` and `_url` is that `_path` gives the relative path and `_url` renders the full URL. If you open up the rails console, by running `rails console`, you can test these route helpers out. Run `app.posts_path` and see what the output is. You can also run `app.posts_url` and see how it prints out the full path instead of the relative path. **In general, it's best to use the `_path` version so that nothing breaks if your server domain changes**
- **Column 2** - This is the HTTP verb(GET)
- **Column 3** - This column shows what the path for the route will be and what parameters need to be passed to the route. As you may notice, the second row for the show route calls for an ID. When you pass the `:show` argument to the `resources` method, it will automatically create this route and assume that you will need to pass the `id` into the URL string. Whenever you have `id` parameters listed in the path like this, you will need to pass the route helper method an ID, so an example of what our show route code would look like is `post_path(@post)`. Notice how this is different than the `index` route of `posts_path`. Also, you can ignore the `(.:format)` text for now. If you open up the Rails console again, you can call the route helpers. If you have a `Post` with an `id` of `3`, you can run `app.post_path(3)` and see what the resulting output is. Running route helpers in the rails console is a great way of testing out routes to see what their exact output will be
- **Column 4** - This column shows the controller and action with a syntax of `controller#action`

## The `link_to` Method

```erb
<% @posts.each do |post| %>
  <div><a href='<%= "/posts/#{post.id}" %>'><%= post.title %></a></div>
<% end %>
```

```erb
<% @posts.each do |post| %>
  <div><%= link_to post.title, post_path(post) %></div>
<% end %>
```

We're using the **link_to** method to automatically create an HTML a tag.
#Rails is smart enough to know that if you pass in the post object as an argument, it should use the ID attribute

## Using the :as option

If for any reason you don't like the naming structure for the methods or paths, you can customize them quite easily. A common change is to customize the path users go to in order to register for a site.

If we had a `User` model/controller, in `routes.rb` file, you would add the following line:

```
get '/users/new', to: 'users#new', as: 'register'
```

Now the application lets programmers use `register_path` when creating links with `link_to`. Rails leverages routes and these "helper route" names in many places to help you keep your code flexible and brief.