## story

Maybe a User would Like to test Their black Jack skill try and get a head of the curve the could do that via my app

### Rules

- Face Cards Are Worth 10+
- Aces are worth 1 or 11, which ever makes a better hand
- Each player starts with two cards, one of the dealer’s cords is hidden until the end
- to ‘Hit’ is to ask for another card. To ‘Stand’ is to hold your total and end your turn.
- over 21 is a ‘bust’, and dealer wins regardless
- If you go over 21 from the start (Ace & 10), you got a black jack
- Blackjack usually means you win 1.5 the amount of you bet. depends on the casino
- dealer will hits until his/her cards total 17 or higher
- doubling is like a hit, only the bit is doubled and you only get one more card
- Splitting, also doubles the bet, because each new hand is worth the original bet
- you can only double/split of the first move, or first move of a hand created by split
- you cannout play on two aces after the are slplit
- you can souble on a hand resulting from a split, tripling or quadrupling you bet.

```
wash
riffle + riffle + box + riffle
cut the deck
```

**(There are these)**

> **Table** (Paraent)
>
> **JavaScript** 
>
> - Deal
>   - Deck - needs to be Recycle, will take top two cards from array deal them to the player and two to the house last
>
> - Game
>   - (Black Jack) - Game needs to initialize the following
>     - Deal
>     - Hit
>     - split
>     - Double_Down
>     - Stay - keeps the the bet and Doesn’t hit, Proceeds the game
> - House
>   - Shuffle - needs to split the deck and stack it
>
> **Ruby API**
>
> - Players(child)
>   - Money
>   - Bet
>   - player Wins / House Wins
>
> - Deck
>
>   - Cards
>
>     - Rank - Clubs(),  Diamonds(), Hearts(), and Spades()
>
>     - colors - Black/ Red
>
>     - Values - (2 - 9), Faces(J, Q, K), and (Ace)

fetch 

```js
//get
fetch("https://anapioficeandfire.com/api/books")
  .then(resp => resp.json())
  .then(json => renderBooks(json))

------------------------------------------------------
//post
fetch('http://localhost:3000/users', {
    method: 'post',
		headers: {
      'Content-Type': 'application/json',
			Accept: 'application/json'
		},
		body: JSON.stringify({
      name: `${name}`,
			email: `${email}`
		})
	})
  .then(function(response) {
    return response.json();
  })
  .then(function(object) {
    document.body.innerHTML = object.id
  })
-------------------------------------------------------


//delete
const configObj = {
				method: 'DELETE',
				headers: {
					'Content-Type': 'application/json',
					Accept: 'application/json'
				}
fetch(`${POKEMONS_URL}/${event.target.dataset.pokemonId}`, configObj);
```

## Front End

Shuffle Algorithm

```JS
a = ["2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K", "A"]
a  = a.reduce(function(res, current, index, a){
    return res.concat([current, current, current, current])
}, [])

//for stacking one card after the other, This needs to happen, three times
a  = a.reduce(function(res, current, index, a){
    return res.concat([current, current, current, current])
}, [])

//for Shuffling cards on Table, THIS need to happen three times
--------------------------------------------------------------
function shuffle (array) {
  var i = 0
    , j = 0
    , temp = null

  for (i = array.length - 1; i > 0; i -= 1) {
    j = Math.floor(Math.random() * (i + 1))
    temp = array[i]
    array[i] = array[j]
    return array[j] = temp
  }
}
```

```
Classes
Deal
	Deck
Game 
	Name
	rules
House
```



## Back End

The back end should handle each deck, and contain the characters of each card.

classes should be as follows

Model

```
Table
has_many :card_games
has_many :playes

TableGame
belongs_to :table
has_many :players

Player
belongs_to :table
belongs_to :TableGame
```

 Controllers

```

```

Migrations

```
Table Attributes
game_ids

TableGame Attributes
Name
rules
games_played
deck
table_id

Player
Money
bet
name
wins/loses
```

```

```

