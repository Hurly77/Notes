## What is Rake?

**rake** is  a **tool** in **Ruby** that allows us to **automate** certain **jobs**. This could be anything from **SQL** to **puts** in a terminal. we can define Rake tasks that execute jobs.

## Why Rake?

**Rake** provides us a **standard**, conventional way to **define** and **execute** such **tasks** using **Ruby**. **Without** having to use **bash**.

## How to Define and Use Rake Tasks

Rake task is easy, since Rake is already available to us as a part of Ruby. create a file 

in the top level of our directory called `Rakefile`

```ruby
desc 'description of the task'
task :hello do
  # the code we want to be executed by this task
end
```

### Describing our Tasks for `rake -T`

**rake -T** -to view a list of available Rake tasks  to work though we need a **task description.** 

```ruby
rake hello       # outputs hello to the terminal
```

### Namespaceing Rake Tasks

A **namespace** is really just a way to group or contain something.

```ruby
#defind another rake task Then
namespace :greeting do
    #tasks that you have definded
end
```

## Common Rake Tasks

### rake db:migrate

We'll call this task **migrate**, because it is a **convention** to say we are "migrating" our **database** by applying **SQL** statements that **alter** that database.

```ruby
namespace :db do
  desc 'migrate changes to your database'
  task :migrate => :environment do
    Student.create_table
  end
end
#But, if we run **rake db:migrate** now, we're going to hit an **error.**
#You might be wondering what is happening with this snippet:

task :migrate => :environment do
...
```

This creates a ***task dependency***. Since our **Student.create_table** code would require access to the **config/environment.rb** file (which is where the student class and database are loaded), we need to **give** our **task** **access** to this **file**. In order to do that, we **need** to define yet **another** **Rake** task that we can tell to **run** **before** the `**migrate** Task is run.

### environment Task

```ruby
# in Rakefile
 
task :environment do
  require_relative './config/environment'
end
#After adding our environment task, running rake db:migrate should create our students table.
```

### rake db:seed

his task is responsible for "seeding" our **database** with some **dummy data.**

The **conventional** way to **seed** your database is to have a file in the **db** directory

```ruby
require_relative "../lib/student.rb"
 
Student.create(name: "Melissa", grade: "10th")
Student.create(name: "April", grade: "10th")
Student.create(name: "Luke", grade: "9th")
Student.create(name: "Devon", grade: "11th")
Student.create(name: "Sarah", grade: "10th")
```

Then, we **define** a **rake task** that executes the code named under  **namespace :db** 

```ruby
#don't forget to run rake db:migrate first
  desc 'seed the database with some dummy data'
  task :seed do
    require_relative './db/seeds.rb'
  end
#Now, if we run rake db:seed in our terminal we will insert five records into the database.
```

### rake console

We'll define a task that starts up the Pry console.

```ruby
desc 'drop into the Pry console'
task :console => :environment do #this makes it dependent upon the enviroment task so that Student class and the database connection load first
  Pry.start
end
rake console 
[1] pry(main)>
    [1] pry(main)> Student.all
=> [[1, "Melissa", "10th"],
 [2, "April", "10th"],
 [3, "Luke", "9th"],
 [4, "Devon", "11th"],
 [5, "Sarah", "10th"]]
```

