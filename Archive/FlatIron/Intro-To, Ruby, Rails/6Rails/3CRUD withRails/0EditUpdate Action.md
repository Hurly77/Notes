rdynamic route that accepts an `:id` as a parameter that the controller can access:

```ruby
get 'articles/:id/edit', to: 'articles#edit', as: :edit_article 
patch 'articles/:id', to: 'articles#update'
#or shortcut 
resources :articles, only: [:index, :show, :new, :create, :edit, :update]
```

##### controllers before

```ruby
def edit
end
 
def update
end
```

```ruby
def edit
  @article = Article.find(params[:id])
end
#to take a look at params
def update
  raise params.inspect
end
```

To get our UPDATE method working we'll have to: 

- Query the database for the `Article` record that matches the `:id` passed to the route.
- Store the query in an instance variable.
- Update the values passed from the form (the update method here is the `update` method supplied by Active Record, not the `update` method we're creating). **The update method takes a hash of the attributes for the model as its argument, e.g. `Article.find(1).update(title: "I'm Changed", description: "And here too!")**
- Save the changes in the database.
- Redirect the user to the `show` page so they can see the updated record.

```ruby
def update
  @article = Article.find(params[:id])
  @article.update(title: params[:article][:title], description: params[:article][:description])
  redirect_to article_path(@article)
end
```

##### after

```ruby
def edit
    @article = Article.find(params[:id])
  end

  def update
    @article = Article.find(params[:id])
    @article.update(title: params[:article], description: params[:article])
    redirect_to article_path(@article)

  end
```

##### edit view, before

```erb
<%= form_tag articles_path do %>
  <label>Article title:</label><br>
  <%= text_field_tag :title %><br>
 
  <label>Article Description</label><br>
  <%= text_area_tag :description %><br>
 
  <%= submit_tag "Submit Article" %>
<% end %>
```

helper, `form_for`, which **will automatically** set up the url where the form will be sent.

```erb
<%= form_for @article do |f| %>
<!--form_for determines that @article is not a new instance of the Article class.-->
  <%= f.label 'Article Title' %><br>
  <%= f.text_field :title %><br>
 
  <%= f.label 'Article Description' %><br>
  <%= f.text_area :description %><br>
 
  <%= f.submit "Submit Article" %>
<!--form_for determines that @article is not a new instance of the Article class.-->
<% end %>
```

