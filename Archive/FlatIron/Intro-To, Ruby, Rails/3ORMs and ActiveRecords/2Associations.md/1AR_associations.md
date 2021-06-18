## How do we use AR Associations?

**Active Record** makes it easy to **implement** the following **relationships** between models:

- belongs_to
- has_one
- has_many
- has_many :through
- has_one :through
- has_and_belongs_to_many

### Our Migrations

We need to **create** Three **models** with **one** having relationship to **both** models

Lets say we have these models

- The Song Model - song will **belong** to an **artist** *and* belong to a **genre**
- The Artist Model - An **artist** will **have many songs** and it will have many **genres** ***through* songs**
- The Genre Model -  **genre** will have **many** **songs** and it will have many **artists** **through songs**

### Building our Associations using AR Macros

### What is a macro?

A **macro** is a **method** that **writes** **code** for us (think metaprogramming).

The following AR macros (or methods)

- [`has_many`](http://guides.rubyonrails.org/association_basics.html#the-has-many-association)
- [`has_many through`](http://guides.rubyonrails.org/association_basics.html#the-has-many-through-association)
- [`belongs_to`](http://guides.rubyonrails.org/association_basics.html#the-belongs-to-association)

### syntax

```ruby
#In app/models/song.rb
class Song < ActiveRecord::Base
  belongs_to :artist
  belongs_to :genre
end
#In app/models/artist.rb
class Song < ActiveRecord::Base
  belongs_to :artist
  belongs_to :genre
end
#In app/models/genre.rb
class Song < ActiveRecord::Base
  belongs_to :artist
  belongs_to :genre
end
```

Let's make a few new songs and artist in pry:

```ruby
[1]pry(main)> hello = Song.new(name: "Hello")
[2]pry(main)> hotline_bling = Song.new(name: "Hotline Bling")
[3] pry(main)> adele = Artist.new(name: "Adele")
[4] pry(main)> drake = Artist.new(name: "Drake")

```

The macros we implemented in our classes allow us to associate a song object directly to an artist object:

```ruby
[5] pry(main)> hello.artist = adele

### | Now, we can ask hello who its artist is | ###
[6] pry(main)> hello.artist
=> #<Artist:0x007fc75b8d9490 id: nil, name: "Adele">

### We can ask THE |hello| METHOD for THE name OF the artist###
[7] pry(main)> hello.artist.name
=> "Adele"

```

We can assign and artist to a song but we cant do the reverse

```ruby
[7] pry(main)> adele.songs
=> []

#we'll need to tell the adele Artist instance wich songs it has
[7] pry(main)> adele.songs.push(hello)
=> [#<Song:0x007fc75a8de3d8 id: nil, name: "Hello", artist_id: nil, genre_id: nil>]
```

However the **instances** we created are **temporary**, we can **persist** them with **save** from **AR**.

```ruby
[8] pry(main)> adele.save
=> true
[9] pry(main)> adele
=> #<Artist:0x007fc75b8d9490 id: 1, name: "Adele">

#  Notice that adele now has an id. What about hello?
[10] pry(main)> hello
=> #<Song:0x007fc75a8de3d8 id: 1, name: "Hello", artist_id: nil, genre_id: nil>
```

We did'nt need to mention **hello** in our **save** because **hello** was **persisted through adele**

## Adding Additional Associations

```ruby
[8] pry(main)> someone_like_you = Song.new(name: "Someone Like You")
[8] pry(main)> someone_like_you.artist = adele
__________________________________________________________
#We've only updated the song, adele is not aware of this song

[8] pry(main)> someone_like_you.artist
=> #<Artist:0x007fc75b8d9490 id: 1, name: "Adele">
[9] pry(main)> adele.songs
=> [#<Song:0x007fc75b9f3a38 id: 1, name: "Hello", artist_id: 1, genre_id: nil>]
___________________________________________________________
#Even if we save the song, adele will not be updated.

[8] pry(main)> someone_like_you.save
=> true
[8] pry(main)> someone_like_you
=> #<Song:0x007fc75b5cabc8 id: 2, name: "Someone Like You", artist_id: 1, genre_id: nil>
[9] pry(main)> adele.songs
=> [#<Song:0x007fc75b9f3a38 id: 1, name: "Hello", artist_id: 1, genre_id: nil>]
_____________________________________________________________
#Creating one more song:

[8] pry(main)> set_fire_to_the_rain = Song.new(name: "Set Fire to the Rain")
=> #<Song:0x007fc75b5cabc8 id: nil, name: "Set Fire to the Rain", artist_id: nil, genre_id: nil>
______________________________________________________________
#Then add the song to adele:
    
[9] pry(main)> adele.songs.push(set_fire_to_the_rain)
=> [#<Song:0x007fc75b9f3a38 id: 1, name: "Hello", artist_id: 1, genre_id: nil>, #<Song:0x00007feac2be4f38 id: 3, name: "Set Fire to the Rain", artist_id: 1, genre_id: nil>]
```

we did not **explicitly** save `set_fire_to_the_rain`, but just by **pushing** the **instance** into `adele.songs`, **Active Record** has gone ahead and **saved** the **instance**. Not only that, notice that the **song instance** *also has an* **artist id!**_

```ruby
[8] pry(main)> set_fire_to_the_rain.artist
=> #<Artist:0x007fc75b8d9490 id: 1, name: "Adele">
```

when dealing with **associations** Active Record it will behave differently **depending** on which side of a **relationship** between two models you are **updating**.

**Remember:** In a `has_many`/`belongs_to` relationship, we can think of the model that `has_many` as the parent in the relationship. The model that `belongs_to`, then, is the child. If you tell the child that it belongs to the parent, *the parent won't know about that relationship*. If you tell the parent that a certain child object has been added to its collection, *both the parent and the child will know about the association*.

create another **new song** and **add** it to `adele`'s songs collection:

```ruby
[10] pry(main)> rolling_in_the_deep = Song.new(name: "Rolling in the Deep")

[11] pry(main)> adele.songs << rolling_in_the_deep
=> [ #<Song:0x007fc75bb4d1e0 id: 4, name: "Rolling in the Deep", artist_id: 1, genre_id: nil>]
[12] pry(main)> rolling_in_the_deep.artist
=> #<Artist:0x007fc75b8d9490 id: 4, name: "Adele">
```

```ruby
# this is the same as running Genre.new, then Genre.save.
[13] pry(main)> pop = Genre.create(name: "pop")
_________________________________________________________
[14] pry(main)> pop.songs << rolling_in_the_deep
=> [#<Song:0x007fc75bb4d1e0 id: 4, name: "Rolling in the Deep", artist_id: 1, genre_id: 1>]
[15] pry(main)> pop.songs
=> [#<Song:0x007fc75bb4d1e0 id: 4, name: "Rolling in the Deep", artist_id: 1, genre_id: 1>]
[16] pry(main)> rolling_in_the_deep.genre
=> #<Genre:0x007fa34338d270 id: 1, name: "pop">
```

It's working! But even cooler is that we've established has many *through* relationships

```ruby
[16] pry(main)> rolling_in_the_deep.artist
=> #<Artist:0x007fc75b8d9490 id: 4, name: "Adele">
[17] pry(main)> pop.artists
=> [#<Artist:0x007fc75b8d9490 id: 1, name: "Adele">]
[28] pry(main)> adele.genres
=> [#<Genre:0x007fa34338d270 id: 1, name: "pop">]
```

## Conclusion

So, order and direction of operations does matter when establishing associations between models - it is typically better to update the `has_many` side of a relationship to get the full benefit of Active Record's power. Still, as we can see, with just migrations and Active Record macros, we can start to build and persist associations between things!