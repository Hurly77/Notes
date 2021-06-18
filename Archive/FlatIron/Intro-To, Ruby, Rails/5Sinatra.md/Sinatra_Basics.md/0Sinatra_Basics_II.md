### Modular Sinatra Applications

**For example** - your application might have multiple routes, route-handlers, and configuration. To handle this, Sinatra is more commonly used through the Modular `Sinatra` Pattern (over the classical, single file `app.rb` pattern).

#### config.ru

The purpose of `config.ru` **is** to `detail` to Rack the environment requirements of the application and start the application.

```ruby
require 'sinatra' #we load the Sinatra library
 
require_relative './app.rb' #defined in app.rb
 
run Application #start the application represented by the ruby class Application, which is defined in app.rb.
```

#### Sinatra Controllers: `app.rb`

Controller to `run`. A Sinatra Controller is simply a Ruby `Class` that inherits from `Sinatra::Base`

```ruby
class Application < Sinatra::Base # create a new classinherit from Sinatra::Base
 
  get '/' do
    "Hello, World!"
  end
 
end
```

