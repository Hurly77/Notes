 For efficiency's sake, Rails aliased the `generate` method to `g`, so the CLI command above could be shortened to:

```
rails g <name of generator> <options> --no-test-framework
```

## Different types of generators

Below are the main generators that Rails offers. We'll go through examples of each:

- Migrations
- Models
- Controllers
- Resources

## Migration Generators

To add a new column called `published_status`, we can use the following command:

```
rails g migration add_published_status_to_posts published_status:string --no-test-framework
```

**generator prepends** a timestamp before the file name. In the case of the migration I just ran, it added `20151127174031`. You can break this timestamp down as follows: `year: 2015, month: 11, date: 27, and then the time itself`.

It should look something like this:

```ruby
class AddPublishedStatusToPosts < ActiveRecord::Migration  
    def change    
        add_column :posts, :published_status, :string  
    end
end
```

Oh no, we made a mistake, let's get rid of that column name with another migration:

```
rails g migration remove_published_status_from_posts published_status:string --no-test-framework
```

```ruby
class RemovePublishedStatusFromPosts < ActiveRecord::Migration
  def change
    remove_column :posts, :published_status, :string
  end
end
```

Let's walk through a real world scenario:

```ruby
"(commmand)rails g migration add_post_status_to_posts post_status:boolean --no-test-framework"
#With this migration we'll add the column post_status with the data type of boolean

#This won't automatically create the change_column method; the file will look something like this:
class ChangePostStatusDataTypeToPosts < ActiveRecord::Migration
  def change
  end
end
#We can simply add in the change_column method like this: 
"change_column :posts, :post_status, :string" #and after running rake db:migrate our schema will be updated.
```

## Model Generators

new model to the app called `Author` with columns `name`, `bio`, and `genre`, we can use the model generator with the following CLI command:

`rails g model Author name:string genre:string bio:text --no-test-framework`

Running this generator will create the following files for us:

```
invoke  active_record
create    db/migrate/20190618010724_create_authors.rb
create    app/models/application_record.rb
create    app/models/author.rb
```

At a high level, this has created:

- A database migration that will add a table and add the columns `name`, `genre`, and `bio`.
- A model file that will inherit from `ApplicationRecord` (as of Rails 5)

**Note:** Up to Rails 4.2, all models inherited from `ActiveRecord::Base`. Since Rails 5, all models inherit from `ApplicationRecord`. If you've used an older version of Rails in the past, you may be wondering what happened to `ActiveRecord::Base`? 

Well, not a lot has changed, actually. This file is automatically added to models in Rails 5 applications:

```ruby
# app/models/application_record.rb
class ApplicationRecord < ActiveRecord::Base
  self.abstract_class = true
end
```

```
(command "rake db:migrate")
Author.all
=> #<ActiveRecord::Relation []>
 
Author.create!(name: "Stephen King", genre: "Horror", bio: "Bio details go here")
=> #<Author id: 1, name: "Stephen King", genre: "Horror", bio: "Bio details go here", created_at: "2015-11-27 22:59:14", updated_at: "2015-11-27 22:59:14">
```

## Controller Generators

Controller generators **are great if** you are creating **static views** or **non-CRUD** related features 

`rails g controller admin dashboard stats financials settings --no-test-framework`

This will create a ton of code! Below is the full list:

```
create  app/controllers/admin_controller.rb
 route  get 'admin/settings'
 route  get 'admin/financials'
 route  get 'admin/stats'
 route  get 'admin/dashboard'
invoke  erb
create    app/views/admin
create    app/views/admin/dashboard.html.erb
create    app/views/admin/stats.html.erb
create    app/views/admin/financials.html.erb
create    app/views/admin/settings.html.erb
invoke  helper
create    app/helpers/admin_helper.rb
invoke  assets
invoke    coffee
create      app/assets/javascripts/admin.js.coffee
invoke    scss
create      app/assets/stylesheets/admin.css.scss
```

what got **added** here

- A controller file that will inherit from `ApplicationController`
- A set of routes to each of the generator arguments: `dashboard`, `stats`, `financials`, and `settings`
- A new directory for all of the view templates along with a view template file for each of the controller actions that we declared in the generator command
- A view helper method file
- A Coffeescript file for specific JavaScripts for that controller
- An `scss` file for the styles for the controller

## Resource Generators

**If you are** building an **API**, using a **front end MVC framework,** or simply want to manually **create your views**, the `resource` generator is a great option for creating the code. 

build Account controller: `rails g resource Account name:string payment_status:string --no-test-framework`

This creates quite a bit of code for us. Below is the full list:

```
invoke  active_record
create    db/migrate/20170712011124_create_accounts.rb
create    app/models/account.rb
invoke  controller
create    app/controllers/accounts_controller.rb
invoke    erb
create      app/views/accounts
invoke    helper
create      app/helpers/accounts_helper.rb
invoke    assets
invoke      coffee
create        app/assets/javascripts/accounts.js.coffee
invoke      scss
create        app/assets/stylesheets/accounts.css.scss
invoke  resource_route
 route    resources :accounts
```

our app have now due to the generator

- A migration file that will create a new database table for the attributes passed to it in the generator
- A model file that inherits from `ApplicationRecord` (as of Rails 5; see Note above)
- A controller file that inherits from `ApplicationController`
- A view directory, but no view template files
- A view helper file
- A Coffeescript file for specific JavaScripts for that controller
- A `scss` file for the styles for the controller
- A full `resources` call in the `routes.rb` file

The `resource` generator is a smart generator that **creates** some of the **core functionality** needed for a full featured resource **without much code bloat.**

`rake routes`

`rake routes | grep account`

This `rake` command will produce the following output in the console:

```
accounts      GET    /accounts(.:format)          accounts#index
              POST   /accounts(.:format)          accounts#create
new_account   GET    /accounts/new(.:format)      accounts#new
edit_account  GET    /accounts/:id/edit(.:format) accounts#edit
account       GET    /accounts/:id(.:format)      accounts#show
              PATCH  /accounts/:id(.:format)      accounts#update
              PUT    /accounts/:id(.:format)      accounts#update
              DELETE /accounts/:id(.:format)      accounts#destroy
```

open up the `accounts_controller.rb` none of the actions shown in the route list are even there