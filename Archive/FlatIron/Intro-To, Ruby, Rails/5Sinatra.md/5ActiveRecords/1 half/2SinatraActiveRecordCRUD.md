**Important:** In Sinatra, the **order in which you define your routes in a controller matters**. Routes are matched in the order they are defined. So, if we were to define the `get '/articles/:id'` route *before* the `get '/articles/new'` route, Sinatra would feed all requests for `/articles/new` to the `/articles/:id` route and we should see an error telling us that your app is unable to find an `Article` instance with an `id` of `"new"`. The takeaway is that you should define your `/articles/new` route *before* your `/articles/:id` route.

##### Database

First, you'll need to create the `articles` table. An article should have a title and content.

Next, set up the corresponding `Article` model. Make sure the class inherits from `ActiveRecord::Base`.





# lab

```ruby
class CreateArticles < ActiveRecord::Migration[5.2]
  def change
    create_table :articles do |t|
      t.string :title
      t.string :content

    end
  end
end

```





```ruby
require_relative '../../config/environment'

class ApplicationController < Sinatra::Base

  configure do
    set :public_folder, 'public'
    set :views, 'app/views'
  end  
```

```ruby
  get '/' do
    end
```

```ruby
    get '/articles' do
      @articles = Article.all #renders all the Article_objects

      erb :index
    end
    #index.erb
    `<%@articles.each do |article| %> #for every instance of article this loop will run
  <a href="/articles/<%= article.id %>">
    <h2><%= article.title %></h2>
    <p><%= article.content %></p>
  </a>
<% end %>`
```

```ruby
  post '/articles' do
      @article = Article.create(params) #this creates and saves a new instance of Article for what evere data is passed in to the name attributs on the form

      redirect to "/articles/#{@article.id}" # "#{@article.id}" this makes or code dynamic we have to interpolate so that in the url it doesn't just appear as @article.id also it wouldn't work
      #and it passes in what ever ID the article was given
    end
```



```ruby
    get '/articles/new' do
      erb :new
    end
    #new.erb
    `<form action="/articles" method="POST"> 

  <label for="title">Title: </label>
  <input type="text" name="title" id="title">

  <label for="content">Content: </label>
  <textarea name="content" id="content"></textarea>

  <button type="submit" id="submit">Submit</button>

</form>`
```

```ruby
    get '/articles/:id' do
      @article = Article.find(params[:id])#use the Active record method find to find the passed in attribute

      erb :show
    end
#show.erb
    `<h1><%= @article.title %></h1>
<p><%= @article.content %></p>
#send a request to the delete controller action
<form action="/articles/<%= @article.id %>" method="POST"> `#will dispal what ever the instance id is for this page`
  <input type="hidden" name="_method" id="hidden" value="DELETE">
     #form includes the hidden input tag
#**add a "delete" button to the show page.**
  <button type="submit" id="delete">Delete</button>

</form>`
```

### Delete

The Delete CRUD action corresponds to the delete controller action, `delete '/articles/:id'`. To initiate this action, we'll **add a "delete" button to the show page.** This button will be in a form, but since the **form isn't visible by default,** you should **only be able to see the button** (intriguing, I know). The form will send a request to the delete controller action, where we will identify the article to delete and delete it. Then, the action should redirect to the index of all articles — we can't go back to the show page, since the article has been deleted!

#### Making our Delete "Button"

In order to make a form that looks like a button, all we need to do is make a form that has no input fields, only a "submit" button with a value of "delete". So, give your form tag a method of `POST` and an action of `"/articles/:id'`. Make sure to dynamically set the `:id` of the form action! You'll also need to make sure the form includes the hidden input tag to change the request from `POST` to `DELETE`.

```ruby
  patch '/articles/:id' do
      @article = Article.find(params[:id]) #find the instance id that is passed in above
      @article.title = params[:title] #then set @article = to the title and content
      @article.content = params[:content]
      @article.save #then saves the @article

      erb :show
    end
    #show.erb
    <h1><%= @article.title %></h1>
<p><%= @article.content %></p>

<form action="/articles/<%= @article.id %>" method="POST">
  <input type="hidden" name="_method" id="hidden" value="DELETE">

  <button type="submit" id="delete">Delete</button>

</form>
```

```ruby
    delete '/articles/:id' do
      Article.find(params[:id]).delete

      redirect to '/' #deletes the article with by its id the redirects the client back the start '/'
    end
```

```ruby
get '/articles/:id/edit' do
      @article = Article.find(params[:id]) 

      erb :edit# renders the edit view for client if they want to update article 
    end
    #edit.erb
    `<form action="/articles/<%= @article.id %>" method="POST">
  <input type="hidden" name="_method" id="hidden" value="PATCH">

  <label for="title">Title: </label>
  <input type="text" name="title" id="title" value="<%= @article.title %>">

  <label for="content">Content: </label>
  <textarea name="content" id="content"><%= @article.content %></textarea>

  <button type="submit" id="submit">Submit</button>

</form>`
end
```

