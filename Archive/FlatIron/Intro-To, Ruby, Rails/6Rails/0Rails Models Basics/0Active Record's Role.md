## Active Record's Role

Behavior Driven Development (**BDD**)

As a professional Rails developer, you will be expected to build applications by leveraging a [BDD](http://rspec.info/) process, so we will walk through how to build each feature with a test-first approach so that the tests can lead our development.

**To generate this app, we installed the Rails gem, then ran**

```bash
# the -T flag tells the Rails project generator not to
# include TestUnit, the default testing framework:
rails new rails-activerecord-models-and-rails-readme -T
 
# The Rails project generator created this directory for us:
cd rails-activerecord-models-and-rails-readme
 
# We modified the Gemfile to include
# gem 'rspec-rails', '~> 3.0'
# in the :development, :test group, then ran:
 
bundle install
 
# Finally, we created the initial RSpec config:
rails g rspec:install
```

create a new file: `spec/models/post_spec.rb`

```ruby
require 'rails_helper'
 
describe Post do
 
end
```

If we run `bundle exec rspec`, it will throw an error

new file in the `app/models` directory called `post.rb`

```ruby
class Post
end
```

There is no need to create a database with `rake db:create` first. The test suite will create a test database for us when we run our tests.

```ruby
#update the Post spec to test for a Post being created.
describe Post do
  it 'can be created' do
    post = Post.create!(title: "My title", description: "The post description")
    expect(post).to be_valid
  end
end
```

```ruby
# Create a new directory in the db/ directory called migrate, and add a new file called 001_create_posts.rb. To that file, add the following code:

class CreatePosts < ActiveRecord::Migration
  def change
    create_table :posts do |t|
      t.string :title
      t.text :description
 
      t.timestamps null: false
        #The timestamp also plays a role in making sure that only new migrations run when we run rake db:migrate
    end
  end
end
```

**important :** The `db/schema.rb` file is updated with a version number corresponding to the timestamp of the last migration you ran. When you run `rake db:migrate` again, only migrations whose timestamps are greater than the schema's version number will run.

Open up the **Rails console** by running `rails console`. Running the console will load the entire Rails environment and give you command line access to the app and the database. 

```ruby
#add a new feature that returns a summary of a post;by creating a spec for the feature:
it 'has a summary' do
  post = Post.create!(title: "My title", description: "The post description")
  expect(post.post_summary).to eq("My title - The post description")
end
#if we run this, we'll get a failure since we do not have a post_summary method for Post
#in App/models/post_summary
def post_summary
end
#This now results in a failure since the method currently doesn't return anything
def post_summary
  self.title + " - " + self.description
end

```

As you may have noticed, **we did not have to create a controller, route, view**, etc,. The data aspect of the application c**an work separately from the view and data flow logic.**

**With that being said**, it is considered a best practice to have your controller and view files follow the proper naming convention so that the MVC associations are readable

For example, to build out the controller and view code for our `Post` model, we would create the following structure:

- Create a `posts_controller.rb` file that calls on the `Post` model
- Create a `views/posts/` directory that stores the views related to the `Post` model

##### futher more



Also, if you are coming from other programming languages **you may be wondering how exactly we are able to connect to the database automatically without having to create connection strings.** The reason for this simplicity **resides in the** `config/database.yml` file that was **generated when** we created our application and **ran** `rake db:create`. In that file, you will see that the development, test, and production databases are all configured. From that stage, the `ActiveRecord::Base.connection` **method connects your application to the database,** which is another benefit of having our model classes inherit from the `ActiveRecord::Base` module.

Being able to work in different environments is one of the strong points of Rails, and the database.yml file takes advantage of this feature by having different database options for each type of environment. If you take a look at the file, you can see that you can establish different database adapters, pools, timeout values, etc. for each environment specifically. This allows for you to have a setup such as using SQLite locally and Postgres in production, along with having a segmented database environment for your testing suite. **Some of these items are components that you won't need until you get into more advanced applications. However**, it's good to know where these items are located in the file system for when you get to that point. Essentially, this file includes a lot of stuff you will rarely have to handle, but just remember that if anything requires database configuration it will be here.