## Creating a Dynamic Web App

lets create a simple slots game.

```ruby
#app/application.rb
class Application
 
  def call(env)
    resp = Rack::Response.new
    resp.write "Hello, World"
    resp.finish
  end
 
end
```

the run command **rackup config.ru** rackup config.ru

 [Kernel](http://ruby-doc.org/core-2.3.0/Kernel.html) is a module that holds many of Ruby's most useful bits. We're just using it here to generate some random numbers.

```ruby
num_1 = Kernel.rand(1..20)
num_2 = Kernel.rand(1..20)
num_3 = Kernel.rand(1..20)

#Then, to check to see if you won or not, we'd have an if statement like this:
num_1 = Kernel.rand(1..20)
num_2 = Kernel.rand(1..20)
num_3 = Kernel.rand(1..20)
 
if num_1==num_2 && num_2==num_3
  puts "You Win"
else
  puts "You Lose"
end
```

The only parts that require a command line are the two `puts` lines. All that needs to change to adapt this for the web is a different way than `puts` to express output to our user. Because this is the web, that means adding it to our response. Instead of `puts` now we'll use the **#write** method in our **Rack::Response** object. **modify it to look like this:**

```ruby
class Application
 
  def call(env)
    resp = Rack::Response.new
 
    num_1 = Kernel.rand(1..20)
    num_2 = Kernel.rand(1..20)
    num_3 = Kernel.rand(1..20)
 
    if num_1==num_2 && num_2==num_3
      resp.write "You Win"
    else
      resp.write "You Lose"
    end
       resp.finish
  end
 
end
#The #write method can be called many times to build up the full response. The response isn't sent back to the user until #finish is called
```

