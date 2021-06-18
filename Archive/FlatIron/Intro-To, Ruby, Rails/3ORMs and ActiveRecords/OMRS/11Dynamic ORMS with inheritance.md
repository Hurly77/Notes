## The Child Class

```ruby
require_relative "./interactive_record.rb"
 
class Song < InteractiveRecord  #class inherits from
 
  self.column_names.each do |col_name| #column_names is inherited from the super class
    attr_accessor col_name.to_sym
  end
 
end
```

## Code in Action

```ruby
#this is the executable file in "bin/run"
song = Song.new(name: "Hello", album: "25")#Here we create a new Song instance
#Remember, initialize is inherited from the InteractiveRecord class
 #than the info about name and album is printed out 
puts "song name: " + song.name
puts "song album: " + song.album
song.save
 
puts Song.find_by_name("Hello")# is used to search the database for the newly created song.
```

When `ruby bin/run` is run in your terminal, it should produce the following, confirming that the song was saved:

```bash
song name: Hello
song album: 25
{"id"=>1, "name"=>"Hello", "album"=>"25"}
#The #initialize, #save and #find_by_name methods used by Song here were inherited from InteractiveRecord.
```

As we begin to build complex web applications using Sinatra and Rails, this pattern of inheritance will become familiar.