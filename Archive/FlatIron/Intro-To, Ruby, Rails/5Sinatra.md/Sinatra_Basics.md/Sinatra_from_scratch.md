### Sinatra Gem

**NOTE:***gems have newer versions. **bundle install** will **lock** in the current versions of the gems for your application. That way, if any **updates** happen, your app **won't break**. It keeps the versions **locked** **in** a file called **Gemfile.lock** that is created for you.

### **app.rb** 

The **app.rb** file is the heart and soul of a Ainatra application. This **controls** the application

```ruby
require 'sinatra'
class App < Sinatra::Base

  get '/' do 
    "Hello, world!"
  end

end
```

Inside The class above We have a **Sinatra method** This method responds to a `GET` request to the root url and displays the text Hello, World! in the browser.