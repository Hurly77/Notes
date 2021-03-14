## What is ORM?

***Object Relational Mapping* (**ORM) is the technique of accessing a relational database using an object-oriented programming language.

**connects your Ruby program to a given database:**

```ruby
database_connection = SQLite3::Database.new('db/my_database.db')
 
database_connection.execute("Some SQL statement")
```

**ORM** -

**When "mapping" our program to a database, we equate classes with database tables and instances of those classes with table rows.** You may also see this referred to as "wrapping" a database

## Why Use ORM?

- Cutting down on repetitious code.
- Implementing conventional patterns that are organized and sensical

```ruby
#lets keep track of pets and those pets' owners. Owners class and Cats class
#Our program would create a connection to the database:
database_connection = SQLite3::Database.new('db/pets.db')

#We would create an owners table and a cats table:
database_connection.execute("CREATE TABLE IF NOT EXISTS cats(id INTEGER PRIMARY KEY, name TEXT, breed TEXT, age INTEGER)")

database_connection.execute("CREATE TABLE IF NOT EXISTS owners(id INTEGER PRIMARY KEY, name TEXT)")

database_connection.execute("INSERT INTO cats (name, breed, age) VALUES ('Maru', 'scottish fold', 3)")

 #Then, we would need to regularly insert new cats and owners into these tables:
database_connection.execute("INSERT INTO cats (name, breed, age) VALUES ('Hana', 'tortoiseshell', 1)")
```

**Notice  there is a lot of repetition when we insert**

Any **SELECT** queries, for example, would repeat the call to the **database_connection.execute** method and differ only in the specifics of what data we are selecting from which table.

**We don't like to repeat ourselves ** we can write a series of methods to abstract that behavior.

 For example, we can write a **.save** method on our **Cats** class that handles the common action of **INSERT **ing data into the database.

```ruby
class Cat
 
  @@all = []
 
  def initialize(name, breed, age)
    @name = name
    @breed = breed
    @age = age
    @@all << self
  end
 
  def self.all
    @@all
  end
 
  def self.save(name, breed, age, database_connection)
    database_connection.execute("INSERT INTO cats (name, breed, age) VALUES (?, ?, ?)",name, breed, age)
  end
end
```

**Now let's create some new cats and save them to the database:**

```RUBY
database_connection = SQLite3::Database.new('db/pets.db')
 
Cat.new("Maru", "scottish fold", 3)
Cat.new("Hana", "tortoiseshell", 1)
 
Cat.all.each do |cat|
  Cat.save(cat.name, cat.breed, cat.age, database_connection)
end
```

classes are mapped *to or* **equated** with **tables** and **instances** of a class are **equated** *to* **table** **rows**.****  If we have a **Cat** class, we have a **cats table**. Cat **instances** get **stored** *as* **rows** in the **cats table.**                              

**equated** - consider (one thing) to be the same as or equivalent to another.