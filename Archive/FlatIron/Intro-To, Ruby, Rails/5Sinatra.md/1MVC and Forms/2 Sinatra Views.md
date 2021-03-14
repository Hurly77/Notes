##### Part 1: Rendering HTML

```ruby
get '/' do
    "<h1>Hello World</h1>"
end
```

Run `shotgun`

##### Part 2: Using an ERB File

 Sinatra uses a templating engine called **ERB** we can write HTML code there just like in a plain old `.html` file. **Sinatra** is also **configured** by default to **look** for our `.erb` files **in** a directory called `views`.

```ruby
    get '/' do
      erb :index
    end
```

This tells Sinatra to render a file called `index.erb`

##### Part 3: Multiple Routes with Multiple Views

We can create as many **routes** and **views** as we want. Let's create a route called "/**info**" in our controller.

```ruby
 get '/' do
      erb :index
    end
 
    get "/info" do
      "Testing the info page"
    end
```

 Next, let's have this route render a separate file instead. Inside of the `views` directory, create a file called `info.erb`

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Info Page</title>
    </head>
    <body>
        <h1>Info Page</h1>
        <p>This is the info page: here's some information about me!</p>
    </body>
</html>
```

Update to look like this 

```ruby
get '/' do
      erb :index
    end
 
    get "/info" do
      erb :info
    end
```

### Important

It's **important** to note that the **name** of the file **doesn't** **have to match** the name of the **route**.