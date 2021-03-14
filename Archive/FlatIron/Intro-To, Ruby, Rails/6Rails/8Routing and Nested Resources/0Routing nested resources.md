1. Create nested resource routes to show all songs for an artist (`/artists/1/songs`) and individual songs for that artist (`/artists/1/songs/1`). Restrict the nested songs routes to `index` and `show` actions only.
2. Update the artists `index` view to use the new nested resource route URL helper to link to the index of all songs by that artist.
3. Update the artists `show` view to list each song for that artist, and use the new nested resource route helper to link each song to its corresponding `show` page.
4. Update the `songs_controller` to allow the `songs#index` and `songs#show` actions to handle a valid song for the artist.
5. In the `songs#index` action, if the artist can't be found, redirect to the `index` of artists, and set a `flash[:alert]` of "Artist not found."
6. In the `songs#show` action, if the song can't be found for a given artist, redirect to the `index` of the artist's songs and set a `flash[:alert]` of "Song not found."

# lab

### controller

```ruby
class SongsController < ApplicationController
  def index
    if params[:artist_id]
     if Artist.find_by_id(params[:artist_id])
      @songs = Artist.find_by_id(params[:artist_id]).songs
    else
      redirect_to artists_path
      flash[:alert] = "Artist not found"
    end
    else 
      @songs = Song.all
    end
  end

  def show
    if @song = Song.find_by_id(params[:id])
    else
      redirect_to artist_songs_path
      flash[:alert] = "Song not found"
    end
  end
end
```

### views

```erb
<!--app/views/artists/show-->
<h1>Artists</h1>
<ul>
  <% @artists.each do |artist| %>
    <li><%= link_to artist.name, artist_songs_path(artist) %></li>
    <!--here we link to the paticluar artist, with the songs proxy and pass in (artist) because AR will know to look for the ID-->
  <% end %>
</ul>


<!--app/views/artists/show-->
<h1><%= @artist.name %></h1>
<ul>
  <% @artist.songs.each do |song| %>
    <li><%= link_to song.title, artist_song_path(@artist, song) %></li>
  <% end %>
</ul>

```

