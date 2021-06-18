#  API-Only Rails Build

## Using the `--api` Flag

To create an API-only include the `--api` after the name of the Rails application name upon creation:

```
rails new bird-watcher-api --api
```

**One noticeable change** - some browser errors will disappear. Since there is no way to render views in this API-only build, if the Rails API fails and we visit it in browser, it will just show a blank screen.

**No changes are required when setting up resources for an API-only Rails build.**

## From Beginning to End

API-only Rails build:

```
rails new bird-watcher-api --api
```

 Rather than create *everything* by hand

```
rails g resource bird name species
rails g resource location latitude longitude
rails g resource sighting bird:references location:references
```

This will create three migrations, three models, and three empty controllers

```ruby
bird_a = Bird.create(name: "Black-Capped Chickadee", species: "Poecile Atricapillus")
bird_b = Bird.create(name: "Grackle", species: "Quiscalus Quiscula")
bird_c = Bird.create(name: "Common Starling", species: "Sturnus Vulgaris")
bird_d = Bird.create(name: "Mourning Dove", species: "Zenaida Macroura")
 
location_a = Location.create(latitude: "40.730610", longitude: "-73.935242")
location_b = Location.create(latitude: "30.26715", longitude: "-97.74306")
location_c = Location.create(latitude: "45.512794", longitude: "-122.679565")
 
sighting_a = Sighting.create(bird: bird_a, location: location_a)
sighting_b = Sighting.create(bird: bird_b, location: location_b)
sighting_c = Sighting.create(bird: bird_c, location: location_c)
```

hen the controller actions we want in the API will need to be added:

```ruby
def index
  sightings = Sighting.all
  render json: sightings, include: [:bird, :location]
end
```

Since the `resource` generator was used, it would be good to be diligent and clean up `config/routes.rb` once we've decided what endpoints the API should have.

## A Note While Developing APIs - Dealing with CORS

