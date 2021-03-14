# lab

```ruby
Rails.application.routes.draw do
resources :students, only: [:index, :show]
get "students/:id/activate", to: "students#activate", as: "activate_student"
    #The test was looking for the method activate_student so I had to change i to fit the test suite
end

```

```erb
#app/views/students/activate
<%if !@student.active %> 
<%="This student is currently active."%>
<% else @student.active %> 
<%="This student is currently inactive."%>
<% end %>
```

```erb
#app/views/students/index
<div>
  <% @students.each do |student| %>
    <div><%= link_to student.first_name + " " + student.last_name, student_path(student) %></div>
  <% end %>
</div>
```

```erb
#app/views/students/show
<h1><%= @student.to_s %></h1>
<%if @student.active%> 
<%="This student is currently active."%>
<% else !@student.active %> 
<%="This student is currently inactive."%>
<% end %>
```

```ruby
class StudentsController < ApplicationController
  before_action :set_student, only: :show
    #This gives the show action access to the private method set_sudent
  
  def index
    @students = Student.all
      #renders the index page with and instance variable = all students
  end

  def show
      #this just renders the show page
  end

  def activate
    @student = Student.find(params[:id])
      #use AR method find to find obj with id matching prams
    if @student.active #the active attribute is set to default at false
      @student.active = false
    else #its boolean so if it is not false it is true
      @student.active = true
    end
    @student.save #then we have to persist the new info to our database
    redirect_to student_path(@student)
      #student_path is a method we inherit when we create the route /students/:id(or at least that is all I can determin from here)
  end

  private
#This is a private method(I believe), that gives whatever action it is set to access all methods under private, so the show can have access to the specific student route. also not sure of its intended use because I don't see why this had to be used here
    def set_student
      @student = Student.find(params[:id])
    end
end
```

