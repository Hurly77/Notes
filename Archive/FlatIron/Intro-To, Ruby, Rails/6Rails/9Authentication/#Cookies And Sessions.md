## E-commerce Site

If we want to build a shopping cart for our user, we could create a Cart model with a `has_and_belongs_to_many` relationship but, Inside the URL path we would have and Id which would mean that you could copy and past your shopping cart to a friend and they'd be able to see what was in your cart which could lead to some privacy issues

**For Example:** `GET` request has information on the path, usually Rails pull the Id from the request path and saves it to the `params[:id]` you'd have something that looks like this:

```ruby
def show
	@item = Item.find(params[:id])
end
```

This would be quite convoluted, to store this information in the path

## What's a cookie, anyway?

Let's see what [the spec](http://tools.ietf.org/html/rfc6265) has to say:

```erb
  This section outlines a way for an origin server to send state
  information to a user agent and for the user agent to return the
  state information to the origin server.
 
  To store state, the origin server includes a Set-Cookie header in an
  HTTP response.  In subsequent requests, the user agent returns a
  Cookie request header to the origin server.  The Cookie header
  contains cookies the user agent received in previous Set-Cookie
  headers.  The origin server is free to ignore the Cookie header or
  use its contents for an application-defined purpose.
```

The description is quite technical, so let's look at their example:

```irb
  == Server -> User Agent ==
  Set-Cookie: SID=31d4d96e407aad42
 
  == User Agent -> Server ==
  Cookie: SID=31d4d96e407aad42
```

**S**ecurity **ID**entifier

This example **The sever** is an **HTTP sever**, and the **User Agent is** a **browser**. The **Server** responds to a **request** with the `Set-Cookie` header. This **header** stets the **value** of **the** `SID` cookie **to** `31d4ds96e407aa42` header in its request 

**Next**, when the user visits another page on the same server, the browser sends the cookie back to the server, including the `Cookie: SID=31d4d96e407aad42` header in its request.

## Using cookies

```
  == Server -> User Agent ==
  Set-Cookie: cart_id=273
```

Only with the `cart.id` of the cart we just saved.

When the user comes back to our site, their browser will include the cookie in their reply:

```
  == User Agent -> Server ==
  Cookie: cart_id=273
```

`session` method is available anywhere in the Rails response cycle, and it behaves like a hash:

```ruby
  # set cart_id
  session[:cart_id] = @cart.id
 
  # load the cart referenced in the session
  @cart = Cart.find(session[:cart_id])
```

we **don't** need a `Cart` model at all—we can just **store** a **list** of items **in the session!**

**Rails** manages all session data in a single cookie, named `_YOUR_RAILS_APP_NAME_session`It *serializes* all the key/value pairs you set with `session`, converting them from a Ruby object into a big string. Whenever you set a `key` with the `session` method, Rails updates the value of its session cookie to this big string.

Your Rails server has a key, configured in `config/secrets.yml`.

```
development:
  secret_key_base: kaleisgreat  # probably not the most secure key ever
```

Somewhere else, Rails has a method, let's call it `sign`, which takes a `message` and a `key` and returns a `signature`, which is just a string:

```ruby
# sign(message: string, key: string) -> signature: string
def sign(message, key):
  # cryptographic magic here
  return signature
end
```

Rails verifies that the signature matches the content (that is, that `sign(cookie_body, secret_key_base) == cookie_signature`).

**If a user tries to edit their cookie** and change the `cart_id`, the signature won't match, and Rails will silently ignore the cookie and set a new one.

# Tying it together

In our `items_controller.rb`, we might have an `add_to_cart` method, which is called when the user adds something to their cart. It might work something like this:

```ruby
#IN app/controllers/items_controller.rb

# Routed from POST /items/:id/add_to_cart
def add_to_cart
  # Get the item from the path
  @item = Item.find(params[:id])
 
  # Load the cart from the session, or create a new empty cart.
  cart = session[:cart] || []
  cart << @item.id
 
  # Save the cart in the session.
  session[:cart] = cart
end
```

That's it! It's common to wrap this up in a helper method:

```ruby
class ApplicationController < ActionController::Base
  helper_method :current_cart
 
  def current_cart
    session[:cart] ||= []
  end
end
```

So Now our controller looks like this:

```ruby
# Routed from POST /items/:id/add_to_cart
def add_to_cart
  # Get the item from the path
  @item = Item.find(params[:id])
 
  # Load the cart from the session, or create a new empty cart.
  current_cart << @item.id
end
```

This way, we can use `current_cart` in our views and layouts too. For example, we may want to show the user how many items are in their cart as part of the layout.

This use of cookies worries people and the EU [has created legislation around the use of cookies](https://en.wikipedia.org/wiki/HTTP_cookie#EU_cookie_directive).

- [HTTP RFC Section 9 — Methods](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)
- [RFC 6265 — HTTP State Management Mechanism (the cookie spec)](http://tools.ietf.org/html/rfc6265)
- [Rails – Accessing the Session](http://guides.rubyonrails.org/action_controller_overview.html#accessing-the-session)
- [Has and belongs to many](http://guides.rubyonrails.org/association_basics.html#the-has-and-belongs-to-many-association)
- [EU Cookie Directive](https://en.wikipedia.org/wiki/HTTP_cookie#EU_cookie_directive)

View [Cookies And Sessions ](https://learn.co/lessons/cookies_and_sessions_readme)on Learn.co and start learning to code for free.

You can also edit cookies, for example with [this extension](https://chrome.google.com/webstore/detail/editthiscookie/fngmhnnpilhplaeedifhccceomclgfbg?hl=en).

[the spec](http://tools.ietf.org/html/rfc6265) 

an [HTTP verb](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)

[`has_and_belongs_to_many`](http://guides.rubyonrails.org/association_basics.html#the-has-and-belongs-to-many-association)