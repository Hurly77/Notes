#### A Note on Seed Files

The phrase '**seeding** the database' refers to the practice of **filling up your database with some dummy data**

```ruby
# Add seed data here. Seed your database with `rake db:seed`
sophie = Owner.create(name: "Sophie")
Pet.create(name: "Maddy", owner: sophie)
Pet.create(name: "Nona", owner: sophie)
```

**You can write code to seed your database in any number of ways**.

#### Creating a New Owner and Their Associated Pets

```erb
<!--app/views/owners/new.erb-->

<h1>Create a new Owner</h1>
 
<form action="/owners" method="POST">
  <label>Name:</label>
 
  <br>
 
  <input type="text" name="owner[name]" id="owner_name">
 
  <input type="submit" value="Create Owner">
</form>
```

#### Dynamically Generating Checkboxes

```ruby
# controllers/owners_controller.rb
get '/owners/new' do
    #load up all of the pets from the database
  @pets = Pet.all
  erb :'/owners/new'
end
```

```erb
# views/owners/new.erb
<% @pets.each do |pet| %>
<!--dynamically generates checkboxes-->
  <input type="checkbox" name="owner[pet_ids][]" id="<%= pet.id %>" value="<%= pet.id %>"><%= pet.name %><br>
<% end %>
```

- That checkbox has a `name` of `"owner[pet_ids][]"` because we want to structure our `params` hash such that the array of pet IDs is stored inside the `"owner"` hash. We are aiming to associate the pets that have these IDs with the new owner.

- We give the checkbox a `value` of the given pet's ID. This way, when that checkbox is selected, its value, i.e., the pet's ID, is what gets sent through to the `params` hash.

  

- We give the checkbox an `id` of the given pet's ID so that our Capybara test can find the checkbox using the pet's ID.
- Finally, in between the opening and closing input tags, we use ERB to render the given pet's name.

Let's place a `binding.pry` in the `post '/owners'` route and submit our form so that we can get a better understanding of the `params` hash we're creating with our form. Once you hit your binding, type `params` in the terminal, and you should see something like this:

```ruby
{"owner"=>{"name"=>"Adele", "pet_ids"=>["1", "2"]}}
```

#### Creating New Owners with Associated Pets in the Controller

***IF*** we had a hash, `owner_info`, that looked like this...

```ruby
owner_info = {name: "Adele"}

#...we could easily create a new owner like this:

Owner.create(owner_info)
```

params hash contains this additional key of `"pet_ids"` pointing to an **array** of pet ID number`{"owner"=>{"name"=>"cam", "pet_ids"=>["1"]}}`. **we can still use mass assignment. Active Record** is smart enough to take that key of `pet_ids`, **pointing to an array** of numbers, find the pets that have those IDs, and **associate them to the given owner**, all **because** we set up **our associations** such that an owner has many pets.

```ruby
@owner = Owner.create(params["owner"])
# => #<Owner:0x007fdfcc96e430 id: 2, name: "Adele">
#It worked! Now, type:
@owner.pets
#=> [#<Pet:0x007fb371bc22b8 id: 1, name: "Maddy", owner_id: 2>, #<Pet:0x007fb371bc1f98 id: 2, name: "Nona", owner_id: 2>]
```

```ruby
# app/controllers/owners_controller.rb
 
post '/owners' do
  @owner = Owner.create(params[:owner])
  redirect "/owners/#{@owner.id}"
end
```

#### Creating a New Owner and Associating Them with a New Pet

add a section to our form for creating a new pet:

```erb
and/or, create a new pet:
    <br>
    <label>name:</label>
      <input  type="text" name="pet[name]"></input>
    <br>

<!--whole form-->

<h1>Create a New Owner</h1>
 
<form action="/owners" method="POST">
  <label>Owner Name:</label>
  <br>
  <input type="text" name="owner[name]">
 
  <br>
  <p>Select an existing pet or create a new one below.</p>
 
 
  <h3>Existing Pets</h3>
  <% @pets.each do |pet| %>
    <input type="checkbox" name="owner[pet_ids][]" id="<%= pet.id %>" value="<%= pet.id %>" > <%= pet.name %></input><br>
  <% end %>
  <br>
 
  <h3>New Pet</h3>
<!--creating a new pet -->
  <label>Pet Name: </label>
  <br>
  <input type="text" name="pet[name]" id="pet_name"></input>
  <br><br>
 
  <input type="submit" value="Create Owner">
</form>
```

 if we fill out our form like this...

submit our form

```ruby
{"owner"=>{"name"=>"Adele", "pet_ids"=>["1", "2"]}, "pet"=>{"name"=>"Fake Pet"}}
```

```ruby
#params["pet"]["name"], use it to create a new pet, and add the new pet to our new owner's collection of pets:
@owner.pets << Pet.create(name: params["pet"]["name"])

# if the user does not fill out the field to name and create a new pet params hash would look like this:
{"owner"=>{"name"=>"Adele", "pet_ids"=>["1", "2"]}, "pet"=>{"name"=>""}}
```

