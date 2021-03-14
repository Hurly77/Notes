

# Forms That Create Multiple Objects

#### The Models

To **create** these two different classes of **objects**, we need to **create** two **models**, `Student` and `Course`.

#### `Student` class

```ruby
class Student
  attr_reader :name, :grade
 
 @@all = []
 
  def initialize(params)
    @name = params[:name]
    @grade = params[:grade]
    @@all << self
  end
 
  def self.all
    @@all
  end
 
end
```

#### `Course` class

```ruby
class Course
  attr_reader :name, :topic
 
  COURSES = []
 
  def initialize(args)
    @name = args[:name]
    @topic = args[:topic]
    COURSES << self
  end
 
  def self.all
    COURSES
  end
end
```

### Creating the Form

**Typically**, if we were just doing student information, we would expect the `params` hash to look something like this:

```RUBY
params = {
  "name" => "Joe",
  "grade" => "9"
}
```

 we need to think about restructuring our `params` hash to have nested hashes. We can have one hash for all of the student information:

```RUBY
params = {
  "student" => {
    "name" => "Joe",
    "grade" => "9",
  }
}
```

we create a hash like this in Ruby? Like so:

```ruby
my_hash = {}
my_hash["student"] = {}
my_hash["student"]["name"] = "Joe"
```

Thankfully, ERB provides a similar syntax. It handles that first level of nesting, so instead of having to do `my_hash["student"]={}` we can just go straight into the `student` hash. ERB assumes that the name of your top-level hash is the first key, so the code to call the value associated with the nested `"name"` key would be `student["name"]`.

build out the HTML for this form:

```html
<form action="/student" method="post">
  Student Name: <input type="text" name="student[name]">
  Student Grade: <input type="text" name="student[grade]">
  <input type="submit">
</form>
```

we've named the action `/student`  name` attribute of the `input` tag is set up as `student[name] This way, when the form gets submitted, the `params` sent to the `/student` controller action will look exactly as we planned.

```ruby
params = {
  "student" => {
    "name" => "Joe",
    "grade" => "9",
    "course" => {
      "name" => "US History",
      "topic" => "History"
    }
  }
}
```

`student` and `course` can have the key `name` because they're in different namespaces. **build this hash using Ruby:**

```ruby
#we don't want to do this
my_hash = {}
my_hash["student"] = {}
my_hash["student"]["name"] = "Joe"
my_hash["student"]["course"] = {}
my_hash["student"]["course"]["name"] = "US History"
my_hash["student"]["course"]["topic"] = "History"
 
my_hash
  => {"student"=>{"name"=>"Joe", "course"=>{"name"=>"US History", "topic"=>"History"}}}
```

 use the ERB syntax to set up our form`my_hash["student"]["course"]["name"]` into `student[course][name]`.

build out the corresponding HTML for the form:

```html
<form action="/student" method="post">
  Student Name: <input type="text" name="student[name]">
  Student Grade: <input type="text" name="student[grade]">
  Course Name: <input type="text" name="student[course][name]">
  Course Topic: <input type="text" name="student[course][topic]">
  <input type="submit">
</form>
```

#### How we want our hasch and form to look

How do we handle **two** (or more!) courses?  We need to once again restructure how we want to store data in the `params` hash.

```ruby
params = {
  "student" => {
    "name" => "Vic",
    "grade" => "12",
    "courses" => [#key that points to an array of hashes
      {
        "name" => "AP US History",
        "topic" => "History"
      },
      {
        "name" => "AP Human Geography",
        "topic" => "History"
      }
    ]
  }
}
# It's much simpler than creating a new key for each course
```

```ruby
<form action="/student" method="post">
  Student Name: <input type="text" name="student[name]">
  Student Grade: <input type="text" name="student[grade]">
  Course Name: <input type="text" name="student[courses][][name]">
  Course Topic: <input type="text" name="student[courses][][topic]">
  Course Name: <input type="text" name="student[courses][][name]">
  Course Topic: <input type="text" name="student[courses][][topic]">
  <input type="submit">
</form>
```

We removed the singular `student[course][name]` and `student[course][topic]` inputs and replaced them with pairs of inputs that allow for the creation of TWO courses.  we've **nested** each **student's** **courses** within their primary **hash**.

This is where ERB syntax differs from Ruby. In Ruby, if you wanted a hash to store an array, you would do something like this:

```ruby
my_hash = {}
my_hash["student"] = {}
my_hash["student"]["name"] = "Joe"
my_hash["student"]["courses"] = []
my_hash["student"]["courses"][0] = { "name" => "AP US History", "topic" => "History"}
my_hash["student"]["courses"][1] = { "name" => "AP Human Geography", "topic" => "History"}
#To access the name of the first course, you would do something like:
my_hash["student"]["courses"][0]["name"]
  => "AP US Histor
```

ERB makes it even easier on us  **Instead** we can use an **empty array** (`[]`) in our form view, and **ERB will automagically index the array for us,** turning `my_hash["student"]["courses"][0]["name"]` into `student[courses][][name]`

## The Display View

##### student.erb

```erb
<h1>Student</h1>
 
<div class="student">
  <h3>Name: <%= @student.name %></h3><br>
  <h4>Grade: <%= @student.grade %></h4>
</div><br>
 
<h1>Courses</h1>
<% @courses.each do |course| %>
  <div class="course">
    <p>Name: <%= course.name %></p><br>
    <p>Topic: <%= course.topic %></p><br>
  </div><br>
<% end %>
```

## The Controller

```ruby
get '/' do
  erb :new
end
```

##### `POST` request:

```ruby
post '/student' do
  @student = Student.new(params[:student])
 
  params[:student][:courses].each do |details|
    Course.new(details)
  end
 
  @courses = Course.all
 
  erb :student
end
```

In this controller action, we first create a new `Student` using the info stored in `params[:student]`, which contains the student's `name`, `grade`, and `courses`.

Then we iterate over `params[:student][:courses]`, which is an array containing a series of hashes that each store individual course information:

```ruby
[
  0 => {
    "name" => "AP US History",
    "topic" => "History"
  },
  1 => {
    "name" => "AP Human Geography",
    "topic" => "History"
  }
]
```

