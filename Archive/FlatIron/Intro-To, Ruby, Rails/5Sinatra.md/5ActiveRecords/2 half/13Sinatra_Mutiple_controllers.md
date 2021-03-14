## Separating Out Our Controllers

If you think about a typical **e-commerce application**, **products** model would require a series of routes all **starting with** `/products`, and the **orders** model routes would **start with** `/orders`. `products/1` would represent **requesting** **information** **about** the very **first product**, and `/orders/1` would represent r**equesting** **information** about the **first order**.

Products:

| Request | Route                | CRUD Action |
| ------- | -------------------- | ----------- |
| GET     | '/products/:id'      | Read        |
| GET     | '/products/new'      | Create      |
| POST    | '/products'          | Create      |
| GET     | '/products/:id/edit' | Update      |
| POST    | '/products/:id'      | Update      |
| GET     | '/products'          | Read        |
| POST    | '/products/:id'      | Delete      |

Orders:

| Request | Route              | CRUD Action |
| ------- | ------------------ | ----------- |
| GET     | '/orders/:id'      | Read        |
| GET     | '/orders/new'      | Create      |
| POST    | '/orders'          | Create      |
| GET     | '/orders/:id/edit' | Update      |
| POST    | '/orders/:id'      | Update      |
| GET     | '/orders'          | Read        |
| POST    | '/orders/:id'      | Delete      |

With GET and POST requests, you're looking at a really full and complex controller, with 7 controller actions for each model. 

Just like we **separate** out our different **models** into different files, **we need** to **separate** these domain concepts in our code into separate **controllers**.

 **Single Responsibility Principle** -  only encapsulating logic relating to a singular entity in our application domain. 

### Products Controller

```ruby
#app/controllers/products_controller.rb
class ProductsController < Sinatra::Base
 
  get '/products' do
    "Product Index"
  end
 
  get '/products/:id' do
    "Product #{params[:id]} Show"
  end
 
end
```

### Orders Controller

```ruby
#app/controllers/orders_controller.rb
class OrdersController < Sinatra::Base
 
  get '/orders' do
    "Order Index"
  end
 
  get '/orders/:id' do
    "Order #{params[:id]} Show"
  end
 
end
```

##### Mounting Multiple Controllers in `config.ru`.

```ruby
#config.ru
require 'sinatra'
 
require_relative 'app/controllers/products_controller'
require_relative 'app/controllers/orders_controller'
# our environment must load both controller files.
```





```ruby
config.ru:
require 'sinatra'
 
require_relative 'app/controllers/products_controller'
require_relative 'app/controllers/orders_controller'
 #The other class must be loaded as Middleware. We won't get into MiddleWare now, suffice to say, you simply use it instead of run.
use ProductsController
#Only one class can be specified to be run.
run OrdersController
```

##### important note

Which classes you `use` or `run` matter

## Video Review

**NOTE** these videos appear later in the Sinatra curriculum.

- [Building a Site Generator, Part 1](https://www.youtube.com/watch?v=wXq-Na6mZuk)
- [Building a Site Generator, Part 2](https://www.youtube.com/watch?v=4Nute1F5TZ4)