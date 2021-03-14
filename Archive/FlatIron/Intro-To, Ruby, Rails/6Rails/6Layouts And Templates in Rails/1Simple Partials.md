```erb
<%= form_tag posts_path do %>
<%= render 'form' %>
  <% end %>
```

```erb
<h2>Post Form</h2>
<%= form_tag post_path(@post), method: "put" do %>
<%= render 'form' %>
<% end %>
```

```erb
<label>Post title:</label><br>
<%= text_field_tag :title, @post.title %><br>
 
<label>Post Description</label><br>
<%= text_area_tag :description, @post.description %><br>
 
<%= submit_tag "Submit Post" %>
```

```erb
<%= render 'author' %>
```

```erb
<%= @author.name %>
<%= @author.hometown %>
```

```ruby
class PostsController < ApplicationController
  def index
    @posts = Post.all
  end

  def show
    @post = Post.find(params[:id])
    @author = @post.author
  end

  def new
    @post = Post.new
  end

  def create
    @author = Author.first
    @post = Post.new
    @post.title = params[:title]
    @post.description = params[:description]
    @post.author_id = @author.id
    @post.save
    redirect_to post_path(@post)
  end

  def edit
    @post = Post.find(params[:id])
  end

  def update
    @post = Post.find(params[:id])
    @post.update(title: params[:title], description: params[:description])
    redirect_to post_path(@post)
  end
  
end
```

# lab

1. Remove the duplicated code in the `students/edit.html.erb` and `students/new.html.erb` files by making a partial called `students/_form.html.erb`.
2. Remove the duplicated code in the `classrooms/show.html.erb` and `students/show.html.erb` files by making a partial called `students/_student.html.erb`.

### views

students/new

```erb
<%= render 'form' %>
```

students/edit

```erb
<%= render 'form' %>
```

students/show

```erb
<h1>Student Info</h1>
<%= render 'student' %>
```

classrooms/show

```erb
Classroom Info
<p>
<%= @classroom.course_name %>
<%= @classroom.semester %>
</p>

We now want to call out the oldest student in the class:
<%= render 'students/student' %>

```

## partials

students/_form

```erb
<%= form_for @student do |f|%>
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

students/_student

```erb
<h1>Student Info</h1>
<ul>
  <li>
    Name: <%= @student.name %>
  </li>
  <li>
    Birthday: <%= @student.birthday.strftime("%m/%d/%Y")  %>
  </li>
  <li>
    Hometown: <%= @student.hometown.capitalize %>
  </li>
</ul>
```

controllers

```ruby
#app/controllers/class_rooms_controller
class ClassroomsController < ApplicationController
  def show
    @classroom = Classroom.find(params[:id])
    @student = @classroom.oldest_student
  end

  def index
    @classrooms = Classroom.all
  end
end

-------------------------------------------------
#app/controllers/students_controller
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
    @students = Student.all
  end

  def student_params
    params.require(:student).permit(:name, :birthday, :hometown)
  end
end

```

