# Foreign Keys

**Foreign keys** - are columns that refer to the primary key of another table. Conventionally, foreign keys in Active Record are **comprised of** the **name** of the **model** you're referencing, **and** `_id`

Like any other column, foreign keys are accessible through instance methods of the same name

```ruby
class AddAuthorIdToPosts < ActiveRecord::Migration
  def change
    change_table :posts do |t|
      t.integer :author_id
    end
  end
end

#Would mean you could find a post's author with the following Active Record query:

Author.find(@post.author_id)

#which is equivalent to the SQL:
SELECT * FROM authors WHERE id = #{@post.author_id}

#and you could lookup an author's posts like this:
Post.where("author_id = ?", @author.id)

#Which is equivalent to the SQL:
SELECT * FROM posts WHERE author_id = #{@author.id}
```

# Many-to-One Relationships

The most common relationship is **many-to-one**, and it is declared in Active Record with `belongs_to` and `has_many`.

## `belongs_to`

Each `Post` is associated with **one** `Author`.

```ruby
class Post < ActiveRecord::Base
  belongs_to :author
end
```

We now have access to some new instance methods, like `author`. This will return the actual `Author` object that is attached to that `@post`.

```ruby
@post.author_id = 5
@post.author #=> #<Author id=5>
```

## `has_many`

In the opposite direction, each `Author` might be associated with zero, one, or many `Post` objects. We haven't changed the schema of the `authors` table at all; Active Record is just going to use `posts.author_id` to do all of the lookups.

```ruby
class Author < ActiveRecord::Base
  has_many :posts
end
```

Now we can look up an author's posts just as easily:

```ruby
@author.posts #=> [#<Post id=3>, #<Post id=8>]
```

Remember, Active Record uses its [Inflector](http://api.rubyonrails.org/classes/ActiveSupport/Inflector.html) to switch between the singular and plural forms of your models.

| Name         | Data        |
| ------------ | ----------- |
| Model        | `Author`    |
| Table        | `authors`   |
| Foreign Key  | `author_id` |
| `belongs_to` | `:author`   |
| `has_many`   | `:authors`  |

**the symbol** you pass **determines** the name of **the instance** method that will be defined. So `belongs_to :author` will give you `@post.author`, and `has_many :posts` will give you `@author.posts`.

# Convenience Builders

## Building a new item in a collection

```ruby
#If you want to add a new post for an author, you might start this way:
new_post = Post.new(author_id: @author.id, title: "Web Development for Cats")
new_post.save

#But the association macros save the day again, allowing this instead:
new_post = @author.posts.build(title: "Web Development for Cats")
new_post.save #This will return a new Post object with the author_id already set for you! 
```

`build` works just like `new` so  **isn't** quite saved to the database

## Setting a singular association

```ruby
#The verbose way of creating this association would be like so:
@post.author = Author.new(name: "Leeroy Jenkins") 
#in the previous section, @author.posts always exists

#Here, 
@post.author #is nil until the author is defined

#Active Record can't give us
@post.author.build
```

Instead, it prepends the attribute with `build_` or `create_`. The `create_` option **will persist** to the database for you.

```ruby
new_author = @post.build_author(name: "Leeroy Jenkins")
#Remember, if you used the build_ option, you'll need to persist your new author with #save.
```

These methods are also documented in the [Rails Associations Guide](http://guides.rubyonrails.org/association_basics.html).

## Collection Convenience

**If** you **add** an **existing** **object** to a collection association, Active Record **will** conveniently take care of **set**ting the **foreign key for you:**

```ruby
@author = Author.find_by(name: "Leeroy Jenkins")
@author.posts
#=> []
@post = Post.new(title: "Web Development for Cats")
@post.author
#=> nil
@author.posts << @post
@post.author
#=> #<Author @name="Leeroy Jenkins">
```

# One-to-One Relationships

it's usually a safe bet to put `belongs_to` on whichever model has the foreign key column in its database table.

# Many-to-Many Relationships and Join Tables

Because there is no "owner" model in this relationship, there's also no right place to put the foreign key column.

Enter the join table:

| `tag_id` | `post_id` |
| -------- | --------- |
| 1        | 1         |
| 2        | 1         |
| 1        | 5         |

This join table depicts two tags (1 and 2) and two posts (1 and 5). Post 1 has both tags, while Post 5 has only one.

Active Record has a migration method for doing exactly this.

```ruby
create_join_table :posts, :tags
```

This will create a table called `posts_tags`.

# `has_many :through`

Ideally, we'd like to be able to call a `@my_post.tags` method, right? That's where `has_many :through` comes in.

First of all, let's add the `has_many :posts_tags` line to our `Post` and `Tag` models:

```ruby
class Post
  has_many :posts_tags
end
 
class PostsTag
  belongs_to :post
  belongs_to :tag
end
 
class Tag
  has_many :posts_tags
end

```

 we need one more `has_many` relationship to complete the link between tags and posts: `has_many :through`.  we need one more `has_many` relationship to complete the link between tags and posts: `has_many :through`

```ruby
class Post
  has_many :posts_tags
  has_many :tags, through: :posts_tags
end
 
class PostsTag
  belongs_to :post
  belongs_to :tag
end
 
class Tag
  has_many :posts_tags
  has_many :posts, through: :posts_tags
end
```

