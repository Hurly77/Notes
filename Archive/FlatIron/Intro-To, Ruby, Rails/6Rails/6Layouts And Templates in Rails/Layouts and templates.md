## Life Without Layouts

1. A list of products
2. A detail view that shows more info for a selected product
3. A shopping cart

It has a nav component, a list of products, and a footer. It would be the **worst online shop ever**, but let's keep it simple for now.

```html
<!-- app/views/products/index.html.erb -->
 
<!DOCTYPE html>
<html>
<head>
  <title>Flatiron Store</title>
  <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track' => true %>
  <%= javascript_include_tag 'application', 'data-turbolinks-track' => true %>
  <%= csrf_meta_tags %>  
</head>
<body>
    <div class="navigation">
      <ul>
        <a href="/products">Products</a>
        <a href="/cart">Cart</a>
      </ul>
    </div>
 
    <h1>The Product List</h1>
 
    <ul>
      <% @products.each do |job| %>
        <li><%= link_to 'Show', job %></li>
      <% end %>
    </ul>  
 
    <div class="footer">
      <p>This shop promises the lowest prices in widgets!</p>
    </div>    
</body>
</html>
```

```html
<!-- app/views/products/show.html.erb -->
 
<!DOCTYPE html>
<html>
<head>
  <title>Flatiron Store</title>
  <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track' => true %>
  <%= javascript_include_tag 'application', 'data-turbolinks-track' => true %>
  <%= csrf_meta_tags %>  
</head>
<body>
    <div class="navigation">
      <ul>
        <a href="/products">Products</a>
        <a href="/cart">Cart</a>
      </ul>
    </div>
 
    <h1><%= @product.name%></h1>
 
    <p>
      <strong>Category:</strong>
      <%= @product.category %>
    </p>
 
    <p>
      <strong>Price:</strong>
      <%= @product.price %>
    </p>
 
    <div class="footer">
      <p>This shop promises the lowest prices in widgets!</p>
    </div>    
</body>
</html>
```

## Layouts to the Rescue

When you generate a new Rails app, it generates a layout for you.

```html
<!-- app/views/layouts/application.html.erb -->
 
<!DOCTYPE html>
<html>
<head>
  <title>Flatiron Store</title>
  <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track' => true %>
  <%= javascript_include_tag 'application', 'data-turbolinks-track' => true %>
  <%= csrf_meta_tags %>
</head>
<body>
 
<%= yield %>
 
</body>
</html>
```

## Yield

```html
<!-- app/views/layouts/application.html.erb -->
 
<!DOCTYPE html>
<html>
<head>
  <title>Flatiron Store</title>
</head>
<body>
    <h1>Welcome To The Flatiron Store!</h1>
</body>
</html>
```

**Note:** usually you should include links to assets like style sheets and JavaScript files in your layouts, which was omitted in the code above to keep it simple.

Other than the missing links to common assets, this layout is missing something **terribly important**. To see what it is, have a look at the following example that uses this layout.

example of an action (`static#about`) where we expect the corresponding template (`app/views/static/about.html.erb`) to render within the `application.html.erb` layout defined above.

```ruby
# app/controllers/static_controller.rb
 
class StaticController < ApplicationController
  def about
  end
end
```

There should also be an associated route in the `config/routes.rb` file to route a request to `/about` to the `static#about` action in the `StaticController`,

```ruby
# config/routes.rb
 
Rails.application.routes.draw do
  get 'about', to: 'static#about'
end
```

And this is the template for the `static#about` action with a simple message, which we would want to display nested inside the `application.html.erb` layout:

```html
<!-- app/views/static/about.html.erb -->
 
<p>Hello!</p>
```

static#about will greet **Welcome To The Flatiron Store!**  but you **won'**t see the "Hello!"

 because the layout file at `app/views/layouts/application.html.erb` does not have a `yield` statement in it. `yield` is what Rails uses to decide where in the layout to render the content for the action. If you don't put a `yield` in your layout, the layout itself will render just fine, but any additional content coded into the action templates will not be correctly placed within the layout.

To fix this issue, add a `yield` to the layout file at `app/views/layouts/application.html.erb`:

```html
<!-- app/views/layouts/application.html.erb -->
 
<!DOCTYPE html>
<html>
<head>
  <title>Flatiron Store</title>
</head>
<body>
    <h1>Welcome To The Flatiron Store!</h1>
    <%= yield %>    
</body>
</html>
```

Now when you hit up the `static#about` route in your browser, you will see **Welcome To The Flatiron Store!** followed by **Hello!** This means that, when the layout rendered, it pulled the action's specific template into the correct place

## How Layouts and Templates are Stitched Together

At its simplest level, this is what happens when a request is made to your Rails application:

1. Rails finds the template for the corresponding action based either on convention or any other options passed to the `render` method in your controller action.
2. Similarly, it then finds the correct layout to use, either through naming/directory conventions or from specific options that you provided.
3. Rails uses the action template to generate the content specific to the action. (Note that the template might be composed of partial views, which you'll learn about a bit later.)
4. It then looks for the layout's `yield` statement and inserts the action's template there.

## How Rails Decides Which Layout to Use

### Deciding on a Layout Through Convention

Rails uses a simple convention to find the correct layout for your request. If you have a controller called `ProductsController`, Rails will see whether there is a layout for that controller at `layouts/products.html.erb`. Similarly, if you have a controller called `AdminController`, it'll look for a layout at `layouts/admin.html.erb`. If it can't find a layout specific to your controller, it'll use the default layout at `app/views/layouts/application.html.erb`.

### Overriding Conventions

1. If you want to use the products layout for every action, simply specify that you want to use the products layout by invoking the `layout` method in your controller, passing it a string that it can use to find the desired layout:

```ruby
  class ShoppingCartController < ApplicationController
    layout "products"
  end
```

If you want to use the products layout only for a particular action, simply use the `render` method in the controller action, specifying the layout you want it to use like this:

```ruby
 class ShoppingCartController < ApplicationController
    def list
      render :layout => "static"
    end
 
    # the rest of the actions will use standard conventions
  end
```

If you want to render your action template without a layout, you can do the following:

```ruby
class ShoppingCartController < ApplicationController
  def list
    render :layout => false
  end
 
  # the rest of the actions will use standard conventions
end
```

**Note:** It's pretty unusual to not render the layout in a standard action. However, once you start using AJAX (JavaScript), it's quite common. Keep this in the back of your mind when you get to JavaScript.