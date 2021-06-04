## What Is Tux

**Tux is** an incredible Ruby gem that lets you access your database and perform all **CRUD**(Create, Read, Update, Delete) operations on it through the terminal.  It also loads a full environment in the console that **allows you to see all routes and views**

## Using Tux

### Create

```ruby
user = User.create(:name => "Trisha", :email => "trisha@test.com", :fav_icecream => "mint chocolate chip")
```

Or:

```ruby
user = User.new
user.name = "Beth"
user.email = "beth@beth.com"
user.fav_icecream = "rocky road"
user.save
```

#### Edit

```ruby
user = User.first
user.name = "Trisha Yearwood"
user.save
```

### Delete

```ruby
user = User.first
user.delete
```

### Search for Specific Users

All the `find` methods work in Tux too!

```ruby
user = User.find_by_id(2)
user = User.find_by(:name => "Beth")
user = User.first
user = User.last
```

