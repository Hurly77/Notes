## divide and organize our data 

we could add a `show` action to allow us to send specific record/model instances

```ruby
Rails.application.routes.draw do
  get '/birds' => 'birds#index'
  get '/birds/:id' => 'birds#show' # new
end
```

Then we could add an additional action:

```ruby
class BirdsController < ApplicationController
  def index
    birds = Bird.all
    render json: birds
  end
 
  def show
    bird = Bird.find_by(id: params[:id])
    render json: bird
  end
end

```

> **Reminder:** No need for instance variables anymore, since we're immediately rendering `birds` and `bird` to JSON and are not going to be using ERB.

Now, visiting `http://localhost:3000/birds` will produce an array of `Bird` objects, but `http://localhost:3000/birds/2` will produce just one:

```ruby
{
  "id": 2,
  "name": "Grackle",
  "species": "Quiscalus Quiscula",
  "created_at": "2019-05-09T21:51:41.543Z",
  "updated_at": "2019-05-09T21:51:41.543Z"
 }

```

**endpoints** - useing multiple routes to differentiate between specific requests

## Removing Content When Rendering

Consider, for instance, the last piece of data:

```js
{
  "id": 2,
  "name": "Grackle",
  "species": "Quiscalus Quiscula",
  "created_at": "2019-05-09T21:51:41.543Z",
  "updated_at": "2019-05-09T21:51:41.543Z"
}
```

we probably don't need bits of data like `created_at` and `updated_at`. we can just pick and choose what we want to send:

```ruby
def show
  bird = Bird.find_by(id: params[:id])
  render json: {id: bird.id, name: bird.name, species: bird.species } 
end
```

 we'll see just the id, name and species:

```js
{
  "id": "3",
  "name": "Common Starling",
  "species": "Sturnus Vulgaris"
}
```

to use Ruby's built-in `slice` method

```ruby
def show
  bird = Bird.find_by(id: params[:id])
  render json: bird.slice(:id, :name, :species)
end
```

Rather than having to spell out each key, the `Hash` [`slice` method](https://ruby-doc.org/core-2.5.0/Hash.html#method-i-slice) returns a *new* hash with only the keys that are passed into `slice`

```js
{
  "id": "3",
  "name": "Common Starling",
  "species": "Sturnus Vulgaris"
}
```

`slice` won't work for an array of hashes, like this does:

```ruby
def index
  birds = Bird.all
  render json: birds
end
```

In this case, we can add in the `only:` option directly after listing an object we want to render to JSON:

```ruby
def index
  birds = Bird.all
  render json: birds, only: [:id, :name, :species]
end

#=>
[
  {
    "id": 1,
    "name": "Black-Capped Chickadee",
    "species": "Poecile Atricapillus"
  },
  {
    "id": 2,
    "name": "Grackle",
    "species": "Quiscalus Quiscula"
  },
  {
    "id": 3,
    "name": "Common Starling",
    "species": "Sturnus Vulgaris"
  },
  {
    "id": 4,
    "name": "Mourning Dove",
    "species": "Zenaida Macroura"
  }
]
```

**Alternatively**, rather than specifically listing every key we want to include, we could also exclude particular content using the `except:` option, like so:

```ruby
def index
  birds = Bird.all
  render json: birds, except: [:created_at, :updated_at]
end
```

The above code would achieve the same result

### Drawing Back the Curtain on Rendering JSON Data

The last code snippet can be rewritten in full to show what is actually happening:

```ruby
def index
  birds = Bird.all
  render json: birds.to_json(except: [:created_at, :updated_at])
end
```

### Basic Error Messaging When Rendering JSON Data

 In our `show` action, we are currently using `Bird.find_by`, passing in `id: params[:id]`:

```ruby
def show
  bird = Bird.find_by(id: params[:id])
  render json: {id: bird.id, name: bird.name, species: bird.species } 
end
```

When using `find_by`, if the record is not found, `nil` is returned. As we have it set up, if `params[:id]` does not match a valid id, `nil` will be assigned to the `bird` variable.

As `nil` is a *false-y* value in Ruby, this gives us the ability to write our own error messaging in the event that a request is made for a record that doesn't exist:

Now, if we were to send a request to an invalid endpoint like `http://localhost:3000/birds/hello_birds`, rather than receiving a general HTTP error, we would still receive a response from the API:

```js
{
  "message": "Bird not found"
}
```

