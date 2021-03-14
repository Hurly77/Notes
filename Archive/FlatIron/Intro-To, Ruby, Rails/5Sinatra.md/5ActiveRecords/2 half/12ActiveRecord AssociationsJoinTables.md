### Why Join Tables

By now, you're familiar with the basic ActiveRecord associations: `has_many` and `belongs_to`. But how would you handle an **e-commerce site**? **user** can have **many items**, and an **item** can **belong to many users**.**

This is where **Join Tables** come in to play, with the `has_many :through` association which is used for object associations where more than one object can own many objects of the same class.

The Table **e-commerce** would contain a `user_id` and `item_id `**Each row** in this table would **contain** a **user's ID** and an **item's ID.** 

we call this join table `user_items`

#### Migrations

```ruby
 
class CreateUsers < ActiveRecord::Migration
  def change
    create_table :users do |t|
      t.string :name
      t.timestamps null: false
    end
  end
end
 
class CreateItems < ActiveRecord::Migration
  def change
    create_table :items do |t|
      t.string :name
      t.integer :price
      t.timestamps null: false
    end
  end
end
 
class CreateUserItems < ActiveRecord::Migration
  def change 
    create_table :user_items do |t|
      t.integer :user_id
      t.integer :item_id
      t.timestamps null: false
    end
  end
end
 
```

### Models

```ruby
class User < ActiveRecord::Base
  has_many :user_items
  has_many :items, through: :user_items
end
 
class Item < ActiveRecord::Base
  has_many :user_items
  has_many :users, through: :user_items
end
 
class UserItem < ActiveRecord::Base 
  belongs_to :user
  belongs_to :item
end
```

Our `User` class has two associations. The first is a `has_many` association with the join table, and the second is the `has_many through:` to connect users and items.

We set up a similar pattern of relationships for the `Item` class, this time to connect items to users.

And finally, we have the model for the join table, `UserItem`. In this model, we set up a `belongs_to` for both users and items.