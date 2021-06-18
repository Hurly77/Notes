```ruby
#Under app.rb
require_relative 'config/environment'

class App < Sinatra::Base
  get '/' do
    "Hello, World"
  end

  get '/name'do
    "My name is Cameron"
  end

  get '/hometown'do
      "My hometown is Placerville"
  end

  get '/favorite-song'do
      "My favorite song is Moral Of the Story"
  end
end

```

