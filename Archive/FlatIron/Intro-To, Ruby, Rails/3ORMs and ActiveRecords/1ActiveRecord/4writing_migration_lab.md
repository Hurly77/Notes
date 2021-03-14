**Writing Our Own Migrations**

create_table with migrations

```ruby
class CreateStudents < ActiveRecord::Migration[5.1]
  def change
    create_table :students do |t|
      t.string :name
        #create_table(table.data_type :column_name)
    end
  end
end
```

Adding colums

```ruby
class AddGradeAndBirthdateToStudents < ActiveRecord::Migration[5.1]
  def change
    add_column :students, :grade, :integer
    add_column :students, :birthdate, :string
  end
end
#add_column(:table_name, :the_column, :data_type)
```

changing columns dataType

```ruby
class ChangeDatatypeForBirthdate < ActiveRecord::Migration[5.1]
  def change
      #used change_column
    change_column :students, :birthdate, :datetime
  end
end
#change_column(:table_name, :the_column, :data_type)
```

