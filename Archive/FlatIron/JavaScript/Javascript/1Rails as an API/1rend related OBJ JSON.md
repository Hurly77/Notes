## Setting up Additional Related Resources

The next resource to build, then, might be `Location` so we could connect specific birds to specific locations. 

let's use the `model` generator Rails provides. We can also give `Location` a few attributes, `latitude` and `longitude`:

```ruby
rails g model location latitude:float longitude:float
```

 a `Sighting`. A `Sighting` will connect a specific bird and location

since we have two existing resources we're connecting, we can use the `references` keyword when listing them, and Rails will automatically connect them:

```
rails g resource sighting bird:references location:references
```

This generates a migration with `references`:

```ruby
class CreateSightings < ActiveRecord::Migration[5.2]
  def change
    create_table :sightings do |t|
      t.references :bird, foreign_key: true
      t.references :location, foreign_key: true
 
      t.timestamps
    end
  end
end
```

if we look at the file, we see it still connects the `"birds"` and `"locations"` tables to the `"sightings"` table by id:

```ruby
create_table "sightings", force: :cascade do |t|
  t.integer "bird_id"
  t.integer "location_id"
  t.datetime "created_at", null: false
  t.datetime "updated_at", null: false
  t.index ["bird_id"], name: "index_sightings_on_bird_id"
  t.index ["location_id"], name: "index_sightings_on_location_id"
end
```

The other effect of using `references` in the generator is that it will add relationships automatically to the generated model:

```ruby
class Sighting < ApplicationRecord
  belongs_to :bird
  belongs_to :location
end
```

Through sightings, birds have many locations, and vice versa, so we would update our models to reflect these. Add the following relationships to the `Bird` and `Location` models:

```ruby
class Bird < ApplicationRecord
  has_many :sightings
  has_many :locations, through: :sightings
end
```

```ruby
class Location < ApplicationRecord
  has_many :sightings
  has_many :birds, through: :sightings
end
```

**seed**

```ruby
bird_a = Bird.create(name: "Black-Capped Chickadee", species: "Poecile Atricapillus")
bird_b = Bird.create(name: "Grackle", species: "Quiscalus Quiscula")
bird_c = Bird.create(name: "Common Starling", species: "Sturnus Vulgaris")
bird_d = Bird.create(name: "Mourning Dove", species: "Zenaida Macroura")
 
location_a = Location.create(latitude: "40.730610", longitude: "-73.935242")
location_b = Location.create(latitude: "30.26715", longitude: "-97.74306")
location_c = Location.create(latitude: "45.512794", longitude: "-122.679565")
 
sighting_a = Sighting.create(bird: bird_a, location: location_b)
sighting_b = Sighting.create(bird: bird_b, location: location_a)
sighting_c = Sighting.create(bird: bird_c, location: location_a)
sighting_d = Sighting.create(bird: bird_d, location: location_c)
sighting_e = Sighting.create(bird: bird_a, location: location_b)
```

## Related Models in a Single Controller Action

confirm our data has been created by including a basic `show` action:

```ruby
def show
  sighting = Sighting.find_by(id: params[:id])
  render json: sighting
end
```

With the Rails server running, visiting `http://localhost:3000/sightings/1` should produce an object representing a *sighting*:

```ruby
{
  "id": 1,
  "bird_id": 1,
  "location_id": 2,
  "created_at": "2019-05-14T11:20:37.225Z",
  "updated_at": "2019-05-14T11:20:37.225Z"
}
```

> **ASIDE:** Notice that the object includes its own `"id"`, as well as the related `"bird_id"` and `"location_id"`. That is useful data. We *could* use these values to send additional requests using JavaScript to get bird and location data if needed.

To include bird and location information in this controller action

```ruby
def show
  sighting = Sighting.find_by(id: params[:id])
  render json: { id: sighting.id, bird: sighting.bird, location: sighting.location }
end
```

This produces nested objects in our rendered JSON for `"bird"` and `"location"`:

```js

  "id": 2,
  "bird": {
    "id": 2,
    "name": "Grackle",
    "species": "Quiscalus Quiscula",
    "created_at": "2019-05-14T11:20:37.177Z",
    "updated_at": "2019-05-14T11:20:37.177Z"
  },
  "location": {
    "id": 2,
    "latitude": 30.26715,
    "longitude": -97.74306,
    "created_at": "2019-05-14T11:20:37.196Z",
    "updated_at": "2019-05-14T11:20:37.196Z"
  }
}
```

## Using `include`

An alternative is to use the `include` option to indicate what models you want to nest:

```ruby
def show
  sighting = Sighting.find_by(id: params[:id])
  render json: sighting, include: [:bird, :location]
end
```

This produces similar JSON 

```js
{
  "id": 2,
  "bird_id": 2,
  "location_id": 2,
  "created_at": "2019-05-14T11:20:37.228Z",
  "updated_at": "2019-05-14T11:20:37.228Z",
  "bird": {
    "id": 2,
    "name": "Grackle",
    "species": "Quiscalus Quiscula",
    "created_at": "2019-05-14T11:20:37.177Z",
    "updated_at": "2019-05-14T11:20:37.177Z"
  },
  "location": {
    "id": 2,
    "latitude": 30.26715,
    "longitude": -97.74306,
    "created_at": "2019-05-14T11:20:37.196Z",
    "updated_at": "2019-05-14T11:20:37.196Z"
  }
}
```

`include:` also works fine when dealing with an action that renders an array, like when we use `all` in `index` actions:

```ruby
def index
  sightings = Sighting.all
  render json: sightings, include: [:bird, :location]
end
```

`include` is actually just another option that we can pass into the `to_json` method. Rails is just *obscuring* this part:

```ruby
def index
  sightings = Sighting.all
  render json: sightings.to_json(include: [:bird, :location])
end
 
def show
  sighting = Sighting.find_by(id: params[:id])
  render json: sighting.to_json(include: [:bird, :location])
end
```

And adding some error handling on our `show` action:

```ruby
def show
  sighting = Sighting.find_by(id: params[:id])
  if sighting
    render json: sighting.to_json(include: [:bird, :location])
  else
    render json: { message: 'No sighting found with that id' }
  end
end
```

## Conclusion

When nesting models in JSON the way we saw in this lab, it is entirely possible to use `include` in conjunction with `only` and `exclude`. For instance, if we wanted to remove the `:updated_at` attribute from `Sighting` when rendered:

```ruby
def show
  sighting = Sighting.find_by(id: params[:id])
  render json: sighting, include: [:bird, :location], except: [:updated_at]
end
```

But this begins to complicate things significantly

*also* remove all instances of `:created_at` and `:updated_at` from the nested bird and location data in the above example,

```ruby
def show
  sighting = Sighting.find_by(id: params[:id])
  render json: sighting.to_json(:include => {
    :bird => {:only => [:name, :species]},
    :location => {:only => [:latitude, :longitude]}
  }, :except => [:updated_at])
end
```

This does produce a more specific set of data:

```ruby
{
  "id": 2,
  "bird_id": 2,
  "location_id": 2,
  "created_at": "2019-05-14T11:20:37.228Z",
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

