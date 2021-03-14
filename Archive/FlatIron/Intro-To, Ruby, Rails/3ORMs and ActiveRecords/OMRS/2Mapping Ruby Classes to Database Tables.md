## Mapping a Class to a Table

**Why map classes to tables?** Our end goal is to persist information and we need to persist that data efficiently and in an organized manner

**we are building a music player app**

```ruby
class Song
 
  attr_accessor :name, :album
 
  def initialize(name, album)
    @name = name
    @album = album
  end
 
end
```

 In building an ORM, it is conventional to **pluralize** the name of the class to create the name of the table

### Creating the Database

Remember, **classes** are mapped to ***tables** **inside** **a database**, *not to the* **database** as a **whole**.

Ruby programs set up **config** **directory** that contains an **environment**.**rb** file.

This file will look something like this:

```ruby
require 'sqlite3'
require_relative '../lib/song.rb'
 
DB = {:conn => SQLite3::Database.new("db/music.db")}
```

This will create a new database called  **music.db *** stored inside the **db** subdirectory. This will return a ruby object **it will repesent the connection between our RUBY program and our SQL databse**

This is what is looks like when it is returned

```ruby
#<SQLite3::Database:0x007f9d6c294508
 @authorizer=nil,
 @busy_handler=nil,
 @collations={},
 @encoding=nil,
 @functions={},
 @readonly=false,
 @results_as_hash=nil,
 @tracefunc=nil,
 @type_translation=nil>
```

This object is created by SQLite-Ruby gem

**DB** is  constant equal to a hash that contains our connection to the database 

In **lib/song.rb** file, we can access DB constant and database like this

```ruby
DB[:conn]
```

**DB[:conn]** constant refers to our connection to the database.

### Creating the Table

we need to create a songs table

**To "map" our class to a database table, we will create a table with the same name as our class and give that table column names that match the `attr_accessor`s of our class.**

EX:

```ruby
class Song
 
  attr_accessor :name, :album, :id
 
  def initialize(name, album, id=nil)
    @id = id
    @name = name
    @album = album
  end
 
  def self.create_table
    sql =  <<-SQL 
      CREATE TABLE IF NOT EXISTS songs (
        id INTEGER PRIMARY KEY, 
        name TEXT, 
        album TEXT
        )
        SQL
    DB[:conn].execute(sql) 
  end
 
```

Let's break down this code.

**id=nil** - The reason we set the default value of id to nil is becuase each table-row needs and id, whick *will be a primary key*. 

when we CREATE new song with **Song.new**, ***WE DO NOT SET THE SONG'S ID***  a song gets and id when it is saved into the database/

#### The .create_table Method

why? simply because it is the **job** of the **class** as a whole to **create the table** that it is **mapped to.**

**Top-Tip:** For strings that will take up multiple lines in your text editor use a [heredoc](https://en.wikipedia.org/wiki/Here_document) 

`<<-` + `special word meaning "End of Document"` + `the string, on multiple lines` + `special word meaning "End of Document"`.

## Mapping Class Instances to Table Rows

When we say that we are saving data to our database **we are not saving Ruby objects in our database**. ACTUALLY we are saving the **attributes** as one, **single row**.

For example

```ruby
gold_digger = Song.new("Gold Digger", "Late Registration")
 
gold_digger.name
# => "Gold Digger"
 
gold_digger.album
# => "Late Registration" 
```

we would be **saving** the **name** and **album** attributes equal the those **values** like this:

```sqlite
INSERT INTO songs (name, album) VALUES ("Gold Digger", "Late Registration")
```

INTURN THIS CREATES A NEW ROW IN OUR DATABASE

Let's abstract this functionality into an instance method, **#save**

### Inserting Data into a table with the `#save` Method

saves a given instance

```ruby
class Song
 
  def save
     # heredoc to craft our multi-line SQL
    sql = <<-SQL 												#IGNORE THE 
     # INSERT data into our songs table
      INSERT INTO songs (name, album)
     # Bound Parameters
      VALUES (?, ?)
          # Remember that the INTEGER PRIMARY KEY datatype will assign and auto-increment the id attribute of each record that gets saved.
    SQL
 
    DB[:conn].execute(sql, self.name, self.album) #this is a connection to the attributs of the object to the colums 
 
  end
end
```

#### Bound Parameters

Bound parameters - are used to protect program from confusion with [SQL injections](https://en.wikipedia.org/wiki/SQL_injection) and special characters. The ? is a place holder for values passed in as arguments.

 We are creating a new **row** in our **songs table** values that characterize that song instance.

## Creating Instances vs. Creating Table Rows

The moment in which we create a **new** **Song** instance with the  **#new** method is *different than the moment in which we **save** a representation of that **song** to our database*. 

- **#new** creates instance, 

- **#save** saves the **attribute value** and makes that into a **new row

  DON'T USE SAVE METHOD ON INTIALIZATION, THIS IS BECAUSE IT WOULD MAKE THEM **dependent upon/always** coupled.

  Now, we can create and save songs like this:

  ```ruby 
  Song.create_table
  hello = Song.new("Hello", "25")
  ninety_nine_problems = Song.new("99 Problems", "The Black Album")
   
  hello.save
  ninety_nine_problems.save
  ```

**Here we:**

- Create the **songs table.**
- Create two **new song instances.**
- Use the **song.save** method to **persist** them to the **database.**

#### Giving **Song** Instance an **id**

We want hello instance to reflect the database row it is associated with.

```ruby
... #continued from earlier the Creating the Table section for the #save
@id = DB[:conn].execute("SELECT last_insert_rowid() FROM songs")[0][0]
 #sets the instance varible equal to the last inserted row
  end
end
```

It is important to understand process of:

Instantiating a **new instance** of the **Song** class.

inserting a **new row** into the **database table** that contains the information regarding the **instance**

Grabbing the **ID** of the newly inserted **row** and assigning the given Song i**nstance's id attribute** *equal* to the **ID** of its **associated** database **table row**.

### The .create Method

This will allow us to refactor our code that instantiated and saved some songs

```ruby
class Song
  ...
 
  def self.create(name, album)
    song = Song.new(name, album)
    song.save
    song
      #The return value of .create should always be the object that we created
      #thats why we return song, NOTE: thas song is an explicit
  end
end
```

The important concept to grasp here is the idea that we are *not* saving Ruby objects into our database.