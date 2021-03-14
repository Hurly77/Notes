## Instructions

1. Refactor the `_form.html.erb` partial to accept the argument to the `form_for` helper as a local. You'll also need to change the `new.html.erb` and `edit.html.erb` views as well.
2. Refactor the `_student.html.erb` partial to pass through each rendered student as a local.
3. On the classroom show page, iterate through each classroom's students and display each of them using our `_student.html.erb` partial with locals.
4. Create a `_classroom.html.erb` partial to display classroom information on the classroom show page.
5. Add in search functionality such that users can type in a student name or fragment of a student name and and see all matching results on the students index page. The results should be displayed by rendering a `students/_student.html.erb` partial. This will require you to do a "fuzzy" or "wildcard" search in the controller in order to create the set of matches. To help you out, you'll want to write a flexibly matching (or "wildcard") query in ActiveRecord that follows the form: `Student.where("name LIKE ?", "%M%")`. You should be able to visually test this by visiting `http://localhost:3000?query="search_text"`. Doing so will mean that you will have a `query` parameter whose data can be fit into a `LIKE` query.

# lab

models

```ruby
class Classroom < ActiveRecord::Base
  has_many :classroom_students
  has_many :students, through: :classroom_students

  def oldest_student
    students.where("birthday is not null").order("birthday asc").first
  end
end
```

```ruby
class ClassroomStudent < ActiveRecord::Base
  belongs_to :classroom
  belongs_to :student
end
```

```ruby
class Student < ActiveRecord::Base
  has_many :classroom_students
  has_many :classrooms, through: :classroom_students

  def self.search(query)
    if query.present?
    self.where("NAME LIKE ?", "%#{query}%")
       #the "%#{query}%" sanatixes the string which basically means that it makes sure no css or html are included when you query the record.
    else
      self.all
        #if you don't put this, than when you render the index view it will be expection that you have already entered and input, so this returns all the students.
    end
  end

end
```

## views

### classrooms

###### classrooms/_classroom

```erb
<p>
<%= @classroom.course_name %>
<%= @classroom.semester %>
</p>
```

###### classrooms/index

```

```

###### classrooms/show

```erb
Classroom Info

<%= render partial: 'classroom', locals: {classroom: @classroom} %>
<!--here we have a set of key value pairs, partial, is a method the acceptes a value, classroom and checks the current level for the view called _classroom
locals: is a method the accepts the value of a hash {key: value} classroom is the method wich accepts and Id to render the current show page for that classroom-->

Students:
<div>
    <%@classroom.students.each do |student|%>
    #<!--here we iterate of the proxy collection students,-->
        <%= render partial: 'students/student', locals: {student: student} %>
    <!--then we then we tell partial we want to render the student view in folder students, and locals accepts the value of student, a local variable, wich will be called each for each obj in the students proxycollection.  -->
    <%end%>
</div>
```

### layouts

application

```erb
<!DOCTYPE html>
<html>
<head>
  <title>Educator</title>
  <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track' => true %>
  <%= javascript_include_tag 'application', 'data-turbolinks-track' => true %>
  <%= csrf_meta_tags %>
</head>
<body>

<%= yield %>

</body>
</html>
```

### students

_form

```erb
<%= form_for (student) do |f|%>
<p>
  <%= f.label :name %>
  <%= f.text_field :name %>
</p>
<p>
  <%= f.label :hometown %>
  <%= f.text_field :hometown %>
</p>
<p>
  <%= f.label :birthday %>
  <%= f.date_field :birthday %>
</p>
  <%= f.submit %>
<% end %>
```

_student

```erb
<ul>
  <li>
    Name: <%= student.name %>
      <!--when this page is rendered, the locals hash, should have a key with the same name as the variable in the views it is rendering, and should point to and object. that way the above code will be useing the method name on a variable equal to and object so it will return the value of that attribute-->
  </li>
  <li>
    Birthday: <%= student.birthday.strftime("%m/%d/%Y")  %>
  </li>
  <li>
    Hometown: <%= student.hometown.capitalize %>
  </li>
</ul>
```

edit

```erb
<%= render partial: "form", locals:{student: @student} %>
```

index

```erb
<%= form_tag students_path, method: :get do %>
<!--here where calling the students path and telling active record it is a get request-->
<p>
  <%= text_field_tag :query, params[:query] %>
    <!--here we set :query attribute equal to the passed in string, then we call params[:query] so the default value after search is whatever was passed in last-->
  <%= submit_tag"Search", name: nil %>
  </p>
<% end %>


<% @students.each do |student| %>
<!--after our seach method queries the database, what every objects, return will be iterated-->
<%= render partial: "student", locals: {student: student} %>
<!--then render the partial, and the local, REMEBER the student: is a local variable equal to a obj in the collection, and student is is the value of that variable, so what ever object is is passed in will be the object when the views is rendered-->
<% end %>
```

new

```erb
<%= render partial: "form", locals: {student: @student } %>
```

show

```erb
<h1>Student Info</h1>

<%= render partial: "student", locals: {student: @student} %>
```

## controllers

classroom:

```ruby
class ClassroomsController < ApplicationController
  def show
    @classroom = Classroom.find(params[:id])
    
  end

  def index
    @classrooms = Classroom.all
  end
end
```

student:

```ruby
class StudentsController < ApplicationController
  def new
    @student = Student.new
  end

  def create
    @student = Student.new(student_params)
    if @student.save
      redirect_to @student
    else
      render 'new'
    end
  end

  def edit
    @student = Student.find(params[:id])
  end

  def show
    @student = Student.find(params[:id])
  end

  def index
    @students = Student.search(params[:query])
    render 'index'
      #did not need this render 'index'
  end

  def student_params
    params.require(:student).permit(:name, :birthday, :hometown)
  end
end
```

## routes

```ruby
Rails.application.routes.draw do
  resources :classrooms
  resources :students
end
```