**The above line** of code would create a new pet with a name of an **empty string** and associate it to our owner **not good**

```ruby
#check whether or not the value of params["pet"]["name"] is an empty string
if !params["pet"]["name"].empty?
  @owner.pets << Pet.create(name: params["pet"]["name"])
end

#all together:
post '/owners' do
  @owner = Owner.create(params[:owner])
  if !params["pet"]["name"].empty?
    @owner.pets << Pet.create(name: params["pet"]["name"])
  end
  redirect "owners/#{@owner.id}"
end
```

**NOTE: When using the shovel operator, ActiveRecord instantly fires update SQL without waiting for the save or update call on the parent object, unless the parent object is a new record.**



### Editing Owners and Associated Pets

We need another form field to create a new pet that'll be associated with our owner.

```erb
# edit.erb
<h1>Update Owner</h1>
 
<h2>Edits for <%= @owner.name %></h2>
 
  <form action="/owners/<%= @owner.id %>" method="POST">
    <input id="hidden" type="hidden" name="_method" value="patch">
    <label>Edit the Owner's Name:</label>
    <br>
    <input type="text" name="owner[name]" value="<%= @owner.name %>">
 
    <br>
    <p>Select an existing pet or create a new one below.</p>
 
 
    <h3>Existing Pets</h3>
    <% @pets.each do |pet| %>
      <input type="checkbox" name="owner[pet_ids][]" id="<%= pet.id %>" value="<%= pet.id %>" <%='checked' if @owner.pets.include?(pet) %>> <%= pet.name %></input><br>
    <% end %>
<!--test whether the given pet is already present in the current owner's collection of pets-->
    <br>
 
    <h3>New Pet</h3>
    <label>Pet Name: </label>
    <br>
    <input type="text" name="pet[name]" id="pet_name"></input>
    <br><br>
 
    <input type="submit" value="Update Owner">
  </form>
```

 unchecked the first two pets checked the next two pets.

```ruby
{"owner"=>{"name"=>"Adele", "pet_ids"=>["3", "4"]},
 "pet"=>{"name"=>"Another New Pet"},
 "splat"=>[],
 "captures"=>["8"],
 "id"=>"8"}
```

#### Updating Owners in the Controller

allow us to update an owner in the same way. In our Pry console in the terminal, let's execute the following:

```ruby
@owner = Owner.find(params[:id])
@owner.update(params[:owner])
```

Now, if we type `@owner.pets`, we'll see that the owner is no longer associated to the pets with an ID of 1 or 2, but they are associated to pets 3 and 4:

```ruby
@owner.pets
# => [#<Pet:0x007fd511d5e560 id: 3, name: "SBC", owner_id: 8>,
 #<Pet:0x007fd511d5e3d0 id: 4, name: "Fake Pet", owner_id: 8>]
```

Great! Now, we need to implement logic similar to that in our `post '/owners'` action to handle a user trying to associate a brand new pet to our owner:

```ruby
patch '/owners/:id' do
    ####### bug fix
    if !params[:owner].keys.include?("pet_ids")
    params[:owner]["pet_ids"] = []
    end
    #######
 
    @owner = Owner.find(params[:id])
    @owner.update(params["owner"])
    if !params["pet"]["name"].empty?
      @owner.pets << Pet.create(name: params["pet"]["name"])
    end
    redirect "owners/#{@owner.id}"
end

```

**NOTE: The bug fix is required so that it's possible to remove ALL previous pets from owner.**

### Creating and Updating Pets with Associated Owners

Now that we've walked through these features together for the `Owner` model, take some time and try to build out the same functionality for `Pet`. The form to create a new pet should allow a user to select from the list of available owners and/or create a new owner, and the form to edit a given pet should allow the user to select a new owner or create a new owner. Note that if a new owner is created it would override any existing owner that is selected.

Make sure you run the tests to check your work.

# lab

```ruby
get '/owners' do
    @owners = Owner.all
    erb :'/owners/index' 
  end

  get '/owners/new' do 
    @pets = Pet.all
    erb :'/owners/new'
  end
 
  post '/owners' do
    @owner = Owner.create(params[:owner])
    if !params["pet"]["name"].empty?
      @owner.pets << Pet.create(name: params["pet"]["name"])
    end
    redirect "owners/#{@owner.id}"
  end

  get '/owners/:id/edit' do 
    @owner = Owner.find(params[:id])
    erb :'/owners/edit'
  end

  get '/owners/:id' do 
    @owner = Owner.find(params[:id])
    erb :'/owners/show'
  end

  patch '/owners/:id' do
    ####### bug fix
    if !params[:owner].keys.include?("pet_ids")
    params[:owner]["pet_ids"] = []
    end
    #######
 
    @owner = Owner.find(params[:id])
    @owner.update(params["owner"])
    if !params["pet"]["name"].empty?
      @owner.pets << Pet.create(name: params["pet"]["name"])
    end
    redirect "owners/#{@owner.id}"
end
end
```

```ruby
i = <Pet932840293 name=["blah"]>
j = <Owner=["hello"]>
i.name = j    
```

