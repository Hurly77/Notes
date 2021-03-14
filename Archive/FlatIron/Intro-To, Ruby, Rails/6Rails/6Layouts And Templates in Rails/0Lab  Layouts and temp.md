```ruby
class StaticController < ApplicationController
  def home
  end
end
```

```ruby
class StoreAdminController < ApplicationController
  layout "admin"

  def home
  end

  def orders
    render :layout => "order_administration"
  end

  def invoice
    render :layout => false
  end
end
```

## views

#### layouts 

layouts/admin

```erb
<!DOCTYPE html>
<html>
<head>
</head>
<body>
    <h1>Flatiron Widgets: Admin</h1>
    <%= yield %>    
</body>
</html>
```

layouts/application

```erb
<!DOCTYPE html>
<html>
<head>
  <title>Flatiron Store</title>
</head>
<body>
    <h1>Flatiron Widgets Store</h1>
    <%= yield %>    
</body>
</html>
```

layouts/order_administration

```erb
<!DOCTYPE html>
<html>
<head>
  <title>Flatiron Store</title>
</head>
<body>
    <h1>Flatiron Widgets: Open Orders</h1>
    <%= yield %>
    <!--when you render a template for an action with out specifying a different layout to use tails will use the layout found at this location app/views/layouts/application.html.erb-->
</body>
</html>
```

#### static

static/home

```erb
<h2>Welcome to Flatiron Widgets</h2>
```

#### store_admin

store_admin/home

```erb
<h2>Welcome Flatiron Admin</h2>
```

store_admin/invoice

```erb
<h1>Your Invoice</h1>
```

store_admin/orders

```erb
<h2>Welcome to Flatiron Open Orders</h2>
<ol>
<li>bark plus</li>
<li>man soap</li>
<li>teeth paste</li>
<li>orange peel</li>
</ol>
```

### routes

```ruby
Rails.application.routes.draw do
  get 'home', to: 'static#home'
  get 'admin/home', to: 'store_admin#home'
  get 'admin/orders', to: 'store_admin#orders'
  get 'admin/invoice', to: 'store_admin#invoice'
end
```

