1. Create a new class array called `@@cart` to hold any items in your cart
2. Create a new route called `/cart` to show the items in your cart
3. Create a new route called `/add` that takes in a `GET` param with the key `item`. This should check to see if that item is in `@@items` and then add it to the cart if it is. Otherwise give an error

```ruby
#MY CODE
	if @@cart.empty? # if the cart is empty
          resp.write "Your cart is empty" #respond to the user with
        else #other wise
          @@cart.each do |item| #give every value in the @@ cart name item
            resp.write "#{item}\n" #then responde to the user "that item"
          end
        end
      elsif req.path.match(/add/) # other wise if the requested path matches /add/
        search_term = req.params["item"] #set search term to equal requested parameter KEY as "item"
      if @@items.include?(search_term) # if @@items array includes the search term 
        @@cart << search_term # place the search term into the cart
        resp.write "added #{search_term}" #responde to the user with added search term
      else
        resp.write "We don't have that item" #otherwise 
        end
```

```ruby
class Application
  @@cart = []
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
      resp.write handle_search(search_term)
        
   #MY CODE WENT HERE

    else
      resp.write "Path Not Found"
  end

    resp.finish
  end

  def handle_search(search_term)
    if @@items.include?(search_term)
      return "#{search_term} is one of our items"
    else
      return "Couldn't find #{search_term}"
    end
  end
end
```

