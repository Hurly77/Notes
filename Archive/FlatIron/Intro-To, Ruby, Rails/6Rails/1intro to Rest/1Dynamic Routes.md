writing a test to verify that the page exists:

```ruby
require 'rails_helper'
 
describe 'navigate' do
  before do
    @post = Post.create(title: "My Post", description: "My post desc")
  end
 
  it 'loads the show page' do
    visit "/posts/#{@post.id}"
    expect(page.status_code).to eq(200)
  end
end
```

let's draw a route in *config/routes.rb*

```ruby
get 'posts/:id', to: 'posts#show'
```

```ruby
# app/controllers/posts_controller.rb
 
class PostsController < ApplicationController
  def show
  end
end
```

```ruby
   it 'shows the title on the show page in an h1 tag' do
    visit "/posts/#{@post.id}"
    expect(page).to have_css("h1", text: "My Post")
  end
```

```ruby 
# app/controllers/posts_controller.rb
 
def show
  @post = Post.find(params[:id])
end
```

```erb
<!-- app/views/posts/show.html.erb -->
<h1><%= @post.title %></h1>
```

### Resource routing

Remove:

```ruby
get 'posts/:id', to: 'posts#show'
```

Replace with:

```ruby
resources :posts, only: :show
```

 For right now, just know that it deals with the seven key RESTful routes. In this case, we limited it to only make the `show` action available.