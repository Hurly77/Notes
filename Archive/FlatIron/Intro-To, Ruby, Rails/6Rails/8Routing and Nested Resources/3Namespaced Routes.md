#### Blog Stats

We are building a blog statistics

here are our routes

```ruby
# config/routes.rb
 
get '/stats', to: 'stats#index'
#This could work but, we want to only allow admins
```

```ruby
# config/routes.rb
 
get '/admin/stats', to: 'stats#index'
#Now, we have to go to admin befor we can get to stats
```

#### Scoping Routes

However, there is a problem here too:

```ruby
get '/admin/stats', to: 'stats#index'
get '/admin/authors/new', to: 'authors#new'
get '/admin/authors/delete', to: 'authors#delete'
get '/admin/authors/create', to: 'authors#create'
get '/admin/comments/moderate', to: 'comments#moderate'
#Now our routes look ugly, plus our code is reapting and this is not dry.
```

We can use scope to allow us to prefix a block of routes under one grouping.

```ruby
# config\routes.rb
 
scope '/admin' do
  resources :stats, only: [:index]
end
# Now our route is resourced. so we won't have to prefix admin again
```

#### Scoping With Modules

Scoping works well enough to group our URLs together but, it will inevitably get harder to keep track of which controllers for regular blog functions and which are for admin functions.

if we add an `/admin` directory to `/controllers` it will be alot easier to keep track of our admin functions use `mkdir app/controllers/admin` to move our controller we can use `mv app/controllers/stats_controller.rb` `app/controllers/admin`

when a new folder under `/controllers` is created, Rails automatically picks it up as a module and expect you to namespace the controller. to modify

```ruby
# controllers/admin/stats_controller.rb
 
class Admin::StatsController < ApplicationController
  def index
 
    ...
 
  end
end
```

Now, the views have to match the controllers. new directory `app/views/admin/stats` then move `stats/index.html.erb` into it so it will look like `/app/views/admin/stats/index.html.erb`

**top-tip:** The `views` folder for a controller module (in this case `/admin`) expects a subfolder structure that matches the names of the controllers (in this case `/admin/stats`).

```ruby
# config/routes.rb
 #oute with a module and use that module's name as the URL
scope '/admin', module: 'admin' do
#here we tell scope /admin is our prefix. then we say that the routes will be handled by controllers in the admin module
  resources :stats, only: [:index]
end
# you will be returned an error unless you follow the above
```

#### Namespace

We can use a **shortcut**, to use module for our our URL name we can use `namespace` 

```ruby
# config/routes.rb
 
namespace :admin do
  resources :stats, only: [:index]
end
```

**Top-tip:** There is one important difference between `scope '/admin', module: 'admin'` and `namespace :admin`, and it's in the URL helpers. Remember above that using `scope` gave us a `stats_path` helper. But now that we are using `namespace`, run `rake routes` again. You'll see that the helper is now prefixed with `admin_`, so `stats_path` becomes `admin_stats_path`. If you switch from `scope` to `namespace`, take care to update any URL helpers you have in use!

