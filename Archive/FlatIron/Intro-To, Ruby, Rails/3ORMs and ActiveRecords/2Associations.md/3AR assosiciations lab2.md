```ruby
require 'pry'
class Actor < ActiveRecord::Base
  has_many :characters
  has_many :shows, through: :characters  def full_name
    "#{self.first_name} #{self.last_name}"
  end  def list_roles
    self.characters.map do |chr| "#{chr.name} - #{chr.show.name}"
    end
  end
end
```

```ruby
class Character < ActiveRecord::Base
belongs_to :actor
belongs_to :showdef say_that_thing_you_say
  "#{self.name} always says: #{self.catchphrase}"
endend
```

```ruby
class Show < ActiveRecord::Base
  has_many :characters
  belongs_to :network
  has_many :actors, through: :characters  def actors_list
    self.actors.map {|obj| obj.full_name}
  end
end
```

```ruby
class CreateActors < ActiveRecord::Migration[5.1]
  def change
    create_table :actors do |t|
      t.string :first_name
      t.string :last_name
    end
  end
end
```

```ruby
class CreateCharacters < ActiveRecord::Migration[5.1]
  def change
    create_table :characters do |t|
      t.string :name
      t.integer :actor_id
      t.integer :show_id
    end
  end
end
```

```ruby
class AddColumnCatchphraseToCharacter < ActiveRecord::Migration[5.1]
  def change
    add_column :characters, :catchphrase, :string
  end
end
```

```ruby
class AddColumnsToShows < ActiveRecord::Migration[5.1]
  def change
    add_column :shows, :day, :string
    add_column :shows, :genre, :string
    add_column :shows, :season, :string
  end
end
```

```ruby
rake db:create_migration NAME=create_movies
```

