SELF.episodes 

```ruby
def self.episodes
        i = 1
        while i < 3
        response = HTTParty.get("http://rickandmortyapi.com/api/episode?page=#{i}")
(#changed to) episodes = response["results"].each {|hash| Episode.new(hash)}
        episodes = response["results"].each {|hash| @@episodes << hash["name"]}
            
            i += 1
        end
        binding.pry
    end
```

need it iterate over hash and assign key value pairs, by Instiation through a new class in your api class.