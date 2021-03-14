## Declare a Function Using a Function Expression

Thus far we've only ever used a *function declaration*:

```js
function foo() {  return 'bar';}
```

A function can also be written:

```js
let foo = function() {  return 'bar';}
```

Try thinking about the following code. Will this work? Does it work? Try to explain to yourself what's happening using the words "anonymous function" and "`call`." If you need help, see [Immediately Invoked Function Expression](https://developer.mozilla.org/en-US/docs/Glossary/IIFE) in the Resources section below.

```
(function() { console.log("Hello world") })()
```

Around 2015, developers got tired of typing `f-u-n-c-t-i-o-n` over and over. They lobbied the ECMAScript body for a way to write functions in a very short way. Here is the result, the "Arrow Function."

```js
let add = (parameter1, parameter2) => parameter1 + parameter2
add(2,3) //=> 5
```

First, add is the name of a variable to which an *anonymous function* is assigned. Nothing new there. So, let's look to the right of the `=`.

```js
(parameter1, parameter2) => parameter1 + parameter2
// Parameter list ^^^^^   // Function Body ^^^^^^^^
```

If your arrow function has only one parameter, the `()` become optional around the parameter:

```js
let twoAdder = x => x + 2;
// is the same as
let twoAdder = (x) => x + 2;
//Almost all developers will drop the parentheses in this case.
```

If we need to do more work than return a single expression, we'll need `{}` to wrap the multiple lines of code, **and** we'll have to declare a `return`. That sweet no-`return` syntax is only available if your function body is one expression long.

```js
let sum = (parameter1, parameter2) => {
  console.log(`Adding ${parameter1}`);
  console.log(`Adding ${parameter2}`);
  return parameter1 + parameter2;
}
sum(1,2) //=> 3
```

## Describe Situations Where Arrow Functions Are Used

Using arrow functions often appears when using JavaScript's *iterator* methods. 

```js
const nums = [1,2,3];
const squares = nums.map(x => x ** 2); //=> [1,4,9]
const doubles = nums.map(x => x * 2); //=> [2,4,6]
```

If all this math stuff seems a bit too textbook-y, be reassured that we can iterate through anything, not just numbers. We can do something similar with DOM elements:

```js
finishedItems = overdueTodoItems.map( item => item.className = "complete" );
header.innerText = `You finished ${finishedItems.length} items!`;
```

Or billing software:

```js
lapsedUserAccounts.map( u => sendBillTo(u.address) );
```

# lab

```js
let divide = () =>  2000 / 100
let square = x => x ** 2
let add = (a, b) => a + b
```

