by considering the separation of concerns, sticking to MVC, and using class methods on our models, we were ultimately able to implement those features in a clean, well-organized way.

model

```ruby
class Post < ActiveRecord::Base

  validate :is_title_case 
  before_validation :make_title_case 
  belongs_to :author

  #put new code here
  def self.by_author(author_id)
    where(author: author_id)
      #querys the database, looking through Post.authors that have the same passed in Id
  end

  def self.from_today
  where("created_at >=?", Time.zone.today.beginning_of_day)
  end
 
  def self.old_news
  where("created_at <?", Time.zone.today.beginning_of_day)
  end

  private

  def is_title_case
    if title.split.any?{|w|w[0].upcase != w[0]}
      errors.add(:title, "Title must be in title case")
    end
  end

  def make_title_case
    self.title = self.title.titlecase
  end
end
```

view 

```erb
<h1>Believe It Or Not I'm Blogging On Air</h1>
<div>
  <h3>Filter posts:</h3>
  <%= form_tag("/posts", method: "get") do %>
    <!--we use the form_tag helper to, with the /posts route and call a get method-->
    <%= select_tag "author", options_from_collection_for_select(@authors, "id", "name"), include_blank: 
	
true %>
     <%= select_tag "date", options_for_select(["Today", "Old News"]), include_blank: true %>
    <%= submit_tag "Filter" %>
  <% end %>
    <!--we ussaully use the Author.all, but becuase we don't want to qurey our database in a model we call @authors, wich is equal to a method that does this in our model class-->
</div>
<% @posts.each do |post| %>
  <div>
    <h2><%= post.title %></h2>
    <h3>by: <%= link_to post.author.name, author_path(post.author) %></h3>
    <p><%= post.description %></p>
  </div>
<% end %>
```

controller

```ruby
class PostsController < ApplicationController
  helper_method :params


  def index
    # provide a list of authors to the view for the filter control
    @authors = Author.all
   
    # filter the @posts list based on user input
    if !params[:author].blank?
        #if is not author is blank
      @posts = Post.by_author(params[:author])
        #then @posts is equal a method that will query the data base for us
    elsif !params[:date].blank?
        #other wise if date is not blank
      if params[:date] == "Today"
          #if the date is today
        @posts = Post.from_today
      else
        @posts = Post.old_news
      end
    else
      # if no filters are applied, show all posts
      @posts = Post.all
    end
  end
  
  def show
    @post = Post.find(params[:id])
  end

  def new
    @post = Post.new
  end

  def create
    @post = Post.new(params)
    @post.save
    redirect_to post_path(@post)
  end

  def update
    @post = Post.find(params[:id])
    @post.update(params.require(:post))
    redirect_to post_path(@post)
  end

  def edit
    @post = Post.find(params[:id])
  end
end

```

