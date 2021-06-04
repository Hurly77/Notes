Your goal in this lab is to create a thorough CRUD interface for one model, the `Song`.

## Songs

Songs have the following attributes and limitations:

- `title`, a `string`

   Must not be blank

  - Cannot be repeated by the same artist in the same year

- `released`, a `boolean` describing whether the song was ever officially released

  - Must be `true` or `false`

- `release_year`, an `integer`

  - Optional if `released` is `false`
  - Must not be blank if `released` is `true`
  - Must be less than or equal to the current year

- `artist_name`, a `string`

  - Must not be blank

- `genre`, a `string`

## Requirements

Use the `resource` generator, **not** the `scaffold` generator

1. Define a model with validations for `Song`.
2. Define all RESTful routes for songs.
3. Render the list of songs in an HTML table.
4. Build views that connect to each other using route helpers.
5. Use `form_for` to build forms with pre-fill and error list features. (*Hint: Try using a partial to cut down on copy/pasting!*)
6. Allow deleting songs with a link, using `link_to`. Check out the [official documentation](http://api.rubyonrails.org/classes/ActionView/Helpers/UrlHelper.html#method-i-link_to) for additional info including setting the `method:` to `:delete`.
7. Use strong parameters in your POST/PATCH controller actions.
8. Set the root route to the song index.

# lab

First thing I did was run`rails g Song title:string released:boolean release_year:integer artist_name:string genre:string --no-test-framework`

### validations

```ruby
class Song < ActiveRecord::Base
  validates :title, presence: true
    #checks field presence
  validates :title, uniqueness: {scope: [:release_year, :artist_name]}
    #scope limits the uniqeuness check be makink sure both atributes
    #release_year and artist_name are do not exit anywhere else in the 		#database
  validates :released, inclusion: {in: [true, false]}
    #validation can be both true and false
  validates :release_year, presence: true, if: :released
    # check the field is not blank, will return true if :released is also true or false
  validate :valid_year
    #my method
  validates :artist_name, presence: true

  def valid_year
    if release_year.present? && release_year > Date.today.year
      #returns true if the release year is present in the field and the year is greater the current year
      errors.add(:release_year, "release year can't be in the future")
        #adds the text when release_year fails validation
    end
  end
end
```

### Controller

```ruby
class SongsController < ApplicationController
  def index
    @songs = Song.all
  end

  def show
    @song = Song.find(params[:id])
  end

  def new
    @song = Song.new
  end

  def create
    @song = Song.new(song_params)

    if @song.valid?
      @song.save
      redirect_to song_path(@song)
    else
    
      render :new
    end
  end

  def edit
    @song = Song.find(params[:id])
  end

  def update
    @song = Song.find(params[:id])
    @song.update(song_params)

    if @song.valid?

      redirect_to song_path(@song)
    else

      render :edit
    end
  end

  def destroy
    @song = Song.find(params[:id]).destroy

    redirect_to songs_url
  end

  private

  def song_params
    params.require(:song).permit(:title, :released, :release_year, :artist_name, :genre)
  end
end
```

### views

##### new

```erb
<%= form_for(@song) do |f| %>
<!--form for SONG-OBJ  -->
    <% if @song.errors.any? %>
	<!--if there are any errors-->
      <div id="error_explanation">
        <h2>There are <%= @song.errors.size%> errors: </h2>
          <!-- number of errors-->
          <ul>
            <% @song.errors.full_messages.each do |msg| %>
              <!--displays the full_messages of each error-->
              <li><%= msg %></li>
            <% end %>
          </ul>
      </div>
      <% end %>

<div class="field">
    <!--Capybara requires there to be a class, I believe-->
    <%= f.label :title %> <br>
    <%= f.text_field :title %> <br>
  </div>
  <div class="field">
    <%= f.label :released %> <br>
    <%= f.check_box :released %> <br>
  </div>
  <div class="field">
    <%= f.label :release_year %> <br>
    <%= f.text_field :release_year %> <br>
  </div>
  <div class="field">
    <%= f.label :artist_name%> <br>
    <%= f.text_field :artist_name%> <br>
  </div>
  <div class="field">
    <%= f.label :genre%> <br>
    <%= f.text_field :genre%> <br>
  </div>
    <%= f.submit %> <br>
  <% end %>
```

##### edit

```erb
<%= form_for(@song) do |f| %>
    <% if @song.errors.any? %>
      <div id="error_explanation">
        <h2>There are <%= @song.errors.size%> errors: </h2>
          <ul>
            <% @song.errors.full_messages.each do |msg| %>
              <li><%= msg %></li>
            <% end %>
          </ul>
      </div>
      <% end %>


  <div class="field"> 
    <%= f.label :title %> <br>
    <%= f.text_field :title %> <br>
  </div>
  <div class="field">
    <%= f.label :released %> <br>
    <%= f.check_box :released %> <br>
  </div>
  <div class="field">
    <%= f.label :release_year %> <br>
    <%= f.text_field :release_year %> <br>
  </div>
  <div class="field">
    <%= f.label :artist_name%> <br>
    <%= f.text_field :artist_name%> <br>
  </div>
  <div class="field">
    <%= f.label :genre%> <br>
    <%= f.text_field :genre%> <br>
  </div>
    <%= f.submit %> <br>
  <% end %>
```

##### index

```erb
<h1>Songs</h1>
<br>

<table>
    <thread>
        <tr>
            <th>Title</th>
            <th>Year</th>
            <th>Genre</th>
            <th>Artist name</th>
            <th></th>
        </tr>
    </thread>

    <tbody>
        <% @songs.each do |song| %>
            <tr>
                <td><%= song.title %></td>
                <td><%= song.release_year %></td>
                <td><%= song.genre %></td>
                <td><%= song.artist_name %></td>
                <td>
                    <%= link_to 'View', song %> |
                    <%= link_to 'Edit', edit_song_path(song) %> |
                    <%= link_to 'Delete', song, method: :delete, data: {confirm: 'Are you sure you want to delete this?'} %>
                </td>
            </tr>
        <% end %>
    </tbody>
</table>
<!--I guess it wanted me to build out a table Not sure when it told me to do that but I did-->
```

