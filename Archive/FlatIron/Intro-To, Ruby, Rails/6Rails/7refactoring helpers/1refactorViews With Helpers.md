## Instructions

The base models, controllers, views and other files have been provided. There are tests for the lab in the `spec` directory. You can run tests with the `rspec` command.

1. Write the code for `#artist_name` and `#artist_name=` so that an `Artist` can be retrieved from, and associated with, a `Song` instance

2. Write a helper method

    

   ```
   #display_artist
   ```

    

   in the appropriate

    

   ```
   app/helpers
   ```

    

   file to be called on in our views. The method's return value should take into consideration whether an artist is already associated with a song:

   - If an artist is already associated with the song, return a link to the artist's `show` page
   - If an artist is not associated with the song (a.k.a. 'else'), return a link to the song's `edit` page, with a link text of "Add Artist"

3. Use the helper to display the artist on the `songs#show` and `songs#index` pages

   # lab

model

```ruby
class Song < ActiveRecord::Base
  belongs_to :artist

  def artist_name
    self.artist ? self.artist.name : nil
      # ? is like useing an if statment
  end

  def artist_name=(name)
    self.artist = Artist.find_or_create_by(name: name)
  end
end
```

helpers

```ruby
module SongsHelper
  def display_artist(song)
    if song.artist_name.present?
        #we can't use song.artist.name because, if the value is nill then we are returned a no method error. However it does work is just won't work for our test suite
        #the reason it works is because song.artist.name has no value which will return false.
        #but the test suite still returns no method
      link_to song.artist.name, artist_path(song.artist)
    else
      link_to 'Add Artist', edit_song_path(song)
    end
  end

end
```

views

```erb
<ul>
<% @songs.each do |song| %>
  <li><%= link_to song.title, song_path(song) %> - <%= display_artist(song) %> </li>
<% end %>
</ul>
<br>
<%= link_to "Add New Song", new_song_path %>
```

```erb
<h1><%= link_to @song.title, song_path(@song) %> - <%= display_artist(@song) %></h1>

<%= link_to "All Songs", songs_path %>

```

