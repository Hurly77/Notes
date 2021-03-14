# LAB 1

## The `greet()` function

The `greet` function should take one argument, a `String` which specifies the 24-hour time in the format `HH:MM`.

- If the time is earlier than 12pm, return "Good Morning".
- If the time is between 12pm and 5pm, return "Good Afternoon".
- If the time is later than 5pm, return "Good Evening".

**NOTE:** The value returned from the `<input>` will be of type `String`. You’ll need to take the `String` of the 24 hour time and convert it to a number. The `split()` [function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split) and `parseInt()` [function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt) should help.

## The `displayMessage()` function

The `displayMessage` function should take one argument, a `String`.

When the function runs it should update the text inside the `#greeting` node with the content of the argument.

It does not return anything.

```javascript
function greet(time){
  const parsed = parseInt(time);
  if (parsed < 12) return "Good Morning";
  if (parsed > 17) return "Good Evening";
  if (parsed > 11 && 17 > parsed) return "Good Afternoon";
}


/* Write your implementation of displayMessage() */
function displayMessage(message) {
  let h1 = document.getElementById("greeting");
  h1.innerText = message;
    //tried to use innerHTML but would not work
}
```

`innerHTML` vs. `innerText`

`innerHTML` allows you to add tags to the DOM, and will be read by the browser

`innerText` only allows you to enter text into an element  tag

also, 

`textContent` will do the same thing as `innerText` with on subtle difference it allows us to see the styling as well.

# LAB 2

## Create the Array o' Functions

Continue writing *"generalized"* functions for

- `wakeDog`
- `leashDog`
- `walkToPark`
- `throwFrisbee`
- `walkHome`
- `unleashDog`

Each function's implementation will be a generalized invocation of `console.log()`.

## Create the Array o' Functions

Next, create our "Array o' Functions!" Create a variable called `routine`. This variable will be an `Array` all of the functions we've just defined.

## Create a Function to Process the Array o' Functions

Lastly, create the function called `exerciseDog` that will take in two arguments:

- `dogName`
- `dogBreed`

The function's implementation should

- Iterate over the `routine` `Array`
- Call each function in the array and
- pass the `dogName` and `dogBreed` received by `exerciseDog()` to the function as they are *called*
- capture the result of each function's call
- return an `Array` of all those functions' return values

```javascript
function wakeDog(dogName, dogBreed) {
return `Wake ${dogName} the ${dogBreed}`;
}
function leashDog(dogName, dogBreed) {
  return `Leash ${dogName} the ${dogBreed}`;
}
function walkToPark(dogName, dogBreed) {
  return `Walk to the park with ${dogName} the ${dogBreed}`;
}
function throwFrisbee(dogName, dogBreed) {
  return `Throw the frisbee for ${dogName} the ${dogBreed}`;
}
function walkHome(dogName, dogBreed) {
  return `Walk home with ${dogName} the ${dogBreed}`;
}
function unleashDog(dogName, dogBreed) {
  return `Unleash ${dogName} the ${dogBreed}`;
}
// relized you don't need the invocation operator => () when useing an array of functions
let routine = [
  wakeDog,
  leashDog,
  walkToPark,
  throwFrisbee,
  walkHome,
  unleashDog
];
function exerciseDog(dogName, dogBreed){
  let newArray = []
  for (let i = 0; i < routine.length; i++){
      // invoction operation goes after the index[num]
    let value = routine[i](dogName, dogBreed)
   // decided to call console.log in iteration instead of writting it in every function
    console.log(value)
    newArray.push(value)
  } 
  return newArray

```

