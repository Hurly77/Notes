### module

```
module Memorable
  module ClassMethods
    def reset_all
      self.all.clear
    end
```



    def count
      self.all.count
    end
  end
  module InstanceMethods
    def initialize
    self.class.all << self
    end
  end
end

```
mp3 = Dir.new("./spec/fixtures/mp3s")
mp3.children
artist_name, song_name, genre_name = file_name.chomp(".mp3").split(" - ")
```

[^This sets each element to a variable in chronlogical oder]: 



```
Dir.entries(self.path).select {|file| file.include?("mp3") 
```

[^brings up the direrory of file than iterates over them to lookind for mp3s]: 

```
def list_songs
Song.all.sort_by(&:name).each.with_index(1) do |song, idx|
  puts "#{idx}. #{song.artist.name} - #{song.name} - #{song.genre.name}"
end
end
```



```
def play_song
  puts "Which song number would you like to play?"
  input = gets.chomp.to_i
  if (1..Song.all.length).include?(input)
  song = Song.all.sort_by(&:name)[input - 1]
end
  puts "Playing #{song.name} by #{song.artist.name}" if song
end
end
```


