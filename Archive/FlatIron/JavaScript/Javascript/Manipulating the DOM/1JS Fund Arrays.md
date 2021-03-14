`getElementById()` returns a **singular** element whereas the other two will return **all elements** that match the tag or class name.

**NOTE**: These methods **don't** actually return `Array`s. An `HTMLCollection` is *actually* returned from the first two methods. This is a major point of confusion in JavaScript! the `HTMLCollection` knows its length like an `Array`. We can iterate across it with a `for...in...` loop. **But** it's not *technically* an `Array`. This is definitely one of the "warts" of JavaScript but also shows a really mean-spirited interview question. It is not difficult to turn an `HTMLCollection` into an actual `Array` (using a loop, for example) but for our purposes we can treat it as an `Array`.

We can ask the **DOM** to give us a collection by asking it to do:

```javascript
document.getElementsByTagName('p')
```

We can get a single member of a collection by providing an element number or, *index* in `[]` after the collection. This was done as follows:

```javascript
document.getElementsByTagName('main')[0]
```

**NOTE**: In some other languages `Arrays` *cannot be of multiple types*. In C, C++, Java, Swift, and others you cannot mix types. JavaScript, Python, Ruby, Lisp, and others permit this.

Just like any other type of JavaScript data, we can assign an `Array` to a variable:

```javascript
const primeNumbers = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37];
 
const tvShows = ['Game of Thrones', 'True Detective', 'The Good Wife', 'Empire'];
```

We can **find** out **how many** elements an `Array` contains by checking the `Array`'s built-in `length` property:

```javascript
const myArray = ['This', 'array', 'has', 5, 'elements'];
 
myArray.length;
// => 5
```

how many elements are in the `Array`

```javascript
const alphabet = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z'];
// => undefined
 
alphabet.length;
// => 26
 
alphabet[alphabet.length - 1];
// => "z"
```

This is why it's called the ***computed\*** *member access operator*. We put an expression (`alphabet.length - 1`) inside the square brackets, and the JavaScript engine *computed* the value of that expression to determine which element we were trying to access. In this case, `alphabet.length - 1` evaluated to `25`, so `alphabet[alphabet.length - 1]` became `alphabet[25]`.

the more flexible and less error-prone solution is:

```javascript
let powerBall = winningNumbers[winningNumbers.length - 1]
```

## Add Elements to an Array

With the `.push()` method, we can add elements to the end of an `Array`:

```javascript
const superheroes = ['Catwoman', "Miss Marvel", 'She-Hulk', 'Jessica Jones'];
 
superheroes.push('Wonder Woman');
// => 5
 
superheroes;
// => ["Catwoman", "Miss Marvel", "She-Hulk", "Jessica Jones", "Wonder Woman"]
```

We can also `.unshift()` elements onto the beginning of an `Array`:

```javascript
const cities = ['New York', 'San Francisco'];
 
cities.unshift('Los Angeles');
// => 3
 
cities;
// => ["Los Angeles", "New York", "San Francisco"]
```

Both `.push()` and `.unshift()` update or *mutate* the original `Array`, adding elements directly to it. Operations that modify the original collection are *destructive*, and those that leave the original collection intact are *nondestructive*.

### Spread Operator

The spread operator allows us to spread out the contents of an existing `Array` into a new `Array`, adding new elements but preserving the original:

```javascript
const coolCities = ['New York', 'San Francisco'];
 
const allCities = ['Los Angeles', ...coolCities];
 
coolCities;
// => ["New York", "San Francisco"]
 
allCities;
// => ["Los Angeles", "New York", "San Francisco"]
```

We can also use the spread operator to add a new item to the end of an `Array` without modifying the original:

```javascript
const coolCats = ['Hobbes', 'Felix', 'Tom'];
 
const allCats = [...coolCats, 'Garfield'];
 
coolCats;
// => ["Hobbes", "Felix", "Tom"]
 
allCats;
// => ["Hobbes", "Felix", "Tom", "Garfield"]
```

## Remove Elements from an Array

**Destructive**

