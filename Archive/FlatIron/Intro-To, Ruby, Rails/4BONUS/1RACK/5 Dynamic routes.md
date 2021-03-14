## Setting Up Dynamic Routes

**playlister app** `Song` object :

```ruby
#song.rb
 
class Song
 
attr_accessor :title, :artist
 
end

#Pretty simple class. Now we have our web app.
class Application
 
  @@songs = [Song.new("Sorry", "Justin Bieber"),
            Song.new("Hello","Adele")]
 
  def call(env)
    resp = Rack::Response.new
    req = Rack::Request.new(env)
 
    @@songs.each do |song|
      resp.write "#{song.title}\n"
    end
 
    resp.finish
  end
end

```

 Remember the path is given to us as a `string`. We could therefore write something like this:

```ruby
class Application
 
  @@songs = [Song.new("Sorry", "Justin Bieber"),
            Song.new("Hello","Adele")]
 
  def call(env)
    resp = Rack::Response.new
    req = Rack::Request.new(env)
 
    if req.path=="/songs/Sorry"
      resp.write @@songs[0].artist
    elsif req.path == "/songs/Hello"
      resp.write @@songs[1].artist
    end
 
    resp.finish
  end
end
```

Thankfully, because paths are `strings`, we can do a regex match against the path. Then we just grab the content after the `/song/` to figure out which `Song` our user would like.

```ruby
class Application
 
  @@songs = [Song.new("Sorry", "Justin Bieber"),
            Song.new("Hello","Adele")]
 
  def call(env)
    resp = Rack::Response.new
    req = Rack::Request.new(env)
 
    if req.path.match(/songs/)
 
      song_title = req.path.split("/songs/").last #turn /songs/Sorry into Sorry
      song = @@songs.find{|s| s.title == song_title}
 
      resp.write song.artist
    end
 
    resp.finish
  end
end
```

