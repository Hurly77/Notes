### `:views` - view template directory

A string specifying the directory where view templates are located. By default, this is assumed to be a directory named “views” within the application’s root directory (see the `:root` setting). The best way to specify an alternative directory name within the root of the application is to use a deferred value that references the `:root` setting:

```ruby
set :views, Proc.new { File.join(root, "templates") }
```

Programmed Random OCcurence

# lab

#### app/controllers/application_controller

```ruby
require 'sinatra/base'

class App < Sinatra::Base

    set :views, Proc.new { File.join(root, "../views/") }
    #it tells sinatra where to find the views file

get '/' do
  erb :super_hero
end

post '/teams' do
 @team = Team.new(params[:team])


params[:team][:members].each do |details|
  Hero.new(details)
end

@heros = Hero.all

erb :team
end

end
```

models/hero.rb

```ruby
class Hero
attr_reader :name, :power, :biography

@@all = []

def initialize(hero)
  @name =  hero[:name]
  @power = hero[:power]
  @biography = hero[:biography]
  @@all << self
end

def self.all
  @@all
end

end

```

models/team.rb

```ruby
class Team
attr_reader :name, :motto

@@all = []

def initialize(team)
  @name = team[:name]
  @motto = team[:motto]
  @@all << self
end

def self.all
  @@all
end

end
```

views/super_hero.erb

```erb
<h1>Create a Team and Heroes!</h1>

<br><form action="/teams" method="post">
  <br><label for="">Team Name:</label><input type="text" name="team[name]" >
  <br><label for="">Team Motto:</label><input type="text" name="team[motto]">

<br><h1>Hero1</h1>
  <br><label for="">Hero's Name</label><input id="member1_name"type="text" name="team[members][][name]">
  <br><label for="">Hero's Power</label><input id="member1_power"type="text" name="team[members][][power]">
  <br><label for="">Hero's Biography</label><input id="member1_bio"type="text" name="team[members][][biography]">
<br><h1>Hero2</h1>
  <br><label for="">Hero's Name</label><input id="member2_name"type="text" name="team[members][][name]">
  <br><label for="">Hero's Power</label><input id="member2_power"type="text" name="team[members][][power]">
  <br><label for="">Hero's Biography</label><input id="member2_bio"type="text" name="team[members][][biography]">

<br><h1>Hero3</h1>
  <br><label for="">Hero's Name</label><input id="member3_name"type="text" name="team[members][][name]">
  <br><label for="">Hero's Power</label><input id="member3_power"type="text" name="team[members][][power]">
  <br><label for="">Hero's Biography</label><input id="member3_bio"type="text" name="team[members][][biography]">
<input type="submit" name="submit" value="submit">
</form>

```

views/team.erb

```erb
<h1><%=@team.name%></h1>

<h2>Team Motto: <%=@team.motto%></h2>

<%@heros.each do |hero|%>
<div class="hero">
<h2>Hero Name: <%=hero.name%> </h2>
<p>Hero Power: <%=hero.power%></p>
<p>Hero Biography: <%=hero.biography%></p>
</div>
<%end%>
```

config/enviroment

```ruby
ENV['SINATRA_ENV'] ||= "development"

require 'bundler/setup'
Bundler.require(:default, ENV['SINATRA_ENV'])

require_all 'app'
```

