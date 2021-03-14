### dPort error

error saying **port 3000 already in use**. In this case, do the following:

Run `lsof -i :3000` to inspect what's running on port 3000 (alternatively, you can try `ps aux | grep 3000`). You will see something like the following:

```
COMMAND PID USER FD TYPE DEVICE SIZE/OFF NODE NAME 
ruby 9639 matteo 28u IPv4 0x89939df84558ba7 0t0 TCP localhost:hbci (LISTEN) 
ruby 9639 matteo 29u IPv6 0x89939dfa2ef1897 0t0 TCP localhost:hbci (LISTEN)
```

Kill the process running by doing `kill -9 <PID_HERE>`, so in the case of my example `kill -9 9639`. Now the `rails s` command should start the server on port 3000 without issues.

> **Aside**: In general, you can always specify what port to use when running the server, by running: `rails s -p PORT_NUMBER` (eg: `rails s -p 3001`).`

### Suggested HTML

```html
<div class="card" data-id="1"><p>Prince</p>
  <button data-trainer-id="1">Add Pokemon</button>
  <ul>
    <li>Jacey (Kakuna) <button class="release" data-pokemon-id="140">Release</button></li>
    <li>Zachariah (Ditto) <button class="release" data-pokemon-id="141">Release</button></li>
    <li>Mittie (Farfetch'd) <button class="release" data-pokemon-id="149">Release</button></li>
    <li>Rosetta (Eevee) <button class="release" data-pokemon-id="150">Release</button></li>
    <li>Rod (Beedrill) <button class="release" data-pokemon-id="151">Release</button></li>
  </ul>
</div>
```

### Building out the project

Remember that your user stories are:

- When a user loads the page, they should see all trainers, with their current team of Pokemon.
- Whenever a user hits "Add Pokemon" and they have space on their team, they should get a new Pokemon.
- Whenever a user hits "Release Pokemon" on a specific Pokemon team, that specific Pokemon should be released from the team.

You should build out just enough of your Rails API to achieve the above. You *should not* build out full CRUD on each model. For example, the frontend will not have the ability to create a new Trainer, so your backend should not have a `POST /trainers` route.

## Example API Requests

### Getting All Trainers and their Pokemon

```js
//#=> Example Request
GET /trainers
 
//#=> Example Response
[
  {
    "id":1,
    "name":"Prince",
    "pokemons":[
      {
        "id":140,
        "nickname":"Jacey",
        "species":"Kakuna",
        "trainer_id":1
      },
      {
        "id":141,
        "nickname":"Zachariah",
        "species":"Ditto",
        "trainer_id":1
      },
      // 
    ]
  }
  // ...
]
```

### Adding a Pokemon

Note: When adding a new pokemon, the nickname should be generated using the `Faker::Name` gem and the species should be generated using the `Faker::Games::Pokemon` gem. See the seeds.rb file above as an example.

```ruby
#=> Example Request
POST /pokemons
 
Required Headers:
{
  'Content-Type': 'application/json'
}
 
Required Body:
{
  "trainer_id": 1
}
 
#=> Example Response
{
  "id":147,
  "nickname":"Gunnar",
  "species":"Weepinbell",
  "trainer_id":1
}
```

### Releasing a Pokemon

```ruby
#=> Example Request
DELETE /pokemons/:pokemon_id
 
#=> Example Response
{
  "id":147,
  "nickname":"Gunnar",
  "species":"Weepinbell",
  "trainer_id":1
}
```

##  Pokémon_teams.gif

![pokemon_teams](C:\Users\camer\dev\flatiron\labs\js-rails-as-api-pokemon-teams-project-new-onl01-seng-pt-021020\pokemon-teams-frontend\assets\pokemon_teams.gif)

# scratch

### JavaScript

```js
let ul = document.createElement('ul'),
				li = document.createElement('li');
			lilBtn = document.createElement('button');
			div.appendChild(ul);
			ul.appendChild(li);
			ul.appendChild(lilBtn);

releaseBtn = document.createElement('button');
        releaseBtn.innerText = "Add Pokemon";
```

# lab

```JS
const BASE_URL = 'http://localhost:3000';
const TRAINERS_URL = `${BASE_URL}/trainers`;
const POKEMONS_URL = `${BASE_URL}/pokemons`;

document.addEventListener('DOMContentLoaded', () => {
	fetch(TRAINERS_URL)
		.then((response) => {
			return response.json();
		})
		.then((trainers) => {
			trainerData(trainers['data']);
		});
});

function addPokemon(id){
  const trnId = id
  fetch(POKEMONS_URL, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      Accept: 'application/json'
    },
    body: JSON.stringify({
      trainer_id: trnId
    })
  })
  .then(function(response) {
    return response.json();
  })
  .then(function(object) {
    trainerData(object)
    
  })
  .catch((error) => {
    console.log(error);
  });
};


