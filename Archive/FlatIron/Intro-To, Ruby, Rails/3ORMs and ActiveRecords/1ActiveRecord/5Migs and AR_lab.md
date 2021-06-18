

```ruby
#each class has its own file under app/models
class CostumeStore < ActiveRecord::Base
end

class Costume < ActiveRecord::Base
end

class HauntedHouse < ActiveRecord::Base
end
```

```ruby
class CreateCostumes < ActiveRecord::Migration[5.1]

def change
  create_table :costumes do |t|
    t.string :name
    t.integer :price
    t.integer :size
    t.string :image_url
    t.timestamps #this will make two columns :created_at and :updated_at
  end
end

end
```

```ruby
class CreateCostumeStores < ActiveRecord::Migration[5.1]
  
  def change
  create_table :costume_stores do |t|
  t.string :name
  t.string :location
  t.integer :costume_inventory
  t.integer :num_of_employees
  t.boolean :still_in_business
  t.datetime :opening_time
  t.datetime :closing_time
    end
  end

end
```

```ruby
class CreateHauntedHouses < ActiveRecord::Migration[5.1]

def change
  create_table :haunted_houses do |t|
  t.string :name
  t.string :location
  t.string :theme
  t.integer :price
  t.boolean :family_friendly
  t.datetime :opening_date
  t.datetime :closing_date
  t.string :description
  end
end

end
```

| Data Type | Examples                                              |
| --------- | ----------------------------------------------------- |
| boolean   | true, false                                           |
| integer   | 2, -13, 485                                           |
| string    | "Halloween", "Boo!", strings between 1-255 characters |
| datetime  | DateTime.now, DateTime.new(2014,10,31)                |
| float     | 2.234, 32.2124, -6.342                                |
| text      | strings between 1 and 2 ^ 32 - 1 characters           |