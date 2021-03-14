## Migrations

**Migrations** are a convenient way for you to **alter your database** in a structured and organized manner. You could edit fragments of SQL by hand but you would then be responsible for telling other developers that they need to go and run them. You’d also have to keep track of which changes need to be run against the production machines next time you deploy.

**Migrations also** allow you to **describe these transformations** using **Ruby**. The great thing about this is that (like most of Active Record’s functionality) it is **database-independent:** you don’t need to worry about the precise syntax of `CREATE TABLE` any more than you worry about variations on `SELECT *` (you can drop down to raw SQL for database-specific features). For example, you could use SQLite3 in development, but MySQL in production.

Another way to think of migrations is like **version control for your database**.

### Setting Up Your Migration

create dir **db** than with **in** the **db** eirectory **create** a **migrate directory** with **mkdir** 

```ruby
#In the db/migrate directory, create a file called 01_create_artists.rb ##(we'll talk about why we added the 01 later).

# db/migrate/01_create_artists.rb
class CreateArtists < ActiveRecord::Migration[5.2]
  def up
  end
 
  def down
  end
end
#we have an up method to define the code to execute when the migration is run and a down method to define the code to execute when the migration is rolled back. Think of it like "do" and "undo."
```

#### Active Record 5.x Migration Syntax Update

**IMPORTANT**: Active Record is **primarily** used in **Rails** applications and as of **Active Record 5.x**, we **must** **specify** **which version of Rails** the migration was written for, **even** in situations like this lab where we **do not have Rails configured.**

```
Caused by:
StandardError: Directly inheriting from ActiveRecord::Migration is not supported. Please specify the Rails release the migration was written for:
 
  class CreateArtists < ActiveRecord::Migration[5.2] 
  ...simply add [5.2] or whatever number is displayed to the end
```

### Active Record Migration Methods: up, down, change

Another method is available to use besides `up` and `down`: `change`, which is more common for basic migrations.

```ruby
# db/migrate/01_create_artists.rb
 
class CreateArtists < ActiveRecord::Migration[5.2]
  def change
  end
end
 #The change method is the primary way of writing migrations. It works for the majority of cases, where Active Record knows how to reverse the migration automatically
```

### Creating a Table

**NOTE**: Recall, we can do this with IRB: `irb -r active_record` or in the config/environment.rb

```ruby
#put this in config/environment.rb
#First, we connect to a database
ActiveRecord::Base.establish_connection(
  :adapter => "sqlite3",
  :database => "db/artists.sqlite"
)
#Then write some SQL to create the table:
sql = <<-SQL
  CREATE TABLE IF NOT EXISTS artists (
  id INTEGER PRIMARY KEY,
  name TEXT,
  genre TEXT,
  age INTEGER,
  hometown TEXT
  )
SQL
 
ActiveRecord::Base.connection.execute(sql)
```

**Using migrations** we till need establish Active Record's connection to the database, but ***we no longer need the SQL!*** 

Since we still need to connect to the database, let's make the connection inside `config/environment.rb`:

```ruby
# config/environment.rb
require 'rake'
require 'active_record'
require 'yaml/store'
require 'ostruct'
require 'date'
 
require 'bundler/setup'
Bundler.require
 
# put the code to connect to the database here
ActiveRecord::Base.establish_connection(
  :adapter => "sqlite3",
  :database => "db/artists.sqlite"
)
 
require_relative "../artist.rb"
```

With the connection to the database configured, we should have access to `ActiveRecord::Migration`, and can create tables using only Ruby!

```ruby
# db/migrate/01_create_artists.rb
def change
  create_table :artists do |t| 
  end
end
#lets add some columns
#On the left we've given the data type we'd like to cast the column as, and on the right, we've given the name we'd like to give the column
def change
    create_table :artists do |t|
      t.string :name 
      t.string :genre
      t.integer :age
      t.string :hometown
    end
  end
#Active Record will generate and auto-increment primary key for us
```

### Running Migrations

Run `rake -T` to see the list of commands we have.

if you have an error with rakehttps://learn.co/tracks/online-software-engineering-structured/orms-and-activerecord/activerecord/mechanics-of-migrations

look at the `Rakefile`. The commands listed when running `rake -T` are made available as Rake tasks through `require 'sinatra/activerecord/rake'`.

```ruby
# config/environment.rb
 
require 'bundler/setup'
Bundler.require
 
ActiveRecord::Base.establish_connection(
  :adapter => "sqlite3",
  :database => "db/artists.sqlite"
)
```

This **file** is **requiring** the **gems** in our **Gemfile** and **giving** our **program** **access** to them. We're using the `establish_connection` method from `ActiveRecord::Base` to **connect** to our `artists` **database**, which will be **created** in the **migration** via **SQLite3** (the adapter).

### Try Out The Following:

Create a class and extend ActiveRecord::Base

```ruby
class Artist < ActiveRecord::Base
end
#Try Out The Following:
#Check that the class exists:
Artist
#returns => Artist (call 'Artist.connection' to establish a connection)

Artist.column_names
#returns => ["id", "name", "genre", "age", "hometown"]

#Instantiate a new Artist named Jon, set his age to 30, and save him to the database:
a = Artist.new(name: 'Jon')
# => #<Artist id: nil, name: "Jon", genre: nil, age: nil, hometown: nil>
 
a.age = 30
# => 30
 
a.save
# => true
```

The `.new` method creates a new instance **but** to persist it we have to **.save** but if we wanted to do this all at the same time we could  with **.create**

```ruby
Artist.create(name: 'Kelly')
# => #<Artist id: 2, name: "Kelly", genre: nil, age: nil, hometown: nil>

#|Return an array of all Artists from the database:|
Artist.all
# => [#<Artist id: 1, name: "Jon", genre: nil, age: 30, hometown: nil>,
 #<Artist id: 2, name: "Kelly", genre: nil, age: nil, hometown: nil>]

#|Find an Artist by name:|
Artist.find_by(name: 'Jon')
# => #<Artist id: 1, name: "Jon", genre: nil, age: 30, hometown: nil>

```

## Using Migrations To Manipulate Existing Tables

Let's add a **favorite_food** column to our **artists** **table** **NOTE**:*Remember that Active Record keeps track of the migrations we've already run so adding the new code to our `01_create_artists.rb`*

To make this change we're going to need a new migration, which we'll call `02_add_favorite_food_to_artists.rb`

### add_column

```RUBY
# db/migrate/02_add_favorite_food_to_artists.rb
 #this tells active records to add a column to artists called favorite_food
class AddFavoriteFoodToArtists < ActiveRecord::Migration[5.2]
  def change
    
  end
end

#Now in bash run rake db:migrate the back to rake console
Artist.column_names
# => ["id", "name", "genre", "age", "hometown", "favorite_food"]

# if you use rake 
#|rake db:rollback| you can roll back that last change like so
Artist.column_names
# => ["id", "name", "genre", "age", "hometown"]
```

