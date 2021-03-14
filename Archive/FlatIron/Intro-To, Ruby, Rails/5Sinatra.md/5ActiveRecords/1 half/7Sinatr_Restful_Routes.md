## What Is A RESTful Route?

 If we wanted to view the article, we would make a `GET` request to `/articles/4`. But what about when I want to update that article? Am I hitting a different resource? Nope! Just doing a different action to that same resource. So instead of a `GET` against `/articles/4` we do a `PUT`. That's why separating what you're talking to (the resource/noun) from the action you're doing (**the HTTP verb**) is important! That's key to REST.

## Browser Caveat

Browsers behave a little strangely as it relates to `PUT`, `PATCH` and `DELETE` requests, in that they don't know how to send those requests. Forms that delete and edit need to be submitted via `POST` requests.

| HTTP VERB | ROUTE                | Action        | Used For                                              |
| --------- | -------------------- | ------------- | ----------------------------------------------------- |
| GET       | '/articles'          | index action  | index page to display all articles                    |
| GET       | '/articles/new'      | new action    | displays create article form                          |
| POST      | '/articles'          | create action | creates one article                                   |
| GET       | '/articles/:id'      | show action   | displays one article based on ID in the url           |
| GET       | '/articles/:id/edit' | edit action   | displays edit form based on ID in the url             |
| PATCH     | '/articles/:id'      | update action | *modifies* an existing article based on ID in the url |
| PUT       | '/articles/:id'      | update action | *replaces* an existing article based on ID in the url |
| DELETE    | '/articles/:id'      | delete action | deletes one article based on ID in the url            |

## The Routes

#### Index Action

```ruby
get '/articles' do
  @articles = Article.all
  erb :index
end # allows the view to access all the articles in the database through the instance variable @articles.
```

#### New Action

```ruby
get '/articles/new' do
    #load the form to create a new article.
  erb :new
end
 
post '/articles' do
    #creates a new article based on the params from the form and saves it to the database.
  @article = Article.create(:title => params[:title], :content => params[:content])
#    redirects to the show page.
  redirect to "/articles/#{@article.id}"
end
```

#### Show Action

```ruby
#responds to a GET request to the route '/articles/:id'. Because this route uses a dynamic URL
get '/articles/:id' do
  @article = Article.find_by_id(params[:id])
  erb :show
end
```

#### Edit Action

```ruby
#loads the edit form in the browser by making a GET request to articles/:id/edit.
get '/articles/:id/edit' do  #load edit form
    @article = Article.find_by_id(params[:id])
    erb :edit
  end
 #handles the edit form submission. This action responds to a PATCH request to the route /articles/:id
patch '/articles/:id' do #edit action
  @article = Article.find_by_id(params[:id])
  @article.title = params[:title]
  @article.content = params[:content]
  @article.save
  redirect to "/articles/#{@article.id}"
end
```

First, **we pull the article by the ID from the URL**, then we **update the title and content attributes and save**. The action ends with a **redirect to** the article **show page.**

We do have to do a little extra work to get the edit form to submit via a `PATCH` request.

Your form must include a hidden input field that will submit our form via `patch`.

```erb
<form action="/articles/<%= @article.id %>" method="post">
  <input id="hidden" type="hidden" name="_method" value="patch">
  <input type="text" name="title">
  <input type="text" name="content">
  <input type="submit" value="submit">
</form>
```

The second line above `<input type="hidden" name="_method" value="patch">` is what does this for us.

#### Using `PATCH`, `PUT` and `DELETE` requests with `Rack::MethodOverride` Middleware

In order to use this middleware, and therefore use `PATCH`, `PUT`, and `DELETE` requests, **you *must* tell your app to use the middleware.**

In the `config.ru` file, you'll need the following line to be placed *above* the `run ApplicationController` line:

```ruby
use Rack::MethodOverride #muest be placed above all other controllers
```

In an application with multiple controllers, `use Rack::MethodOverride` must be placed **above all controllers** in which you want access to the middleware's functionality.

#### Delete Action

```ruby
delete '/articles/:id' do #delete action
  @article = Article.find_by_id(params[:id])
  @article.delete
  redirect to '/articles'
end
#This action finds the article in the database based on the ID in the url parameters, and deletes it. 
#then redirects to the index page /articles.
```

Again, this delete form needs the hidden input field:

```ruby
<form action="/articles/<%= @article.id %>" method="post">
  <input id="hidden" type="hidden" name="_method" value="delete">
  <input type="submit" value="delete">
</form>
```

