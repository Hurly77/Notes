## Updating Records

In our Ruby ORM, where attributes of given Ruby objects are stored as an individual row in a database table, we will need to retrieve these attributes, reconstitute them into a Ruby object, make changes to that object using Ruby methods, and *then* save those (newly updated) attributes back into the database.

**analogous **- comparable in certain respects, typically in a way which makes clearer the nature of the things compared

## Updating a Record in a Ruby ORM

we'll be useing a **"music managment app"**  

### The Song Class

With the assumption that our database id in DB[:conn]

```ruby
class Song
 
attr_accessor :name, :album
attr_reader :id
 
  def initialize(id=nil, name, album)
    @id = id
    @name = name
    @album = album
  end
 
  def self.create_table
    sql =  <<-SQL  ingnore -- the #
      CREATE TABLE IF NOT EXISTS songs (
        id INTEGER PRIMARY KEY,
        name TEXT,
        album TEXT
        )
        SQL
    DB[:conn].execute(sql)
  end
 
  def save
    sql = <<-SQL-- the #
      INSERT INTO songs (name, album)
      VALUES (?, ?)
    SQL
 
    DB[:conn].execute(sql, self.name, self.album)
    @id = DB[:conn].execute("SELECT last_insert_rowid() FROM songs")[0][0]
  end
 
  def self.create(name:, album:)#This means that we don't have to pass in arguments in a specific order but instead as key value pairs. In this perticular event the arrgument needs to be able to take in a hash.
    song = Song.new(name, album) 
    song.save
    song
  end
 
  def self.find_by_name(name)
    sql = "SELECT * FROM songs WHERE name = ?"
    result = DB[:conn].execute(sql, name)[0]
    Song.new(result[0], result[1], result[2])
  end
end
```

**we can now retrieve instances from the database**

```ruby
ninety_nine_problems = Song.create(name: "99 Problems", album: "The Blueprint")
 
Song.find_by_name("99 Problems")
# => #<Song:0x007f94f2c28ee8 @id=1, @name="99 Problems", @album="The Blueprint">
```

### Updating Songs

WE need find a record before we update it

```ruby
ninety_nine_problems = Song.find_by_name("99 Problems")
 
ninety_nine_problems.album
# => "The Blueprint"
#uh-oh, 99 Problems is off The Black Album, as we all know. Let's fix this.
```

Now we need to save this record back into the database we'll use the **UPDATE** statement

```sqlite
UPDATE songs
SET album="The Black Album"
WHERE name="99 Problems";
```

Now we need to put it all together with DB[:conn]

```ruby
sql = "UPDATE songs SET album = ? WHERE name = ?"
DB[:conn].execute(sql, ninety_nin_problems.album,
ninety_nin_problems.name) # now the song attribute has been updated
#WE CAN UPDAT OTHER attributes as well
Song.create(name: "hella", album: "25")
#if we want to change the name: attribute from "hella" to "hello" we can like this
hello = Song.find_by_name("hella")
sql = "UPDATE songs SET name= 'HELLO' WHERE name = ?"
DB[:conn].execute(sql, hello.name)
```

## The #update Method

If we want write a method that will update *any* attributes, we'll **first** have to update *all* attributes so any changed one will be caught but the unchanged ones will remain the same.

```ruby
hello = Song.findL_by_name("Hella")
sql = "UPDATE songs SET name = 'Hello', ablum = ? WHERE name = ?"
DB[:conn].execute(sql, hello.album, hello.name)
```

Now we can build the #update method

```ruby
class Song
  ...
 
  def update
    sql = "UPDATE songs SET name = ?, album = ? WHERE name = ?"
    DB[:conn].execute(sql, self.name, self.album, self.name)
  end
end
# NOW WE CAN :
hello = Song.create(name: "Hella", album: "25")
hello.name = "Hello" #this will change hella to hello but now we can't find the hella to update it
hello.update # when this runs,UPDATE songs SET name = hello, album = 25 WHERE name = hello
#but our data base doesn't know of any song name called hello only hella
```

but, we have a problem, we changed the **name** of the **song**, so now when the code runs it can't compare "**hello**" to the **column** where it belongs so we'll have to use some sort of **analogous** such as a **id with a primary key.**  

## Identifying Objects and Records Using ID

We need a fixed and unique attribute. Song records have unique id, and our Song instance has an id attribute.

The unique **id** number of a **Song** instance should ***come from the database***. When a song record gets inserted into the database, that row **automatically** gets assigned a **unique ID number**. We need to grab that **ID number** from the database record and assign it to the **Song** instance's **id** attribute.

## check out this diagram:

![](D:\camer\Documents\Ruby_notes\Untitled drawing.png)

Lets break it down: we create a new instance of **Song** class. with attributes like **ID** which is = nil. When the data is inserted we get a **new record** -- ie. **row** and that **record** has an **ID of 1**. That **id** of the new record is then **assigned** to the id **attribute** of the **original** **object**. This give us the ability to differentiate between  objects and update unique those objects.

## Assigning Unique IDs on  #save

**After** we **insert** and instance of **Song** we want to **assign** a **unique id** to the **object** that it belongs too.

```ruby
def save
  sql = <<-SQL #Ignore #
    INSERT INTO songs (name, album)
    VALUES (?, ?)
  SQL
  DB[:conn].execute(sql, self.name, self.album) #THIS is were we want to place our Song object its unique id from the database.
end
```

get the unique ID of the record we just created

```sqlite
SELECT last_insert_rowid() FROM songs
```

#### **Important:** 

**When we execute the above SQL statement using our SQLite3-Ruby gem, we get back something that may feel unexpected:**

```sqlite
DB[:conn].execute("SELECT last_insert_rowid() FROM songs")
# => [[1]]
--whenever we execute SQL statements against our database using the SQLite3-Ruby gem's #execute method, we will get back an array of arrays With an element with the last row id.
```

## new-and-improved #save method:

```ruby
def save
  sql = <<-SQL #ignore #
    INSERT INTO songs (name, album)
    VALUES (?, ?)
  SQL
  DB[:conn].execute(sql, self.name, self.album)
  @id = DB[:conn].execute("SELECT last_insert_rowid() FROM songs")[0][0]
end
#Now let's see what happens when we create a new song:
    
    hello = Song.create(name: "Hello", album: "25")
 
hello.name
# => "Hello"
 
hello.album
# => "25"
 
hello.id #We did it! Now our individual Song objects will get assigned a unique id attribute,
# => 1
```

## Using -ID- to Update Records 

**#update** should identify unique **ID** that  **both** the **object** and **table** share

```ruby
class Song
  ...
 
  def update
    sql = "UPDATE songs SET name = ?, album = ? WHERE id = ?"
    DB[:conn].execute(sql, self.name, self.album, self.id)
  end
end
#Now we will never have to worry about accidentally updating the wrong record
```

## Refactoring our #save Method to Avoid Duplication

Save looks like this

```ruby
def save
  sql = <<-SQL #ignore
    INSERT INTO songs (name, album)
    VALUES (?, ?)
  SQL
 
  DB[:conn].execute(sql, self.name, self.album)
  @id = DB[:conn].execute("SELECT last_insert_rowid() FROM songs")[0][0]
end
    #The #save method at it's current state is subject to duplication with the only difference being the ID NUMBER
    # IF WE SAVE THE SAME SONG TWICE we'll have it returned WITH THE SAME VALUES AND DIFFERENT ID'S
```

We need to see if the **object** has been **persisted**. If so, *don't * **INSERT**, **UPDATE** the existing one.

```ruby
#just add this to the top of the #save method
  if self.id
    self.update
  else
```