```javascript
//pop

const days = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
 
days.pop();
// => "Sun"
 
days;
// => ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat"]

const days = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
 
days.shift();
// => "Mon"
 
days;
// => [Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]
```

Notice that the return value for the `.pop()` and `.shift()` methods is the element that was removed.

### `.**slice()**`

**non-destructive**

```javascript
const primes = [2, 3, 5, 7];
 
const copyOfPrimes = primes.slice();
 
primes;
// => [2, 3, 5, 7]
 
copyOfPrimes;
// => [2, 3, 5, 7]
```

Note that the `Array` returned by `.slice()` has the same elements as the original, but it's a copy — **the two `Array`s point to different objects in memory**. If you add an element to one of the `Array`s, it does **not** get added to the other:

```javascript
const primes = [2, 3, 5, 7];
 
const copyOfPrimes = primes.slice();
 
primes.push(11);
// => 5
 
primes;
// => [2, 3, 5, 7, 11]
 
copyOfPrimes;
// => [2, 3, 5, 7]
```

#### With Arguments

```javascript
const days = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
 
days.slice(2, 5);
// => ["Wed", "Thu", "Fri"]
```


If no second argument is provided, the slice will run from the index specified by the first argument to the end of the `Array`:

```javascript
const days = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
 
days.slice(5);
// => ["Sat", "Sun"]
```

To remove the first element and return a new `Array`, we call `.slice(1)`:

```javascript
const days = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
 
days.slice(1);
// => ["Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]
```

And we can remove the last element in a way that will look familiar from earlier in this lesson:

```javascript
const days = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
 
days.slice(0, days.length - 1);
// => ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat"]
```

In fact, `.slice()` provides an easier syntax for grabbing the last element in an `Array`:

```javascript
const days = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
 
days.slice(-1);
// => ["Sun"]
 
days.slice(-3);
// => ["Fri", "Sat", "Sun"]
```

***DO NOT read about slice() and shrug and move on. It's used everywhere in JavaScript code and having to look up it's parameters every time is a major time-loss. Memorize its call format now.\***

### `.splice()`

```
array.splice(start)
array.splice(start, deleteCount)
array.splice(start, deleteCount, item1, item2, ...)
```

#### With a Single Argument

```javascript
array.splice(start)
```

The first argument expected by `.splice()` is the index at which to begin the splice

**If we only provide the one argument,** `.splice()` will **destructively** remove a chunk of the original `Array` beginning at the provided index and continuing to the end of the `Array`:

```javascript
const days = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
 
days.splice(2);
// => ["Wed", "Thu", "Fri", "Sat", "Sun"]
 
days;
// => ["Mon", "Tue"]
```

**With a negative 'start' index, the opposite happens:**

```javascript
const days = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
// => ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]
 
days.splice(-2);
// => ["Sat", "Sun"]
 
days;
// => ["Mon", "Tue", "Wed", "Thu", "Fri"]
```

#### With Two Arguments

```javascript
array.splice(start, deleteCount)
```

**two arguments to** `.splice()`, the first is still the index at which to begin splicing, and the second dictates how many elements we want to remove from the `Array`

```javascript
const days = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
// => ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]
 
days.splice(2, 3);
// => ["Wed", "Thu", "Fri"]
 
days;
// => ["Mon", "Tue", "Sat", "Sun"]
```

## Replace Elements in an Array

### `.splice()` with 3+ arguments

```javascript
array.splice(start, deleteCount, item1, item2, ...)
```

After the first two, every additional argument passed to `.splice()` will be inserted into the `Array` at the position indicated by the first argument. We can replace a single element in an `Array` as follows, discarding a card and drawing a new one:

```javascript
const cards = ['Ace of Spades', 'Jack of Clubs', 'Nine of Clubs', 'Nine of Diamonds', 'Three of Hearts'];
 
cards.splice(2, 1, 'Ace of Clubs');
// => ["Nine of Clubs"]
 
cards;
// => ["Ace of Spades", "Jack of Clubs", "Ace of Clubs", "Nine of Diamonds", "Three of Hearts"]
```

Or we can remove two elements and insert three new ones

