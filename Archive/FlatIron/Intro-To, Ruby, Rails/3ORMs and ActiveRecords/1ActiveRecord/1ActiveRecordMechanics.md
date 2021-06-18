## Active Record ORM

Active Record is a Ruby gem, meaning we get an entire library of code just by running **gem install activerecord** or by including it in our **Gemfile**

#### Connect to DB

Telling active records where our **database is**. You can do this by running `ActiveRecord::Base.establish_connection`**Once** `establish_connection` is run, `ActiveRecord::Base` keeps it stored as a **class variable** at `ActiveRecord::Base.connection`

```ruby
ActiveRecord::Base.establish_connection(
  :adapter => "sqlite3",
  :database => "db/students.sqlite"
)
```

#### Create a table

```ruby
sql = <<-SQL
  CREATE TABLE IF NOT EXISTS students (
  id INTEGER PRIMARY KEY,
  name TEXT
  )
SQL
 
# Remember, the previous step has to run first so that `connection` is set!
ActiveRecord::Base.connection.execute(sql)
```

#### Link a Student "model" to the database table **students**

The last step in telling Ruby class to make use of **ActiveRecord**'s built-in ORM. We can do this with **class inheritance**. we can make out Student class a sub-class by

```ruby
class Student < ActiveRecord::Base
end
```

Our **Student** class in now  a gateway of **talking** to the **student table** in the database. Now student class has a bunch of methods it can use.