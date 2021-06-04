## Use `map` to Transform an `Array`

```js
[10, 20, 30, 40].map(function(a) {
  return a * 2;
}); //=> [20, 40, 60, 80]
```

## Use `reduce` to Reduce an `Array` to a Value

```js
[10, 20, 30, 40].reduce(function(memo, i) { return memo + i }) //=> 100
[10, 20, 30, 40].reduce(function(memo, i) { return memo + i }, 100) //=> 200
```

Remember, ***JavaScript loves functions\*.** By being able to pass functions around comfortably, we can take advantage of methods that JavaScript gives us! Given what you know about writing your own `map` and `reduce` functions, read the JavaScript documentation and make sure you know how to use the versions JavaScript provides you:

- [map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
- [reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

## Use JavaScript Documentation to Learn More About Other Variations on `map` and `reduce`

## Filter

Look at the documentation for [filter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter). The syntax snippet is provided as:

```js
let newArray = arr.filter(callback(element[, index[, array]])[, thisArg])
Here, we're told that on an Array (arr), we add a .filter
```

> **REMEMBER** Because of JavaScript's non-enforcement of *arity*, we don't **have to** provide all the arguments.

We'll talk a lot more about `this` and `thisArg` shortly, but for the moment, we can *leave it out*.

Let's learn some more about the `callback`. The documentation tells us what parameters it supports.

```js
// ... callback(element[, index[, array]]) ...)
```

Possible parameters:

1. `element`
2. `index` (optional)
3. `array` (optional)

> **PRO-TIP**: After reading our description above, look at MDN's and make sure you know how to explain filter in your own words. Being able to learn from MDN is a core skill we must continually develop!

## filter

```js
[10, 20, 30, 40].filter(function() {
    return true;
  }) //=> [10, 20, 30, 40] (map, basically)
 
  [10, 20, 30, 40].filter(function(e) {
    return e < 30;
  }) //=> [10, 20]
 
  [10, 20, 30, 40].filter(function(e, index) {
    return index % 2 === 0;
  }) //=> [10, 30] (elements with an even-numbered index)
```

1. Map a collection
2. Only accumulate the elements that return a truthy value from the callback. The callback's mechanics are described above

## Provide a List of Collection-Processing Methods to Memorize

These are the collection-processing methods you should ***memorize\*** and practice heavily. They're going to be your friends every day in JavaScript-land. There are other collection-processing methods that you might not memorize, but it's pretty common for a developer to realize that they're working in the "Character of Collection Processing" and think, "Hm, maybe there's a collection-processing method for that...." When that moment hits, the developer comes *back* to the documentation and look for that "just-right" collection-processing method.

- [`every`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every): Did all elements satisfy the callback?
- [`find`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find): Find the first element that satisfies the callback
- [`findIndex`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex): Find the first element that satisfies the callback's index
- [`map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map): Transform every element and create a new array
- [`reduce`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce): Reduce every element into a new value
- [`some`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some): Did any element satisfy the callback?

 For a more simple condition, we use `Array.prototype.indexOf()`. For more complex calculations, we use `Array.prototype.find()`.

## Find Elements Using a Simple Condition with `Array.prototype.indexOf()`

`Array.prototype.indexOf()` depends on one required parameter, and one optional parameter. It compares the elements using the strict equality operator (===) and returns the index of the matching element.

```js
let cards = ['queen of hearts', 'jack of clubs', 'ten of diamonds', 'ace of spades'];
 
console.log(cards.indexOf('jack of clubs')); //1
 
```

The first parameter, the *item* that you are looking for, is required. You can also pass in the *start* position like so:

```js
let cards = ['queen of hearts', 'jack of clubs', 'ten of diamonds', 'ace of spades'];
 
console.log(cards.indexOf('jack of clubs', 2  )); // -1 
```

However, if you pass in a start position that is after the element that you're looking for, or if the element that you are looking for is not in the array, `Array.prototype.indexOf()` will return `-1` to let you know.

## Find Elements Using More Complex Conditions with `Array.prototype.find()`

`Array.prototype.find()` refers to a function to execute on each value in the array, taking in three arguments.

```js
 
function isOdd(element, index, array) {
  return (element %2 === 1);
}
 
console.log([4, 6, 8, 10].find(isOdd)); // undefined, not found
console.log([4, 5, 8, 10].find(isOdd)); // 5
console.log([4, 5, 7, 8, 10].find(isOdd)); // 5
console.log([4, 7, 5,  8, 10].find(isOdd)); // 7
```

`Array.prototype.find()` takes a callback and passes three arguments to it: the current element of the array, the index of the current element, and the array itself. It returns the value of the first element in the array that satisfies the condition specified by the function. Otherwise, undefined is returned.