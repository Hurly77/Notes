#### Looping y iterating

```javascript
const gifts = ["teddy bear", "drone", "doll"];
 
function wrapGift(gift) {
  # For Ruby or Pythonistas, note that the " is now a ` (back-tick)
  # We'll discuss interpolation in detail elsewhere, but note that
  # JavaScript uses ` like Ruby uses ".
  console.log(`Wrapped ${gift} and added a bow!`);
}
```

we can think of our **collection** of gifts as an **`Array`** and the act of wrapping them as a function. For example:

```javascript
const gifts = ["teddy bear", "drone", "doll"];
 
function wrapGift(gift) {
  # For Ruby or Pythonistas, note that the " is now a ` (back-tick)
  # We'll discuss interpolation in detail elsewhere, but note that
  # JavaScript uses ` like Ruby uses ".
  console.log(`Wrapped ${gift} and added a bow!`);
}
```

## The `for` loop

The `for` loop is made up of four statements in the following structure:

```javascript
for ([initialization]; [condition]; [iteration]) {
  [loop body]
}
```

- Initialization
  - Typically used to initialize a **counter** variable.
- Condition
  - An expression evaluated before each pass through the loop. If this expression evaluates to `true`, the statements in the loop body are executed. If the expression evaluates to `false`, the loop exits.
- Iteration
  - A statement executed at the end of each iteration. Typically, this will involve incrementing or decrementing a counter, bringing the loop ever closer to completion.
- Loop body
  - Code that runs on each pass through the loop.

***Usage\***: Use a `for` loop when you know exactly how many times you want the loop to run (for example, when you have an array of known size).

The code below will announce our next ten birthdays:

```javascript
for (let age = 30; age < 40; age++) {
  console.log(`I'm ${age} years old. Happy birthday to me!`);
}
 
// LOG: I'm 30 years old. Happy birthday to me!
// LOG: I'm 31 years old. Happy birthday to me!
// LOG: I'm 32 years old. Happy birthday to me!
// LOG: I'm 33 years old. Happy birthday to me!
// LOG: I'm 34 years old. Happy birthday to me!
// LOG: I'm 35 years old. Happy birthday to me!
// LOG: I'm 36 years old. Happy birthday to me!
// LOG: I'm 37 years old. Happy birthday to me!
// LOG: I'm 38 years old. Happy birthday to me!
// LOG: I'm 39 years old. Happy birthday to me!
```

`let age = 30` is the **initialization**

The **condition** for the above loop is `age < 40`

The **iteration** is `age++`, which increments the value of `age` by `1` after every pass through the loop

The **loop body** is the set of statements that we want to run when the condition evaluates to `true`.

The `for` loop is often used to iterate over every element in an array. Let's rewrite our gift-wrapping action above as a `for` loop:

```javascript
const gifts = ["teddy bear", "drone", "doll"];
 
function wrapGifts(gifts) {
  for (let i = 0; i < gifts.length; i++) {
    console.log(`Wrapped ${gifts[i]} and added a bow!`);
  }
 
  return gifts;
}
 
wrapGifts(gifts);
// LOG: Wrapped teddy bear and added a bow!
// LOG: Wrapped drone and added a bow!
// LOG: Wrapped doll and added a bow!
// => ["teddy bear", "drone", "doll"]
```

# Assignment

In `index.js`, build a function named `writeCards()` that accepts two arguments: an array of string names, and an event name. Create a `for` loop with a counter that starts at `0` and increments at the end of each loop. The condition should halt the `for` loop after the last name in the array is printed out in the loop body. 

Inside the loop, create a custom message for each name from the provided array, thanking that person for their gift

```javascript
const myArray = []
function writeCards(array, _event) {
  for (let i = 0; i < array.length; i++){
    myArray.push(`Thank you, ${array[i]}, for the wonderful ${_event} gift!`);
  }
  return myArray
}
```

## The `while` loop

The `while` loop is similar to a `for` loop, repeating an action in a loop based on a condition. Both will continue to loop until that condition evaluates to `false`. Unlike `for`, `while` only requires condition and loop statements:

```javascript
while ([condition]) {
  [loop body]
}
```

In fact, we could rewrite our original `for` loop gift wrapping example using a `while` loop and achieve the exact same result:

```javascript
const gifts = ["teddy bear", "drone", "doll"];
 
function wrapGifts(gifts) {
  let i = 0; // the initialization moved OUTSIDE the body of the loop!
  while (i < gifts.length) {
    console.log(`Wrapped ${gifts[i]} and added a bow!`);
    i++; // the iteration moves INSIDE the body of the loop!
  }
 
  return gifts;
}
 
wrapGifts(gifts);
// LOG: Wrapped teddy bear and added a bow!
// LOG: Wrapped drone and added a bow!
// LOG: Wrapped doll and added a bow!
// => ["teddy bear", "drone", "doll"]
```

**CAUTION**: When using `while` loops, it is easy to forget to involve iteration. Leaving iteration out can result in a condition that *always* evaluate to `true`, causing an infinite loop!

`while` loops are sometimes used when we *want* a loop to run an indeterminate amount of times.

`while` loops are sometimes used when we *want* a loop to run an indeterminate amount of times.

```javascript
function plantGarden() {
  let keepWorking = true;
  while (keepWorking) {
    chooseSeedLocation();
    plantSeed();
    waterSeed();
    keepWorking = checkForMoreSeeds();
  }
}
```

## When to Use `for` and `while`

Often, you will see `while` loops simply being used as an alternative to `for` loops:

```javascript
let countup = 0;while (countup < 10) {  console.log(countup++);}
```

This is perfectly fine as an alternative way to describe:

```javascript
for (let countup = 0; countup < 10; countup++) {  console.log(countup);}
```

## Assignment

To get more acquainted with `while`, your task is to write a function, `countDown`, that takes in any positive integer and, starting from that number, counts down to zero using `console.log()`. So, when written if you were to run

```javascript
function countDown(num){
  while (num > -1){
    console.log(num--);
  }
}
```