```javascript
const menu = ['Jalapeno Poppers', 'Cheeseburger', 'Fish and Chips', 'French Fries', 'Onion Rings'];
 
menu.splice(1, 2, 'Veggie Burger', 'House Salad', 'Teriyaki Tofu');
// => ["Cheeseburger", "Fish and Chips"]
 
menu;
// => ["Jalapeno Poppers", "Veggie Burger", "House Salad", "Teriyaki Tofu", "French Fries", "Onion Rings"]
```

adding new books to our library in alphabetical order:

```javascript
const books = ['Bleak House', 'David Copperfield', 'Our Mutual Friend'];
 
books.splice(2, 0, 'Great Expectations', 'Oliver Twist');
// => []
 
books;
// => ["Bleak House", "David Copperfield", "Great Expectations", "Oliver Twist", "Our Mutual Friend"]
```

### Using the Computed Member Access Operator to Replace Elements

If we only need to replace a single element in an `Array`, there's a simpler solution than `.splice()`:

```javascript
const cards = ['Ace of Spades', 'Jack of Clubs', 'Nine of Clubs', 'Nine of Diamonds', 'Three of Hearts'];
 
cards[2] = 'Ace of Clubs';
// => "Ace of Clubs"
 
cards;
// => ["Ace of Spades", "Jack of Clubs", "Ace of Clubs", "Nine of Diamonds", "Three of Hearts"]
```

 **computed member access operator (`[]`) is still *destructive***

 There's a *nondestructive* way to replace or add items at arbitrary points within an `Array`

### Slicing and Spreading

*nondestructively*, leaving the original `Array` unharmed:

```javascript
const menu = ['Jalapeno Poppers', 'Cheeseburger', 'Fish and Chips', 'French Fries', 'Onion Rings'];
 
const newMenu = [...menu.slice(0, 1), 'Veggie Burger', 'House Salad', 'Teriyaki Tofu', ...menu.slice(3)];
 
menu;
// => ["Jalapeno Poppers", "Cheeseburger", "Fish and Chips", "French Fries", "Onion Rings"]
 
newMenu;
// => ["Jalapeno Poppers", "Veggie Burger", "House Salad", "Teriyaki Tofu", "French Fries", "Onion Rings"]
```

## Identify Nested Arrays

In the above 'slicing and spreading' example, if we don't use the spread operator we're left with an interesting result:

```javascript
const menu = ['Jalapeno Poppers', 'Cheeseburger', 'Fish and Chips', 'French Fries', 'Onion Rings'];
 
const newMenu = [menu.slice(0, 1), 'Veggie Burger', 'House Salad', 'Teriyaki Tofu', menu.slice(3)];
 
newMenu;
// => [["Jalapeno Poppers"], "Veggie Burger", "House Salad", "Teriyaki Tofu", ["French Fries", "Onion Rings"]]
```

```javascript
const egregiouslyNestedArray = ['How', ['deep', ['can', ['we', ['go', ['?'], 'Pretty'], 'dang'], 'deep,'], 'it'], 'seems.'];
```

**in general, try to keep your `Array`s to no more than two levels deep**

```javascript
const board = [
  ['X', 'O', ' '],
  [' ', 'X', 'O'],
  ['X', ' ', 'O']
];
 
board;
// => [["X", "O", " "], [" ", "X", "O"], ["X", " ", "O"]]
```

```javascript
board[1];// => [" ", "X", "O"]
```

The second `[]` operator specifies the square within that row, left (`board[1][0]`), middle (`board[1][1]`), or right (`board[1][2]`). For example:

```javascript
board[0][0];// => "X" board[0][2];// => " " board[2][2];// => "O"
```

# lab

```javascript
// Write your solution here!
const append = ["Milo", "Otis", "Garfield"]
const prepend = ["Milo", "Otis", "Garfield"]
const removeLast = ["Milo", "Otis", "Garfield"]
const removeFirst = ["Milo", "Otis", "Garfield"]

append.push("Odie");
prepend.unshift("Odie")
removeLast.pop()
removeFirst.shift()
```

