```ruby
require 'pry'
class Dog
  attr_accessor :id, :name, :breed

  def initialize(id: nil, name:, breed:) #Passed in attr's as keys
    @id = id
    @name = name
    @breed = breed    
  end
------------------------------------------------------------------
  def self.create_table #pretty staight forwarn no explaination 
    sql = <<-SQL 
    CREATE TABLE IF NOT EXISTS dogs (
    id INTEGER PRIMARY KEY,
    name TEXT,
    breed TEXT
    );
    SQL

    DB[:conn].execute(sql)
  end
-------------------------------------------------------------------
  def self.drop_table # N/A
    sql = <<-SQL 
    DROP TABLE IF EXISTS dogs;
    SQL
    DB[:conn].execute(sql)
  end
------------------------------------------------------------------
  def save
    sql = <<-SQL 
    INSERT INTO dogs(name, breed)
    VALUES(?, ?)
    SQL

    DB[:conn].execute(sql, self.name, self.breed)
    @id = DB[:conn].execute("SELECT last_insert_rowid() FROM dogs")[0][0]
    self # WAS ASKED TO RETURN THE THE INSTANCE THIS IS HOW I DID IT
  end
----------------------------------------------------------------------
  def self.create(hash) #here we had to pass in a hash, wich is a little different than but, not to difficult
    new_dog = self.new(hash) #this creats a new obj
    new_dog.save #save it in to the table
    new_dog # and then we return it
  end
------------------------------------------------------------------------
  def self.new_from_db(row) # here we take in a row from a table
      # Now we make a hash were the attribute is the key the column is the value 
    hash = {
      id: row[0],
      name: row[1],
      breed: row[2]
    }
    self.new(hash) #then we create a new obj from the key value pairs
  end
-----------------------------------------------------------------------
  def self.find_by_id(id)
    sql = <<-SQL 
    SELECT *
    FROM dogs WHERE id = ?
    LIMIT 1;
    SQL # dogs where id = the passed in id

    DB[:conn].execute(sql, id).map do |row| # then we map through the raw data 
      self.new_from_db(row) #and use our .new_from_db method to create and instance of dog
    end.first #here we are just returning the first to be true
      # HOW EVER BOTH THE ".first" METHOD AND "LIMIT" KEYWORD WERE USELESS
  end
----------------------------------------------------------------------------
  def self.find_or_create_by(name:, breed:) #PASS IN THE KEYS WICH REPRESENT THE VALUE THE POINT TO
  dog = DB[:conn].execute(
    'SELECT * FROM dogs WHERE name = ? AND breed = ?', name, breed).first #THAN WE GRAB THE ONLY THE FIRST TO IN THE TABLE

    if dog # IF TRUE CREATE A NEW INSTANCE USEING THE RECORD DATA
      new_dog = self.new_from_db(dog)
    else # ELSE FALE THAN CREATE A NEW INSTANCE OF dog WHICH WILL SAVE TO TABLE
      new_dog = self.create(name: name, breed: breed)
    end
    new_dog #than we return THAT INSTANCE 
  end
--------------------------------------------------------------------------------
  def self.find_by_name(name)
    name = DB[:conn].execute('SELECT * FROM dogs WHERE name = ?', name).first
      # above we go through the record and take all the 'first' name to match
    name = self.new_from_db(name) #then we create a new instance useing the data from the records 
    name # and return that object
  end
-----------------------------------------------------------------------------
  def update
    sql = 'UPDATE dogs SET name = ?, breed = ? WHERE id = ?' #here we update based on the Id that has been passed in 

    DB[:conn].execute(sql, self.name, self.breed, self.id) 
  end

end
```

