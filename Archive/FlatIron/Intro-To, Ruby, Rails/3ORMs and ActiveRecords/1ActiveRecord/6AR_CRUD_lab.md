### Create Table

- **use the rake task** `rake db:create_migration NAME=create_movies`

- Then create table in **db/create_movies**

- **rake db:migrate**

- **rake db:migrate SINATRA_ENV=test and run learn**

  ```ruby
  #db/migrate
  class CreateMovies < ActiveRecord::Migration[5.2]
    def change
      create_table :movies do |t|
        t.string :title
        t.integer :release_date 
        t.string :director
        t.string :lead
        t.boolean :in_theaters
      end
    end
  end
  ```

  

#### Create

```ruby
def can_be_instantiated_and_then_saved #here I reverenced mechanics of migration in notes
  movie = Movie.new  ##make a new instanve of the Movie classs
  movie.title = "This is a title." ## set move-title to "this is a title"
  movie.save #than I persisted it to the data base
end

def can_be_created_with_a_hash_of_attributes
  # Initialize movie and then and save it
  attributes = {
      title: "The Sting",
      release_date: 1973,
      director: "George Roy Hill",
      lead: "Paul Newman",
      in_theaters: false
  } # created a hash called attributes 
  movie = Movie.create(attributes) # With create method movie is both instantiated and saved at the same time Thanks to AR::Base
  movie ##then return movie, this return is known as implicit return
end

#GOT STUMPED BY THIS ONE, args need to be equal to a hash with the default values passed in.
def can_be_created_in_a_block(args = {title: "Home Alone", release_date: 1990})
  # If no arguments are passed, use default values: 
  # title == 
  # release_date == 
  Movie.create(args)#It will set title and release date to default unless a hash-argument is passed in.
end
```



#### Read

```ruby
def can_get_the_first_item_in_the_database
  Movie.all.first ## just used the first method to grab the frist element in the all array
end

def can_get_the_last_item_in_the_database
Movie.all.last #used Last method on all array
end

def can_get_size_of_the_database
  Movie.all.size # used size method on all array
end

def can_find_the_first_item_from_the_database_using_id
  Movie.all.find {|movies| movies.id == 1 } #used the find iterator to grab the id == 1
end

def can_find_by_multiple_attributes
  # Search Values:
  # title == "Title"
  # release_date == 2000
  # director == "Me"
  Movie.all.find_by(title: "Title", release_date: 2000, director: "Me") #used the find_by method provided by AR::Base passing in hash as an argument
end

# THIS ONE WAS TRICKY HAD TO USE THE AR GUIDE
def can_find_using_where_clause_and_be_sorted
  # For this test return all movies released after 2002 and ordered by 
  # release date descending
  Movie.where('release_date > 2002').order(release_date: :desc)
    #used the where method provided by AR::Base to grab movies newer than 2002 and the order method, also provided by AR::Base, in desending order 
end
```



#### Update

```ruby

def can_be_found_updated_and_saved
  # Updtate the title "Awesome Flick" to "Even Awesomer Flick", save it, then return it
  Movie.create(title: "Awesome Flick")
  movie = Movie.find_by(title: "Awesome Flick").update(title: "Even Awesomer Flick")
    #used find_by and update methods provided by AR::Base to find  title and update it
end


def can_update_using_update_method
  # Update movie title to "Wat, huh?"
  Movie.create(title: "Wat?")
  Movie.find_by(title: "Wat?").update(title:"Wat, huh?")
end


def can_update_multiple_items_at_once
  # Change title of all movies to "A Movie"
  5.times do |i|
    Movie.create(title: "Movie_#{i}", release_date: 2000+i)
  end
    Movie.all.update(title: "A Movie") #used update method provided by AR::Base on the all array.
end
```



#### Destroy

```ruby
def can_destroy_a_single_item
  Movie.create(title: "That One Where the Guy Kicks Another Guy Once")
  Movie.destroy(1) #Used the destroy method provided by AR::Base to destroy 1 instance
end

def can_destroy_all_items_at_once
  10.times do |i|
    Movie.create(title: "Movie_#{i}")
  end
  Movie.destroy_all 
end
```

