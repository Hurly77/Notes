## A Basic `class`

The `class` syntax was introduced in [ECMAScript 2015](https://www.w3schools.com/js/js_es6.asp) and it's important to note that the `class` keyword is just syntactic sugar, or a nice abstraction, over JavaScript's existing prototypal object structure.

Reminder: All JavaScript objects inherit properties and methods from a `prototype`. This includes standard objects like functions and data types.

## Accessing Instance Properties

By using `this.name` and `this.age` to define properties in our `constructor`, we can also refer to these properties within other methods of our `class`:

```js
class Fish {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
 
    sayName() {
        return `Hi my name is ${this.name}`;// inside Object block
    }
}

// another example

class Square {
  constructor(sideLength) {
    this.sideLength = sideLength;
  }
 
  area() {
    return this.sideLength * this.sideLength;
  }
}
 
let square = new Square(5);
square; // => Square { sideLength: 5 }
square.sideLength; // => 5
square.area(); // => 25
```

#### Private Properties

Currently, there is no official way to make a property private - all `class` and object properties are exposed as we see above. One common convention, however, is to include an underscore at the beginning of the property name to indicate those properties are not intended to be accessed from outside the `class`:

```js
class Transaction {
  constructor(amount, date, memo) {
    this._amount = amount;
    this._date = date;
    this._memo = memo;
  }
}
```

# lab

```js
// Write your code here
class Breakfast {
  constructor(food, drink) {
    this.food = food;
    this.drink = drink;
  }
}

class Lunch {
  constructor(salad, soup, drink) {
    this.salad = salad;
    this.soup = soup;
    this.drink = drink;
  }
}

class Dinner {
  constructor(salad, soup, entree, dessert){
    this.salad = salad;
    this.soup = soup;
    this.entree = entree;
    this._dessert = dessert;
  }
}
```

# Lab 2 Behavior with Method

```js
class Cat {
	constructor(name) {
		this.name = name;
	}
	speak() {
		return `${this.name} says meow!`;
	}
}

class Dog {
	constructor(name) {
		this.name = name;
	}
	speak() {
		return `${this.name} says woof!`;
	}
}

class Bird {
	constructor(name, gender) {
		this.name = name;
		this.gender = gender;
	}

	speak() {
		if (this.gender == 'male') {
			return `It's me! ${this.name}, the parrot!`;
		} else {
			return `${this.name} says squawk!`;
		}
	}
}
```

