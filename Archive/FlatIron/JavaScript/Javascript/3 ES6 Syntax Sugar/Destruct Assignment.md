When we want to assign variables from the top level in JS we normally do it individually:

```js
const doggie = {
  first: 'Buzz',
  breed: 'Great Pyrenees',
  fur_color: 'black and white',
  activity_level: 'sloth-like',
  favorite_food: 'hot_dogs'
};
const first = doggie.first;  // Buzz
const breed = doggie.breed; // Great Pyrenees
```

This is repetitive code. The algorithm at play is:

 Declare a variable with a name (e.g. `first` or `breed`)

1. Use that variable's name to point to an attribute in the `Object` whose name matches the name of the variable (e.g. `doggie.breed` or `doggie.first`)
2. Assign the attribute's value to the created variable

JavaScript gives us the ability to perform this algorithm with *one* simple line of code.

```js
const doggie = {
  first: 'Buzz',
  breed: 'Great Pyrenees',
  fur_color: 'black and white',
  activity_level: 'sloth-like',
  favorite_food: 'hot_dogs'
};
 
const { first, breed } = doggie;
console.log(first); 
console.log(breed); 
 
```

We can also use it to destructure a nested syntax:

```js
const doggie = {
  first: 'Buzz',
  breed: 'Great Pyrenees',
  fur_color: 'black and white',
  activity_level: 'sloth-like',
  favorite_foods: {
    meats:{
      ham: 'smoked',
      hot_dog: 'oscar_meyer',
    },
    cheeses:{
      american: 'kraft'
    }
  }
};
 
const { ham, hot_dog } = doggie.favorite_foods.meats;
console.log(ham); 
console.log(hot_dog); 
```

Destructuring does not just work on objects - we can also use the same syntax with `Array`s as well.

The cool part is we can pick the parts of the `Array` that we want to assign!

```js
const dogs = ['Great Pyrenees', 'Pug', 'Bull Mastiff']
const [, small, giant] = dogs
console.log(small, giant) //  Pug, Bull Mastiff
```

We can also destructure with strings, as a whole:

```js
const dogsName = 'Sir Woody BarksALot'
const [title, firstName, lastName] = 'Sir Woody BarksALot'.split(' ')
console.log(title, firstName, lastName) // Sir Woody BarksALot
```

And we can also destructure it in parts, just as we did with `Array`s above:

```js
const dogsName = 'Sir Woody BarksALot'
const [title, ,lastName] = 'Sir Woody BarksALot'.split(' ')
console.log(title, lastName) // Sir BarksALot
```