> **Cross-Origin Resource Sharing (CORS)** -  a mechanism that uses additional [HTTP](https://developer.mozilla.org/en-US/docs/Glossary/HTTP) headers to **tell browsers** to give a web application running at one [origin](https://developer.mozilla.org/en-US/docs/Glossary/origin), access to **selected resources** **from a different origin.** A web application executes a cross-origin HTTP request when it requests a resource that has a different origin (domain, protocol, or port) from its own.

While working on your own APIs, you'll typically want to have your Rails server running while also trying out various endpoints using `fetch()`. In order to do this, though, you will need deal with [Cross-Origin Resource Sharing](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS), or CORS

CORS is designed to **prevent** scripts like `fetch()` from **one origin** accessing a resource from a **different** origin **unless** that resource **specifically** states that it **expects** to **share**

###### ***EXAMPLE OF CORS***

>  So, for instance, if you have run the command `rails server` with your server running at `http://localhost:3000`, then go to '[www.google.com,](http://www.google.com%2C/)' open the browser console and attempt to send a `fetch()` to your server. The browser considers these two different origins, and will ***refuse*** your request.

`--api` flag 

A solution is already provided though. By using the `--api` flag, the `Gemfile` was altered to include the [`rack-cors`](https://github.com/cyu/rack-cors) gem. The gem will be commented out initially:

```
# Use Rack CORS for handling Cross-Origin Resource Sharing (CORS), making cross-origin AJAX possible
# gem 'rack-cors'
```

To get `rack-cors` working, uncomment the gem and run `bundle install`. Then, add the following to `config/application.rb` **inside** `class Application < Rails::Application`:

```ruby
 config.middleware.insert_before 0, Rack::Cors do
      allow do
        origins '*'
        resource '*',
          :headers => :any,
          :methods => [:get, :post, :delete, :put, :patch, :options, :head],
          :max_age => 0
      end
    end
```

This shouldn't replace anything else inside `class Application < Rails::Application`, just be included in addition.

This will allow you to test your APIs while developing them locally. Secretly, `rack-cors` has been bundled with the last set of lessons to ensure they were all working smoothly in case you decided to code along and spin up a rudimentary API.

**WARNING:** Disabling CORS altogether in the long term can leave your server unsecure. Check out the documentation on [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) and [`rack-cors`](https://github.com/cyu/rack-cors) for additional information.

# Extraction a service class

## Initial Configuration

There are already three resources set up based on where we left off

```ruby
#Models
class Bird < ApplicationRecord
  has_many :sightings
  has_many :locations, through: :sightings
end

class Location < ApplicationRecord
  has_many :sightings
  has_many :birds, through: :sightings
end

class Sighting < ApplicationRecord
  belongs_to :bird
  belongs_to :location
end

#e left off with a messy combination of include, only, and except in order to customize what attributes we wanted to render to JSON:
def show
  sighting = Sighting.find_by(id: params[:id])
  render json: sighting.to_json(:include => {
    :bird => {:only => [:name, :species]},
    :location => {:only => [:latitude, :longitude]}
  }, :except => [:updated_at])
end
```

`http://localhost:3000/sightings/2` produces

```ruby
{
  "id": 2,
  "bird_id": 2,
  "location_id": 2,
  "created_at": "2019-05-14T14:56:35.978Z",
  "bird": {
    "name": "Grackle",
    "species": "Quiscalus Quiscula"
  },
  "location": {
    "latitude": 30.26715,
    "longitude": -97.74306
  }
}
```

We can use the same render statement in an `index` action without having to change it:

```ruby
class SightingsController < ApplicationController
  def index
    sightings = Sighting.all
    render json: sightings.to_json(:include => {
      :bird => {:only => [:name, :species]},
      :location => {:only => [:latitude, :longitude]}
    }, :except => [:updated_at])
  end
 
  def show
    sighting = Sighting.find_by(id: params[:id])
    render json: sighting.to_json(:include => {
      :bird => {:only => [:name, :species]},
      :location => {:only => [:latitude, :longitude]}
    }, :except => [:updated_at])
  end
end
```

One way to resolve this( **issue** - controllers are really just meant to act as a relay between our models and our view, or well, our rendered JSON in this case.) is to build a service class.

## Creating a Service Class

**A service class** - is a class specific to our domain that handles some of the business logic of the application.

Our **goal** is not to change these statements, just move the work off of the controller.

To create a class we will be able to utilize in place of the current render statements, first, we'll create a new folder within `app` called `services`:

```
mkdir app/services
```

Then we'll need to create a service class to use in our `SightingsController`

Since we're specifically arranging and serving up data, and also for reasons that will become much clearer in the next lesson, we'll create a class called `SightingSerializer` and save it in the `services` folder as `sighting_serializer.rb`:

```
touch app/services/sighting_serializer.rb
```

```ruby
class SightingSerializer
end
```

 **you'll need to restart the Rails server**

## Configuring the New Serializer

we are currently calling the `to_json` method on the variables `sightings` and `sighting` in the two controller actions:

```ruby
class SightingsController < ApplicationController
  def index
    sightings = Sighting.all
    render json: sightings.to_json(:include => {
      :bird => {:only => [:name, :species]},
      :location => {:only => [:latitude, :longitude]}
    }, :except => [:updated_at])
  end
 
  def show
    sighting = Sighting.find_by(id: params[:id])
    render json: sighting.to_json(:include => {
      :bird => {:only => [:name, :species]},
      :location => {:only => [:latitude, :longitude]}
    }, :except => [:updated_at])
  end
end
```

 The goal of our new serializer class is to replace this without having to change too much.

First, we will want to define an `initialize` method for the class

```ruby
class SightingSerializer
 
  def initialize(sighting_object)
    @sighting = sighting_object
  end
 
end
```

**Now**, whatever we pass when initializing a new instance of `SightingSerializer` will be stored as `@sighting`. 

The second step is to write a method that will call `to_json` on this instance variable, handling the inclusion and exclusion of attributes, and return the results. 

```ruby
class SightingSerializer
 
  def initialize(sighting_object)
    @sighting = sighting_object
  end
 
  def to_serialized_json
    @sighting.to_json(:include => {
      :bird => {:only => [:name, :species]},
      :location => {:only => [:latitude, :longitude]}
    }, :except => [:updated_at])
  end
 
end
```

clean up `SightingsController`

```ruby
class SightingsController < ApplicationController
  def index
    sightings = Sighting.all
    render json: SightingSerializer.new(sightings).to_serialized_json
  end
 
  def show
    sighting = Sighting.find_by(id: params[:id])
    render json: SightingSerializer.new(sighting).to_serialized_json
  end
end
```

In the `to_serialized_json` method, we are passing multiple options into `to_json` when it is called. These options are just key/value pairs in a hash, though, and we can choose to break this line up to get a better grasp of what is actually going on

```ruby
def to_serialized_json
  options = {
    include: {
      bird: {
        only: [:name, :species]
      },
      location: {
        only: [:latitude, :longitude]
      }
    },
    except: [:updated_at],
  }
  @sighting.to_json(options)
end
```

