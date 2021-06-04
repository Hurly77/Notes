   It is not amongst the most popular of opinions to think of code as being "realistic", what does that even mean, and what would its purpose even be? I wouldn't consider myself to be clever or the sherlock holms type, I don't seek to fill unanswered questions, at least not usually. Though In the last project I was working on, "JS and Rails API", I did so often chase what seemed too pointless questions, ones that, beyond my immediate desire, were far from relative to the task at hand. Though I found one thing particularly interesting, and for me at least, thought-provoking. I wondered if simulating code to behave like the natural world, is useful or just plain dumb.
	  Now, for some, it may seem like an utterly stupid Idea, or relatively naïve, and it may be. But, I digress. Let's start from The beginning, shall we? My project started off as what I thought to be a relatively simple and smooth sailing Idea, and for most, it would have been. I wanted to create a Blackjack game, How exciting right. It would have been so simple a game as simple as, and please mind my pseudocode, " if playing cards are more than house cards, the player loses else player wins", seems simple enough. Well, the truth of the matter is that it is. Then why I am I writing this then, well I've had a little motto since I started my venture into code "it's more important to understand why something works than why it does". Bare with me if you will. What I'm saying is that just because something can be simple does not necessarily mean that is it is or that it should. for example, I was working on a function for shuffling and I did some digging and I found this nifty little trick.

```js
 function shuffle (array) {
 let i = 0
    , j = 0
    , tmp = null

  for (i = array.length - 1; i > 0; i -= 1) {
    j = Math.floor(Math.random() * (i + 1))
    temp = array[i]
    array[i] = array[j]
    array[j] = temp
  }
}
```

This is simply just taking one element and swapping it with a random element . By the way, this is really good, casinos would love it if they could adopt a shuffling machine or dealer who could shuffle this well, but that's it isn't it. In reality, we don't have objects like `Math.random().`

  Now as I'm sure your aware, incomplete truth or as fare as my knowledge leads me to believe there are no random occurrences, everything can be calculated down and repeated if one had the 100% precision and info. What some might perceive to be "random" is just a group of events to large and too complicated for someone to keep track of. That is where I come in "dud, da du daaa", I thought it might be a nice idea to try and replicate how a deck of cards is shuffle or at least to as much as possible. Professionals Use a method called Riffle, Riffle, Box, Riffle

```js
const riffle = (deck) => {
	let halfDeck = deck,
		tmp = [],
		half = Math.floor(halfDeck.length / 2),
		h1 = halfDeck.slice(0, half),
		h2 = halfDeck.slice(half, halfDeck.length);

for (let i = 0; i < half; i++) {
	if (i % 2 == 1 || i == 0) {
		tmp.push(h1[i]);
		tmp.push(h2[i]);
	} else {
		tmp.push(h2[i]);
		tmp.push(h1[i]);
	}
}
return tmp;
```

Here Instead of just grabbing a random card from the array, I stack one card on top of the other forming a new array of cards. It is really quite simple. Though most might choose efficiency over reality. However given that you want to give some one the actual experience of playing a game of cards then you might be incline to code it like how you would actually shuffle it.

I’m not trying to claim a superiority, that this is better or more efficient, but given that you want an accurate simulated out come it might be more beneficial to think of how in reality something like shuffling cards is made to seem random and how you might copy that pattern.   I guess what  I’m trying to say is that visualizing how things would actually behave and coding with that in mind could be a very useful tool. It might help spark you thoughts so you can then later try and evaluate how to maximize the efficiency. 