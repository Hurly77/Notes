## What are Active Record Associations?

Active Record associations allow us to associate models *and their analogous database tables* without having to write tons of code.

### Migration

- Run `mkdir db` and then `mkdir db/migrate` to create the `migrate` folder within `db`. Then create a file in the `db/migrate` folder called `001_create_shows.rb`. In this file, write the migration code to create a `shows` table. The table should have `name`, `network`, `day`, and `rating` columns. `name`, `network`, and `day` have a datatype of string, and `rating` has a datatype of integer.
- Create an `app` folder with a `models` folder within it, and then create a file, `show.rb`, in `app/models`. In this file, you will define a `Show` class that inherits from `ActiveRecord::Base`.
- Now we need to create a second migration to add another column to our `shows` table. In the `db/migrate` folder, create another file, `002_add_season_to_shows.rb`, and write a migration to add a column, `season`, to the `shows` table. The datatype of this column is string.

### Methods

```ruby
class Show < ActiveRecord::Base
  def self.highest_rating
    Show.maximum(:rating) #used maximum on Show and attribute :rating
  end

  def self.most_popular_show
    Show.all.find {|obj| obj.rating == self.highest_rating}
      #used find iterator to find the instance .highest_rationg method
  end

  def self.lowest_rating
    Show.minimum(:rating) #the Vice of Maximum
  end

  def self.least_popular_show
    Show.all.find {|obj| obj.rating == self.lowest_rating} # Vice
  end

  def self.ratings_sum 
    Show.sum(:rating)# used self.sum(:key) grabs the sum of all instances ratings
  end

 def self.popular_shows
  Show.where("rating > ?", 5) #grabs instances greater than 5
 end
  
  def self.shows_by_alphabetical_order
    Show.order("name") #orders the name, the order method automatically alphabetizes whatever arg is passed in
  end

end
```

