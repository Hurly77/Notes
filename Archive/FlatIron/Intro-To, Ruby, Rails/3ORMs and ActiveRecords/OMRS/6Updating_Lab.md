```ruby
require_relative "../config/environment.rb"
#at the begning of the lab ran into a problem where it didn't know about my data base fixed this by just deleting /students.db and making a new one
class Student
  
attr_accessor :name, :grade

attr_reader :id # don't forget you need to put the reader

def initialize(id=nil, name, grade)
  @id = id
  @name = name
  @grade = grade
end

def self.create_table
  sql = <<-SQL	#
  CREATE TABLE IF NOT EXISTS students ( #need to remember to put "IF NOT EXISTS"
    id INTEGER PRIMARY KEY,
    name TEXT,
    grade TEXT
  )
  SQL

  DB[:conn].execute(sql)
end

def self.drop_table
  DB[:conn].execute("DROP TABLE IF EXISTS students")
end

def save
  if self.id
    self.update
  else
  sql = <<-SQL  #
  INSERT INTO students(name, grade)
  Values (?, ?)
  SQL

  DB[:conn].execute(sql, self.name, self.grade)
  @id = DB[:conn].execute("SELECT last_insert_rowid() FROM students")[0][0]
  end
end

def self.create(name, grade) #FOR SOME REASON i DIDN'T NEED TO PUT THE :
  student = Student.new(name, grade)
  student.save
  student  
end

def self.new_from_db(row)
  new_student = self.new(row[0], row[1], row[2]) #Found way to refator this with out putting new_student a 100 times also the test wouldn't pass with out it 
  new_student
end

def self.find_by_name(name)
  sql = <<-SQL			#
  SELECT *
  FROM students
  WHERE name = ?
  LIMIT 1
  SQL

  DB[:conn].execute(sql, name).map do |row|
    self.new_from_db(row)
    end.first
  end

  def update
    sql = "UPDATE students SET name = ?, grade = ? WHERE id = ?"
    DB[:conn].execute(sql, self.name, self.grade, self.id)
  end

end
```

