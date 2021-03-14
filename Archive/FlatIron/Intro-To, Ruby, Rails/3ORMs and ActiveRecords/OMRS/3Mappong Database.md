## **sMapping Database Tables to Ruby Objects**

Ruby program and the database don't speak the same language/

Ruby understands objects. The database understands raw data.

we store descriptions of ruby objects in table rows, and we select that row to reconstruct the object

EXAMPLE:

WE will build a **song domain**, we'll have a class **Song** that makes songs, and that will have two attributes **title** and **lenghth**

#### .new_from_db

lets build a method**.new_from_db**.

First we need to convert the database into a ruby object, **SQLite** in this case will **return** an **array** for each row, for one song it would look like this. **[1, "Thriller", 356 ]** or **[Id, title, length]**.

```ruby
def self.new_from_db(row)
  new_song = self.new  # self.new is the same as running Song.new
  new_song.id = row[0]
  new_song.name =  row[1]
  new_song.length = row[2]
  new_song  # return the newly created instance, note: this is impicit return
end
```

We only **need** this method **temporarily**. thats why we didn't save or ***create*** anything.

### Song.all

This method will retrieve the data. To do this will need to use the **SELECT * FROM** songs; **SQL query** will create a **variable** called **sql** using a **heredoc** **(<<-)**: 	LIKE THIS

```ruby
sql = <<-SQL
  SELECT *
  FROM songs
SQL
```

Now to make a call to that the data base we use the **DB[:conn]** located in the **Config/enviroment.rb**  FILE: **DB = {:conn => SQLite3::Database.new("db/songs.db")}** this will create an instance of  **SQLite3::Database** . that is how we connect out database. **execute** accepts raw **SQL** as a string. We can make a **.all** method like this:

```ruby
class Song
  def self.all
    sql = #<<-SQL --ignore the octothorpe or # 
      SELECT *
      FROM songs
    SQL
 
    DB[:conn].execute(sql)
  end
end
#READ !!!! THIS!
#This will return an array of rows from the database that matches our query
```

Now we **iterate** over those rows with our .**new_from_db** we made earlier.

```ruby
class Song
  def self.all
    sql = # <<-SQL --ignore the octothorpe or # 
      SELECT *
      FROM songs
    SQL
 
    DB[:conn].execute(sql).map do |row| # REMEBER this is an array of raw data
      self.new_from_db(row) # REMEBER this will instantiate new song but doesn't save it
    end
  end
end
```

### Song.find_by_name

We'll do this just like **song.all** but, also we add name and make it = to ?. **REMEBER** that the **?** is  **BOUND PARAMETER**.

```ruby
class Song
  def self.find_by_name(name)
    sql = # <<-SQL   --ignore the octothorpe or # 
      SELECT *
      FROM songs
      WHERE name = ?
      LIMIT 1
    SQL
 
    DB[:conn].execute(sql, name).map do |row|
      self.new_from_db(row)
    end.first # we're simply grabbing the .first element from the returned array
  end
end
```

