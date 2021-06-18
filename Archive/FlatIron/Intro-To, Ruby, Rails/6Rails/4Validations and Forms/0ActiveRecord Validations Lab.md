

# lab

```ruby
class Author < ActiveRecord::Base
  validates :name, presence: true
  validates :name, uniqueness: true
  validates :phone_number, length: {is: 10}
end
```

```ruby
class Post < ActiveRecord::Base
  include ActiveModel::Validations
  validates :title, presence: true
  validates :content, length: {minimum: 250}
  validates :summary, length: {maximum: 250}
  validates :category, inclusion: {in: %w(Fiction Non-Fiction)}
  validates_with IsBait
end

```

```ruby
class IsBait < ActiveModel::Validator
  def validate(record)
    if record.title
      reqs = ["Won't Believe", "Secret", "Top[number]", "Guess"]
      if reqs.detect {|el| record.title.include?(el) }.nil?
        record.errors[:title] << "Must contain clickbait"
      end
    end
  end
end
```

