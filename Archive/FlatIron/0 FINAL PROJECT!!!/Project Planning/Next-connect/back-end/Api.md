**Relationships**

User, has_many: posts => ATTRIBUTES:

- :name - string
- email - string
- password - string
- friends - array

Post, belongs_to :user => ATTRIBUTES:

- user_id
- image
- text

