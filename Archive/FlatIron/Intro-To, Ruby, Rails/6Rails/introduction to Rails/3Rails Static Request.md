```ruby
#config/routs.rb
Rails.application.routes.draw do
  get 'hello_world', to: 'static#hello_world'
    #http verb "/path" to: "controller#action"
end

```

```ruby
#app/controllers/static_controller
class StaticController < ApplicationController
  def hello_world
    'hello_world'
  end
end
```

```erb
<!--app/views/static/hello_world-->
<h1>Hello World</h1>
```

