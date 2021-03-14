## Instructions

1. Using nested resources, set up routes and controller actions to create new `song` records through an `artist`. **Hint:** Don't forget to update the strong parameters.
2. Set up routes and controller actions to support editing a `song` as a nested resource of an `artist`.
3. Create a helper to display a drop-down list of artists if someone edits a song directly via `/songs/:id/edit` and to only display the artist's name if they are editing through nested routing. Name the helper method `artist_select`. **Hint:** You'll need to set a variable in the controller action to pass to the helper method as an argument along with a `song` instance.
4. Validate that new songs created for an artist via nested routing are created for valid artists, and redirect to `/artists` if not.
5. Validate that songs being edited via nested routing have a valid artist. Redirect to `/artists` if not.
6. Validate that songs being edited via nested routing are in the artist's `songs` collection. Redirect to `/artists/:artist_id/songs` if not.
7. Make sure all tests pass!

# Lab

routes

```ruby
Rails.application.routes.draw do
  resources :artists do
    resources :songs, only: [:index, :show, :new, :edit]
  end
  resources :songs
end

```

controller

```ruby
class SongsController < ApplicationController
  def index
    if params[:artist_id]
      @artist = Artist.find_by(id: params[:artist_id])
      if @artist.nil?
        redirect_to artists_path, alert: "Artist not found"
      else
        @songs = @artist.songs
      end
    else
      @songs = Song.all
    end
  end

  def show
    if params[:artist_id]
      @artist = Artist.find_by(id: params[:artist_id])
      @song = @artist.songs.find_by(id: params[:id])
      if @song.nil?
        redirect_to artist_songs_path(@artist), alert: "Song not found"
      end
    else
      @song = Song.find(params[:id])
    end
  end

  def new
     if params[:artist_id] && !Artist.exists?(params[:artist_id])
        redirect_to artists_path, alert: "Artist not found"
     else
    @song = Song.new(artist_id: params[:artist_id])
     end
  end

  def create
    @song = Song.new(song_params)

    if @song.save
      redirect_to @song
    else
      render :new
    end
  end

  def edit
    if params[:artist_id]
      artist = Artist.find_by(id: params[:artist_id])
      if artist.nil?
        redirect_to artists_path, alert: "Artist not found"
      else
        @song = artist.songs.find_by(id: params[:id])
        redirect_to artist_songs_path(artist), alert: "Artist not found" if @song.nil?
      end
      else
    @song = Song.find(params[:id])
    end
  end

  def update
    @song = Song.find(params[:id])

    @song.update(song_params)

    if @song.save
      redirect_to @song
    else
      render :edit
    end
  end

  def destroy
    @song = Song.find(params[:id])
    @song.destroy
    flash[:notice] = "Song deleted."
    redirect_to songs_path
  end

  private

  def song_params
    params.require(:song).permit(:title, :artist_name, :artist_id)
  end
end
```

views

```erb
<%= form_for @song  do |f| %>
<%= artist_id_field(@song) %> <br>
<br>
  <%= f.label :title %> <br>
  <%= f.hidden_field :artist_id %>
  <%= f.text_field :title %><br>
  <%= f.label :artist_name %> <br>
  <%= f.text_field :artist_name %> <br>
  <br/>

  <%= f.submit %>
<% end %>

```

helpers

```ruby
module SongsHelper
  def artist_id_field(song)
    if song.artist.nil?
      select_tag "song[artist_id]", options_from_collection_for_select(Artist.all, :id, :name)
    else
      hidden_field_tag "song[artist_id]", song.artist_id
    end
  end
end
```

