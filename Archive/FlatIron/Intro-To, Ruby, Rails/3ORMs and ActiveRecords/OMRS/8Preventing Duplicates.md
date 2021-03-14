## The Dreaded Duplication

If we use the same attrubute to create two ruby objects we get practically identical objects Like if we use Song class and create 

```ruby
hello = Song.new("Hello", "25")
hello_again = Song.new("Hello", "25")
#WHEN WE TRY TO SAVE THOSE TO THE DATA BASE WE'D GET
hello.save
hello_again.save
 
DB[:conn].execute("SELECT * FROM songs WHERE name = "Hello")
# => [[1, "Hello", "25"], [2, "Hello", "25"]]
#we should check to make sure the record doesn't already exist
```

## Saving vs. Updating

```ruby
class Song
 
attr_accessor :name, :album
attr_reader :id
 
  def initialize(id=nil, name, album)
    @id = id
    @name = name
    @album = album
  end
 
  def save
    if self.id
      self.update
    else
      sql = <<-SQL  #
        INSERT INTO songs (name, album)
        VALUES (?, ?)
      SQL
 
      DB[:conn].execute(sql, self.name, self.album)
      @id = DB[:conn].execute("SELECT last_insert_rowid() FROM songs")[0][0]
    end
  end
 
  def self.create(name:, album:)
    song = Song.new(name, album)
    song.save
    song
  end
 
  def self.find_by_id(id)
    sql = "SELECT * FROM songs WHERE id = ?"
    result = DB[:conn].execute(sql, id)[0]
    Song.new(result[0], result[1], result[2])
  end
 
  def update
    sql = "UPDATE songs SET name = ?, album = ? WHERE id = ?"
    DB[:conn].execute(sql, self.name, self.album, self.id)
  end
end
```



## The  #find_or_create_by Method

```ruby

  def self.find_or_create_by(name:, album:)
    song = DB[:conn].execute("SELECT * FROM songs WHERE name = ? AND album = ?", name, album)
    if !song.empty?
      song_data = song[0]
      song = Song.new(song_data[0], song_data[1], song_data[2])
    else
      song = self.create(name: name, album: album)
    end
    song
  end

#BREAK DOWN
#First, we query the database: does a record exist that has this name and album?
song = DB[:conn].execute("SELECT * FROM songs WHERE name = ? AND album = ?", name, album)#if song does exist the song variable will return an array
[[1, "Hello", "25"]]

if !song.empty? # IF THIS IS TRUE
      song_data = song[0] #THAN WE GRAB THE DATTA IN THE ARRAY #REMEBER DB[:conn]
    #will return an arrray of arrys
      song = Song.new(song_data[0], song_data[1], song_data[2]) #then we create an object from the raw data
 else
      song = self.create(name: name, album: album) #OTHER WISE WE CREATE A NEW INSTANCE WITH .CREATE
    end
    song # AND THEN WE 
  end   
```

### Our Code in Action

Now we can do this:

```ruby
Song.find_or_create_by(name: "Hello", album: "25")
Song.find_or_create_by(name: "Hello", album: "25")
 
DB[:conn].execute("SELECT * FROM songs WHERE name = Hello, album = 25")
# => [[1, "Hello", "25"]]
```

 