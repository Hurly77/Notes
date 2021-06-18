#### Adding Your Gems

To get Sinatra working with ActiveRecord, we'll add three gems to allow us to *use* ActiveRecord: `activerecord` version `5.2`, sinatra-activerecord`, and `rake

```ruby
  gem 'sinatra'
  gem 'thin'
  gem 'require_all'
  gem 'activerecord', '5.2'  #gives us access to the magical database mapping
  gem 'sinatra-activerecord'#The activerecord gem
  gem 'rake' #The rake gem, short for "ruby make", 
```

**Into our development group**, we'll add two other gems: `sqlite3` and `tux`. `sqlite3` is our **database adapter gem** - it's what allows our Ruby application to **communicate with a SQL database.**

```ruby
  gem 'sinatra'
  gem 'thin'
  gem 'require_all'
  gem 'activerecord', '5.2'
  gem 'sinatra-activerecord'
  gem 'rake'
 
  group :development do
    gem 'shotgun'
    gem 'pry'
    gem 'tux'#will give us an interactive console that pre-loads our database and ActiveRecord relationships for us
    gem 'sqlite3', '~> 1.3.6' #is our database adapter gem
  end
```

```ruby
#this is in the config/enviroment.rb
Bundler.require(:default, ENV['SINATRA_ENV'])).
#set up a connection to our database
configure :development do
  set :database, 'sqlite3:db/database.db' #could change .db name if we wanted
end
#This sets up a connection to a sqlite3 database named "database.db"
```

#### Making a Rakefile

```ruby
#after creating a rakefile.rb
require './config/environment'
require 'sinatra/activerecord/rake'
#rake -T to get a list of commands
```

### Testing it Out

creating a `dogs` table with two columns: `name` and `breed`.

```
rake db:create_migration NAME=create_dogs
```

```ruby
class CreateDogs < ActiveRecord::Migration[5.2]
  def up
  end
 
  def down
  end
end
```

**Important:** When we create migrations with ActiveRecord, we must specify the version we're using just after `ActiveRecord::Migration`. In this case, we're using `5.2`, so all the examples here will show `ActiveRecord::Migration[5.2]`

`up` method

```ruby
class CreateDogs < ActiveRecord::Migration[5.2]
  def up
    create_table :dogs do |t|
      t.string :name
      t.string :breed
    end
  end
 
  def down
    drop_table :dogs
  end
end
```

rub `rake db:migrate ` like this

```
rake db:migrate SINATRA_ENV=development
```

Why add `SINATRA_ENV=development`, you might ask? Well, remember the top line of `config/environment.rb`? It's telling Sinatra that it should use the "development" environment for both `shotgun` and the testing suite

##### The `change` Method

The **change method is actually a shorter way of writing** `up` and `down` **methods**. We can refactor our migration to look like this:

```ruby
class CreateDogs < ActiveRecord::Migration[5.2]
  def change
    create_table :dogs do |t|
      t.string :name
      t.string :breed
    end
  end
 
end
```

While the rollback (`down`) method is not included, it's implicit in the change method. Rolling back the database would work in exactly the same way as using the `down` method.