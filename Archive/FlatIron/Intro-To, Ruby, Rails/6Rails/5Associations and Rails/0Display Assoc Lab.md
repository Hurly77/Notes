# lab

### models

```ruby
class Artist < ActiveRecord::Base
  has_many :songs

  def song_count
    self.songs.count
  end
end
```

```ruby
class Song < ActiveRecord::Base
  belongs_to :artist

  def artist_name
    self.artist.name
  end
end
```

#### views/artists

```erb
<h1>Editing <%= @artist.name %></h1>

<%= form_for @artist  do |f| %>
  <%= f.label :name %>
  <%= f.text_field :name %>
  <br />

  <%= f.submit %>
<% end %>

```

```erb
<h1>Artists: </h1>
<% @artists.each do |artist| %>
<h3>Songs: <%= pluralize(artist.song_count, 'Song')%></h3>
<li><%= link_to artist.name, artist_path(artist) %></li>
<% end %>
```

```erb
<h1>Create an Artist</h1>

<%= form_for @artist  do |f| %>
  <%= f.label :name %>
  <%= f.text_field :name %>
  <br />

  <%= f.submit %>
<% end %>

```

```erb
<h1><%= @artist.name %></h1>
<h3>Songs: <%= pluralize(@artist.song_count, 'Song')%></h3>
<ul>
<% @artist.songs.each do |song| %>
<li><%= link_to song.title, song_path(song) %></li>
<% end %>
</ul>
```

#### views/songs

```erb
<h1>Editing <%= @song.title %></h1>

<%= form_for @song  do |f| %>
  <%= f.label :title %>
  <%= f.text_field :title %>
  <br />

  <%= f.submit %>
<% end %>
```

```erb
<h1>Songs</h1>

<% @songs.each do |song| %>
<li><%= link_to "#{song.artist_name} - #{song.title}", song_path(song) %></li>
<% end %>
```

```erb
<h1>Create a Song</h1>

<%= form_for @song  do |f| %>
  <%= f.label :title %>
  <%= f.text_field :title %>
  <br />

  <%= fields_for :artist do |artist_f| %>
    <%= artist_f.label :artist %>
    <%= artist_f.text_field :name %>
    <br />
  <% end %>

  <%= f.submit %>
<% end %>
```

```erb
<h1>Create a Song</h1>

<%= form_for @song  do |f| %>
  <%= f.label :title %>
  <%= f.text_field :title %>
  <br />

  <%= fields_for :artist do |artist_f| %>
    <%= artist_f.label :artist %>
    <%= artist_f.text_field :name %>
    <br />
  <% end %>

  <%= f.submit %>
<% end %>
```

### migrations

#### songs

```ruby
class CreateSongs < ActiveRecord::Migration
  def change
    create_table :songs do |t|
      t.string :title
      t.belongs_to :artist

      t.timestamps null: false
    end
  end
end
```

