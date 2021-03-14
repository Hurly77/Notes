## First pass: manual checks

`DocumentsController`

```ruby
def show
  @document = Document.find(params[:id])
end
```

```ruby
def show
  return head(:forbidden) unless session.include? :user_id
    #documents should only be shown to users when they're logged in
  @document = Document.find(params[:id])
end
```

The first line is a return guard. Unless the session includes `:user_id`, we return an error. `head(:forbidden)` is a controller method that returns the specified HTTP status code—in this case, if a user isn't logged in, we return `403 Forbidden`.

## Refactor

```ruby
class DocumentsController < ApplicationController
  def show
    return head(:forbidden) unless session.include? :user_id
      #will only let users view document if they are loged in
    @document = Document.find(params[:id])
  end
 
  def index
    return head(:forbidden) unless session.include? :user_id
      #only allow users to view the index view if they ar loggin
  end
 
  def create
    return head(:forbidden) unless session.include? :user_id
    @document = Document.create(author_id: user_id)
  end
 
  def update
    return head(:forbidden) unless session.include? :user_id
    @document = Document.find(params[:id])
    # code to update a document
  end
end
```

Lets look at a more dry way to write this Controller

```ruby
class DocumentsController < ApplicationController
  #the before_action rails method, will check the session to see if the user_id matches the current_user before every action.
    before_action :require_login
    
  def show
    @document = Document.find(params[:id])
  end
 
  def index
  end
 
  def create
    @document = Document.create(author_id: user_id)
  end
 
  private
 
  def require_login
    return head(:forbidden) unless session.include? :user_id
  end
end
```

```ruby
before_action :require_login
```

This is a call to `ActionController` class method `before_action`. A (**filter method**) - runs, before, after, or around, a controller's action.

## Skipping filters for certain actions

If we wanted to allow anyone to see documents:

```ruby
class DocumentsController < ApplicationController
  before_action :require_login
  skip_before_action :require_login, only: [:index]
 
  # ...
end
```

This class method,

```ruby
skip_before_action :require_login, only: [:index]
```

Tells Rails to skip the `require_login` filter only on the `index` action.