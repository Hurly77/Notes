#### app.rb

```ruby
require_relative 'config/environment'

class App < Sinatra::Base
configure do
  enable :sessions
  set :session_secret, "hey"
end

get '/' do
    erb :index
  end

  post '/checkout' do
      @sessions = session 
      item = params["item"] #this is in q
      @sessions[:item] = item
    end
end
```

#### index.erb

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>
    <form class="shopping_cart" action="/checkout" method="post">
    <label for="Item">Item: </label><input id="item" type="text" name="item">
    <input id="submit" type="submit" name="submit" value="">
  </body>
</html>

</form>

```

