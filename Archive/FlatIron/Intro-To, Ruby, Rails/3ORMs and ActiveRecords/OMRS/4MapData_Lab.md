

## Mapping Database Rows to Objects Lab** NOTES:

### .new_from_db

```ruby
#WE used .attr so we can access the attr value by [local_varible.attr] => attr_value. 
new_student = self.new #create a new instance of our klass
    new_student.id = row[0] #make attr = row[index_number] 
    new_student.name = row[1] 
    new_student.grade = row[2]
    new_student # returns an object of klass
```

### self.all

```ruby
def self.all
# Ingnore #
    sql = #<<-SQL   open a heredoc
    SELECT * 
    FROM students
    SQL # close here doc
    DB[:conn].execute(sql) #this will "execute the sql and connect to our database"
    .map |row| # iterates of raw data
    self.new_from_db(row) #then with the raw data we instantiate a new instance of studet whit the .new_from_db(row) method 
  end
  end
```

### .find_by_name(name)

```ruby
def self.find_by_name(name)
    self.all.find {|row| row.name == name} #Here we used the .all method to access each object or row and grabbed the name or column == to the argument.
end
```

The **ABOVE** is refoctored from what the lesson said wich looked like this:

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

### .all_students_in_grade_9

```ruby
def self.all_students_in_grade_9
    self.all.find_all {|obj| obj.grade == '9'} # We used the same principle as we did from .find_by_name method but we used the find_all iterator instead 
  end
```

**self.students_below_12th_grade** 

```ruby
self.all.find_all  {|obj| obj.grade < '12'} # not sure why Find_all works and not map but this uses a compason and returns the true value and the object in an array
```

 **.first_X_students_in_grade_10(x)**

```ruby
self.all.map.first(x) {|obj| obj.grade == '10'} #I used the "first" array-method to grabe the first value in wich the block returned true
```

**self.first_student_in_grade_10**

```ruby
self.all.find {|student| student.grade == '10'} #same as the .find_by_name method
```

**self.all_students_in_grade_X(x)**

```ruby
self.all.find_all {|students| students.grade = x} # this I may have cheated but, not sure if I'm right but I think it is setting the grade to x
#however I need to look futher into it.
```

