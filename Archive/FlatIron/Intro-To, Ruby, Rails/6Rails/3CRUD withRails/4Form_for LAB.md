##### routes

```ruby
Rails.application.routes.draw do
  # For details on the DSL available within this file, see http://guides.rubyonrails.org/routing.html
resources :students, only: [:new, :edit, :show, :index, :create, :edit, :update]
resources :school_classes, only: [:new, :edit, :show, :index, :create, :edit, :update]
end

```

##### models

```ruby
class Student < ActiveRecord::Base
  def to_s
    self.first_name + " " + self.last_name
  end
end
```

```ruby
class SchoolClass < ActiveRecord::Base
  def klass
    self.title + " " + self.room_number.to_s
      #just a class the puts both the title and room_number
  end
end
```

##### db/migrate

```ruby
class CreateStudents < ActiveRecord::Migration
  def change
    create_table :students do |t|
      t.string :first_name
      t.string :last_name

      t.timestamps null: false
    end
  end
end

class CreateSchoolClasses < ActiveRecord::Migration
  def change
    create_table :school_classes do |t|
    t.string :title
    t.integer :room_number

    t.timestamps null: false
    end
  end
end

```

##### controllers

```ruby
class SchoolClassesController < ApplicationController
  def index
    @school_classes = SchoolClass.all
  end

  def new
    @school_class = SchoolClass.new
      #need to put new so you can access obj directly through the form_for
  end

  def create
    @school_class = SchoolClass.new(post_params(:title, :room_number))
      #passed in both attr because test never specified if one should not be allowen to update
    @school_class.save
    redirect_to school_class_path(@school_class)
  end

  def show
    @school_class = SchoolClass.find(params[:id])
  end

  def edit
    @school_class = SchoolClass.find(params[:id])
  end

  def update
    @school_class = SchoolClass.find(params[:id])
    @school_class.update(post_params(:title))
    redirect_to school_class_path(@school_class)
  end

  private

  def post_params(*args)
    params.require(:school_class).permit(*args)
  end
end
```

```ruby
class StudentsController < ApplicationController
  def index
    @students = Student.all  
  end

  def new    
    @student = Student.new
  end

  def create 
    @student = Student.new(post_params(:first_name, :last_name))
    @student.save
    redirect_to student_path(@student)
  end

  def show
    @student = Student.find(params[:id])
  end

  def edit
    @student = Student.find(params[:id])
  end

  def update
    @student = Student.find(params[:id])
    @student.update(post_params(:first_name, :last_name))
    redirect_to student_path(@student)
  end

  private

  def post_params(*args)
    params.require(:student).permit(*args)
  end
end
```

##### views/school_class

**edit**

```erb
<%= form_for(@school_class) do |f|  %>
    <%= f.label :title %>
    <%= f.text_field :title %>
    <%= f.label :room_number %>
    <%= f.text_field :room_number %>
    <%= f.submit "Update School class"%>
<% end %>
```

##### index

```erb
<div>
<% @school_classes.each do |school_class| %>
<div><%= link_to school_class.klass, school_class_path(school_class) %>
<% end %></div>
</div>
```

##### new

```ruby
<h1>School Class Form</h1>
<%= form_for(@school_class) do |f| %>
  <%= f.label :title %><br>
  <%= f.text_field :title %><br>
  <%= f.label :room_number %><br>
  <%= f.text_field :room_number %><br>
  <%= f.submit "Create School class" %>
<% end %>
```

##### show

```ruby
<h1><%= @school_class.klass %></h1>
```



##### views/student

**edit**

```ruby
<%= form_for(@student) do |f|  %>
    <%= f.label :first_name %>
    <%= f.text_field :first_name %>
    <%= f.label :last_name %>
    <%= f.text_field :last_name %>
     <%= f.submit "Update Student"%>
<% end %>
```

**index**

```ruby
<div>
<% @students.each do |student| %>
<div>
<%=link_to student.to_s, student_path(student)%>
</div>
<% end %>
</div>
```

**new**

```ruby
<h1>Student Form</h1>
<%= form_for(@student) do |f| %>
  <%= f.label :first_name %><br>
  <%= f.text_field :first_name %><br>
  <%= f.label :last_name %><br>
  <%= f.text_field :last_name %><br>
  <%= f.submit "Create Student" %>
<% end %>
```

**show**

```ruby
<h1> <%= @student.to_s %> </h1>
```