function trainerData(trainers) {
  let main = document.querySelector('main');
  
	trainers.forEach(function(trainer) {
    const trnId = trainer['id'];

    let div = document.createElement('div'),
    p = document.createElement('p'),
    addPokeBtn = document.createElement('button'),
    ul = document.createElement('ul');
    
    addPokeBtn.setAttribute('data-trainer-id', trainer['id']);
    addPokeBtn.setAttribute('class', 'add-poke')
    div.setAttribute('class', 'card');
		div.setAttribute('data-id', trainer['id']);
    
		main.appendChild(div, addPokeBtn);
		for (const key in trainer) {
      if (key == 'attributes') {
        let pokemons = trainer[key].pokemons;
        
				p.innerText = trainer[key].name;
				div.appendChild(p);
        
				addPokeBtn.innerText = 'Add Pokemon';
        addPokeBtn.addEventListener('click', (event) => {
          addPokemon(trnId)
          event.preventDefault()
        })
        
				div.appendChild(addPokeBtn);
				div.appendChild(ul);
        
				for (const key in pokemons) {
          let li = document.createElement('li');
					lilBtn = document.createElement('button');
					lilBtn.setAttribute('class', 'release');
					lilBtn.setAttribute('data-pokemon-id', pokemons[key].id);
					lilBtn.innerText = 'Release';
					li.innerText = `${pokemons[key].nickname} (${pokemons[key].species})`;
					li.appendChild(lilBtn);
          ul.appendChild(li);
				}
			}
		}
  });
  releasePokemon()
}

function releasePokemon() {
	const buttons = document.querySelectorAll('.release');
	buttons.forEach((button) => {
		button.addEventListener('click', () => {
			const configObj = {
				method: 'DELETE',
				headers: {
					'Content-Type': 'application/json',
					Accept: 'application/json'
				}
			};
			fetch(`${POKEMONS_URL}/${event.target.dataset.pokemonId}`, configObj);
			event.target.parentElement.remove();
		});
	});
}
```

## back-end

**controllers**

```ruby
class PokemonsController < ApplicationController
  def index
    pokemons = Pokemon.all
    render json: PokemonSerializer.new(pokemons)
  end

  def create
    trainer = Trainer.find(params[:trainer_id])
    pokemon = trainer.pokemons.build({
        nickname: Faker::Name.first_name,
        species: Faker::Games::Pokemon.name
    })
    if pokemon.save
        render json: pokemon
    else
        render json: {message: pokemon.errors.messages[:team_max][0]}
    end
  end

  def destroy
    pokemon = Pokemon.find_by(id: params[:id])
    pokemon.delete_all
  end
end

Trainer/controller
-----------------------------------------------------------

class TrainersController < ApplicationController
  def index
    trainers = Trainer.all
    render json: TrainerSerializer.new(trainers)
  end
end

```

**Serializers/models**

```ruby
#Serialzers
--------------
class PokemonSerializer
  include FastJsonapi::ObjectSerializer
  attributes :nickname, :id

  belongs_to :trainer
end


class TrainerSerializer
  include FastJsonapi::ObjectSerializer
  attributes :name, :pokemons
  
  has_many :pokemons
end

#Models
--------------------------------------------------------------
class Pokemon < ApplicationRecord
  belongs_to :trainer
end

class Trainer < ApplicationRecord
  has_many :pokemons
end
```

```ruby
#Schem
ActiveRecord::Schema.define(version: 2020_08_28_010119) do

  create_table "pokemons", force: :cascade do |t|
    t.string "species"
    t.string "nickname"
    t.integer "trainer_id", null: false
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
    t.index ["trainer_id"], name: "index_pokemons_on_trainer_id"
  end

  create_table "trainers", force: :cascade do |t|
    t.string "name"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
  end

  add_foreign_key "pokemons", "trainers"
end

#SEED DATA
---------------------------------------------------------------
require 'faker'
require 'securerandom'
 
Trainer.delete_all
Pokemon.delete_all
 
trainers_name = [
  'Natalie',
  'Prince',
  'Dick',
  'Rachel',
  'Garry',
  'Jason',
  'Matt',
  'Niky',
  'Ashley'
]
 
trainer_collection = []
 
trainers_name.each do |name|
  trainer_collection << Trainer.create(name: name)
end
 
trainer_collection.each do |trainer|
  team_size = (SecureRandom.random_number(6) + 1).floor
 
  (1..team_size).each do |poke|
    name = Faker::Name.first_name
    species = Faker::Games::Pokemon.name
    Pokemon.create(nickname: name, species: species, trainer_id: trainer.id)
  end
end
```