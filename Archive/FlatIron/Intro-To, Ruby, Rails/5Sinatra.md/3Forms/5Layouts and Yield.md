## Layout

**websites** Typically, **have** a **navigation** bar and a **footer** content stay the **same** 

**Below** is the **HTML** for a **website** that has a **header** and **links** to **JavaScript** files.

```html
<!doctype html>
<html>
  <head>
    <title>Cats</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
    <link rel="stylesheet" href="/css/style.css">
  </head>
  <body>
 
    <div class="container">
      <h1>I love cats</h1>
      <img src="https://s3.amazonaws.com/after-school-assets/cat-typing.gif">
 
 
 
    </div>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/js/bootstrap.min.js"></script>
  </body>
</html>
```

We want every page to have a `head` tag with links to Bootstrap's and our own CSS files.  The body of our site contains the **heading** `I love cats` and a cat gif **the bottom, we have our jQuery and Bootstrap links.** 

```html
<h2>This cat...</h2>
<img src="https://s3.amazonaws.com/after-school-assets/cat.gif">
```

### Yield

how can we get the `layout.erb` loaded around the `index.erb`?

This is where the `yield` comes in.

In `layout.erb`, we need to add a `yield` wherever we want the other page content to be loaded:

```html
<!doctype html>
<html>
  <head>
    <title>Cats</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
    <link rel="stylesheet" href="/css/style.css">
  </head>
  <body>
 
    <div class="container">
      <h1>I love cats</h1>
      <img src="https://s3.amazonaws.com/after-school-assets/cat-typing.gif">
 
      <%= yield %> #
 
 
 
    </div>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/js/bootstrap.min.js"></script>
  </body>
</html>
```

Controller

```ruby
get '/' do
  erb :index
end
```

When the above controller action is triggered and the `erb` method is called, it looks to see if there is a view titled `layout.erb`. If that file exists, it loads that content around the desired erb file, in this case `index.erb`.

The resulting HTML will look like this:

```ruby
<!doctype html>
<html>
  <head>
    <title>Cats</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
    <link rel="stylesheet" href="/css/style.css">
  </head>
  <body>
 
    <div class="container">
 
      <h1>I love cats</h1>
      <img src="https://s3.amazonaws.com/after-school-assets/cat-typing.gif">
 
      <h2>This cat...</h2>
      <img src="https://s3.amazonaws.com/after-school-assets/cat.gif">
 
 
    </div>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/js/bootstrap.min.js"></script>
  </body>
</html>
```

