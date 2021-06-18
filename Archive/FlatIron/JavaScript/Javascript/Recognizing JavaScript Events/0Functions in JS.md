"The full *function declaration* for `exerciseDog` is:

```javascript
function exerciseDog(dogName, dogBreed) {
  console.log(`Wake ${dogName} the ${dogBreed}`);
  console.log(`Leash ${dogName} the ${dogBreed}`);
  console.log(`Walk to the park ${dogName} the ${dogBreed}`);
  console.log(`Throw the fribsee for ${dogName} the ${dogBreed}`);
  console.log(`Walk home with ${dogName} the ${dogBreed}`);
  console.log(`Unleash ${dogName} the ${dogBreed}`);
}
//helpful tip for myself, string with interpolation use `` instead of ""
```

assign default arguments to our parameters.

```javascript
function exerciseDog(dogName="ERROR the Broken Dog", dogBreed="Sick Puppy") {
...
```

## Demonstrate *Return Values*

Functions in JavaScript can return things

```javascript
let weatherToday = "Rainy";
 
function exerciseDog(dogName, dogBreed) {
  if (weatherToday === "Rainy") {
    return `${dogName} did not exercise due to rain`;
  }
  console.log(`Wake ${dogName} the ${dogBreed}`);
  console.log(`Leash ${dogName} the ${dogBreed}`);
  console.log(`Walk to the park ${dogName} the ${dogBreed}`);
  console.log(`Throw the fribsee for ${dogName} the ${dogBreed}`);
  console.log(`Walk home with ${dogName} the ${dogBreed}`);
  console.log(`Unleash ${dogName} the ${dogBreed}`);
  return `${dogName} is happy and tired!`
}
 
let result = exerciseDog("Byron", "poodle");
console.log(result); // => "Byron did not exercise due to rain"
```

```javascript
function functionName(argument1, argument2, argument3) {
  body code goes here
}
//() <= this is an invocation operator
```

