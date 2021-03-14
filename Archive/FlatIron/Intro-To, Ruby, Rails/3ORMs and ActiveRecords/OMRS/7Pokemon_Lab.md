we you used **touch db/pokemon.db** command in the command prompt to intialize the data base 

wich is just **db**

To connect that database to our sql we used **sqlite3 path/to/database.db < source_of_sql_data**

```ruby
require 'pry'
class Pokemon
attr_accessor :name, :type, :db 

attr_reader :id

def initialize(id=nil, name=nil, type=nil, db='db/pokemon.db')# here I'm giving this attribute a value of the file where my data is stored
  @id = id
  @name = name
  @type = type
  @db = db
end

def self.save(name, type, db)
  sql = <<-SQL		#
  INSERT INTO pokemon (name, type)
  values(?, ?)
  SQL
  db.execute(sql, [name, type]) #this a prepare statment, I just used a sql varible instead of writting it all out on the same line.
      #prepare statments are suppose to protect against SQL injection 
end

def self.find(id, db)
  sql = <<-SQL			#
    SELECT * FROM pokemon WHERE id = (?); #used bound prams to search
  SQL
  pokemon = db.execute(sql, [id]).flatten # usded flatten to because this returns and array of arrays
  self.new(pokemon[0], pokemon[1], pokemon[2], pokemon[3]) #then use new to create the objects I was looking for
end

end
```

