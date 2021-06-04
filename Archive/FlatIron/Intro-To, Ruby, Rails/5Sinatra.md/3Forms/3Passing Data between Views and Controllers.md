## Using Instance Variables

**Instance variables** allow us to **bypass** **scope** between the various methods in a class.

We can now access the contents of `@method` inside of our view

**Note:** Instance variables are ONLY passed from the controller method where they are created to the view that is rendered, not between controller methods. For example:

Let's start by taking a look at our params when we submit the form on the /reverse page. The easiest way to do this is to find the post route to which the form is sending data - in this case `post /reverse do` - and add a `puts params` inside the method:

```ruby
post '/reverse' do
  puts params
 
  erb :reversed
end
```

![Puts Params](https://s3.amazonaws.com/learn-verified/puts-params.png)

```ruby
post '/reverse' do
  original_string = params["string"]
  reversed_string = original_string.reverse
 
  erb :reversed
end
```



```ruby
get "/" do
  @user = "Ian"
 
  erb :index # @user will be defined as 'Ian' in the view
end
 
get "/profile" do
  erb :profile # @user will be nil here
end
```

- `<%= contents %>` will display the evaluated expression within the opening and closing.
- `<% contents %>` will evaluate the contents of the expression, but will not display them.

```erb
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>The Ultimate Reversed String!</title>
    <link rel="stylesheet" href="stylesheets/style.css" type="text/css">
  </head>
 
  <body>
    <h1>The Ultimate Reversed String!</h1>
 
    <!--   Use ERB tags to bring in an instance variable containing the reversed string -->
    <h2><%= @reversed_string %></h2>
  </body>
</html>
```

#### Iterating in ERB

```erb
<%# BAD EXAMPLE %>
<h2><%= @friends %></h2>
```

 we want to **assign** an **array** (**rather** than a string) to an instance variable.

If we want to show each item in the array inside a `<h2>` tag we have to to use **iteration** with the `.each` method to loop through each item in the array and put it in its separate `<h2>` tag.

```erb
<% @friends.each do |friend| %>
  <h2><%= friend %></h2>
<% end %>
```

The rendered HTML will look like this: 

```erb
<h2>Emily Wilding Davison</h2>
<h2>Harriet Tubman</h2>
<h2>Joan of Arc</h2>
<h2>Malala Yousafzai</h2>
<h2>Sojourner Truth</h2>
```

