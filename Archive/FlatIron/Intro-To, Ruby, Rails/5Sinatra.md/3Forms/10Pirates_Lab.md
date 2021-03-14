The erb file needs to be like `erb :pirates/new` but the link only has to go like `<a href="/new">link</a>` 

# lab

##### models/pirate.rb

```ruby
class Pirate
    attr_reader :name, :weight, :height
  @@all = []
  def initialize(pirate) #this is just a bare word, it could be anything, it just poitns to a hash with Key value pairs
    @name = pirate[:name] # pirate => {"name"=> "Jason"}
    @weight = pirate[:weight]
    @height = pirate[:height]
    @@all << self
  end
  def self.all
    @@all
  end
end
```

##### models/ship.rb

```ruby
class Ship
  attr_reader :name, :type, :booty
@@all = []
def initialize(ship) # same as above
  @name = ship[:name]
  @type = ship[:type]
  @booty = ship[:booty]
  @@all << self
end
def self.all
  @@all
end
def self.clear
  self.all.delete
end
end
```

##### views/pirate/new.erb

```erb
<h1>Make your form here</h1>
<form action="/pirates" method="post"><!--Renders when submited this will post to /pirates-->
<label for="name">Pirate Name</label><input type="text" name="pirate[name]"> <!--after submited will post data to the to the key value name in the pirate hash-->
<label for="height">Pirate height</label><input type="text" name="pirate[height]">
<label for="weight">Pirate weight</label><input type="text" name="pirate[weight]">
<br>
<label for="name">Ship Name</label><input id="ship_name_1" type="text" name="pirate[ships][][name]">
    <!--after submited will post to the pirate hash in the ships hash in the array of hashes with key value pairs -->
<label for="type">Ship Type</label><input id="ship_type_1" type="text" name="pirate[ships][][type]">
    <!--Because of the capibara we have to set a  and id for every field such as id="ship_type_1"-->
<label for="booty">Ship Booty</label><input id="ship_booty_1" type="text" name="pirate[ships][][booty]">
<br>
<label for="name">Ship Name</label><input id="ship_name_2" type="text" name="pirate[ships][][name]">
<label for="type">Ship Type</label><input id="ship_type_2" type="text" name="pirate[ships][][type]">
<label for="booty">Ship Booty</label><input id="ship_booty_2" type="text" name="pirate[ships][][booty]">
  <input id="Submit" type="submit">
</form>
```

##### view/pirate/show.erb

```erb
<h1>Pirate</h1>
<div class="pirate">
  <h2>Name: <%=@pirate.name(opens in new tab)%></h2><br><!--here we use the @pirate local variable to accecces the Params[:pirate][:name] wich was instantiated when we posted /pirates -->
  <h2>Height:<%=@pirate.height%></h2><br>
  <h2>Weight:<%=@pirate.weight%></h2><br>
</div><br>
<h1>Ships</h1>
<%@ships.each do |ship|%> <!--here we use the @ships variable to accesses the params[pirates][ships] hash to grabe each instance of our key value pairs -->
<div class="ship">
  <p>Name:<%=ship.name%></p><br>
  <p>Type:<%=ship.type%></p><br>
  <p>Booty:<%=ship.booty%></p>
</div><br>
<% end %>
```

##### app.rb

```ruby
require './environment'
module FormsLab
  class App < Sinatra::Base
get '/' do
  erb :'pirates/new'
end
post '/pirates'
@pirate = Pirate.new(params[:pirate])#OKAY now the good stuff haha, HERE we fuck around a bit, SO... this local variable is set to a new instance of Pirate class wich passes in params[:pirate] as an argument wich actually would look something like this 
    #@pirate = Pirate.new(params = {
    #Pirate=> {
     #"name"=> "What ever the value is."
     #"height"=>"What ever the value is." 
     #3"height"=> "What ever the value is."      
        #}
    #}) 
params[:pirate][:ships].each do |details| # this fuckery right here haha would look like this
    #params = {
    #Pirate=> {hash stuff}, ships= [{"name"=>"stuff"
    #"type"
    #"booty"}, {and so on}] }
#this sets all thos the ships array = to details and passes the in as an argument so all the matching data when insatianted will be displayed to the client
 Ship.new(details)
end
@ships = Ship.all
erb :'pirates/show'
  end
end
```

