### Primary Keys

owners can **have many** cats and cats **belong to** an owner. Let's assume we have two tables in our database: `cats` and `owners`

#### Review: Creating a table with ActiveRecord

rake db:create_migration NAME="create_cats"

#### Review: Primary Keys

A **primary** **key** uniquely identifies each record in a table. It **must** be **unique** and **cannot** have **NULL values.** **ActiveRecord will create** the primary key **for us**

**WE Have**

- **three** instances of the **Cat** class:

- And **two** instances of our **Owner** class:

Our `cats` table looks like this:

| id   | name    | age  | breed         |
| ---- | ------- | ---- | ------------- |
| 1    | Maru    | 3    | Scottish Fold |
| 2    | Hannah  | 2    | Tabby         |
| 3    | Patches | 2    | Calico        |

Our `owners` table looks like this:

| id   | name   |
| ---- | ------ |
| 1    | Sophie |
| 2    | Ann    |

**WE NEED TO** Tell our tables how to relate to each other

#### Using Foreign Keys

A foreign key points to a **primary key** in another table. In ActiveRecord we will use the `tablename_id` *convention*.

The foreign key always sits on the table of the object that belongs to.

```ruby
#In this case, because cats belong to an owner, the owner_id becomes a column in the cats table.
class AddColumnToCats < ActiveRecord::Migration
  def change
    add_column :cats, :owner_id, :integer
  end
end
```

Our `cats` table should look like this:

| id   | name    | age  | breed         | owner_id |
| ---- | ------- | ---- | ------------- | -------- |
| 1    | Maru    | 3    | Scottish Fold | 1        |
| 2    | Hannah  | 2    | Tabby         | 2        |
| 3    | Patches | 2    | Calico        | 1        |

#### `belongs_to` and `has_many`

```ruby
#cat belongs to an owner
class Cat
  belongs_to :owner
end
```

```ruby
and an owner can have many cats.
class Owner
  has_many :cats
end
```

#### Important note:

> Whenever we use a `has_many` we also have to use the `belongs_to` (and vice-versa) in the other model. ***Keep in mind:\*** The model with the `belongs_to` association also has the foreign key.

#### Creating objects



```ruby
sophie = Owner.create(name: "Sophie")
#.create method to instantiate and save the owner to our database.
maru = Cat.new(name: "Maru", age: 3, breed: "Scottish Fold")
#To instantiate the cat object we used the .new method
maru.owner = sophie
maru.save
```

The `has_many`/`belongs_to` relationship is the most used association, but there are others as well. You can read more about ActiveRecord Associations [here](http://guides.rubyonrails.org/association_basics.html).