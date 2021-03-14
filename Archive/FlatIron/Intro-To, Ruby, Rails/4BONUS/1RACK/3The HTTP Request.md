#### **HTTP Requests** 

An HTTP Request that our browser sends to the server contains two main sections of information. One is headers and the other section is the resource or path requested (for example, `/search` or `/profile_name`)

#### The Path

The **path** that is **requested** is the **resource** that the client **wants**. **path** signifies which specific **part** of your **server** it **wants**. **EX** Shopping cart app

| Path   | Description              |
| ------ | ------------------------ |
| /items | List all items available |
| /cart  | List items in cart       |

The path lives in the HTTP request, and to get to it we have to inspect the `env` part of our `#call` function.  It looks like this

```ruby
class Application
 
  def call(env)
    resp = Rack::Response.new
    req = Rack::Request.new(env)
    resp.finish
  end
 
end
```

We can use the method **#lpath** this will return the requested path. 

```ruby

class Application
 
  @@items = ["Apples","Carrots","Pears"]
 
  def call(env)
    resp = Rack::Response.new
    req = Rack::Request.new(env)
 
    @@items.each do |item| # this will show the user all items 
      resp.write "#{item}\n"
    end
 
    resp.finish
  end
end
```

Let's filter so that this only works for the `/items` path using the `#path` method of our `Rack::Request` object:

```ruby
class Application
 
  @@items = ["Apples","Carrots","Pears"]
 
  def call(env)
    resp = Rack::Response.new
    req = Rack::Request.new(env)
 
    if req.path.match(/items/)
      @@items.each do |item|
        resp.write "#{item}\n"
      end
    else
      resp.write "Path Not Found"
    end
 
    resp.finish
  end
end
```

### User Input Via The Path

`https://github.com/search?q=apples`. We have a domain of `github.com`, path of `search`, and then a `?` character. After that character comes `q=apples`. There is our search, 

 The section after the `?` is called the `GET` parameters. If you notice in the above example, `GET` params come in key/value pairs. The key in GitHub's case would be `q` and the value is `apples`

```ruby
#If we wanted to implement a /search route that accepted a GET param with the key q it would look something like this:
class Application
 
  @@items = ["Apples","Carrots","Pears"]
 
  def call(env)
    resp = Rack::Response.new
    req = Rack::Request.new(env)
 
    if req.path.match(/items/)
      @@items.each do |item|
        resp.write "#{item}\n"
      end
    elsif req.path.match(/search/)
 
      search_term = req.params["q"]
 
      if @@items.include?(search_term)
        resp.write "#{search_term} is one of our items"
      else
        resp.write "Couldn't find #{search_term}"
      end
 
    else
      resp.write "Path Not Found"
    end
 
    resp.finish
  end
end
```

