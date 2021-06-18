### `map()` With `Array`s

We use `map()` when we want to perform an action on each element in the collection, and "gather" the results into a new `Array`.

We'll write `map()` **four times** so you can see how the iterator functions allow us to write more *expressive* code.

### `for` and `for...of`

```js
let students = ["harry", "ron", "hermione", "ginevra"];
let rollCall = [];
 
for (const student of students) {
  rollCall.push( student + " the wizard" );
}
 
//=> rollCall = ["harry the wizard", "ron the wizard", "hermione the wizard", "ginevra the wizard"];
```

### `map()` With a Function Declaration

```js
function studentRollCall(student) {
  return student + " the wizard";
}
 
let students = ["harry", "ron", "hermione", "ginevra"];
let rollCall = students.map(studentRollCall);
//=> rollCall = ["harry the wizard", "ron the wizard", "hermione the wizard", "ginevra the wizard"];
```

### `map()` With a Function Expression

```
let students = ["harry", "ron", "hermione", "ginevra"];
let rollCall = students.map(function(student) {
  return student + " the wizard";
});
//=> rollCall = ["harry the wizard", "ron the wizard", "hermione the wizard", "ginevra the wizard"];
```

### `map()` With an Arrow Function

```js
// When the parameter list is only one element, we can drop () !
let students = ["harry", "ron", "hermione", "ginevra"];
let rollCall = students.map(student => student + " the wizard")
//=> rollCall = ["harry the wizard", "ron the wizard", "hermione the wizard", "ginevra the wizard"];
```

## Demonstrate `map()` With Complex Data Structures

```js
const robots = [
  { name: 'Johnny 5', modes: 5, isActivated: false, },
  { name: 'C3PO', modes: 3, isActivated: false, },
  { name: 'Sonny', modes: 2.5, isActivated: false, },
  { name: 'Baymax', modes: 1.5, isActivated: false, },
];
 
const activatedRobots = robots.map(function (robot) {
  return Object.assign({}, robot, {
    modes: robot.modes * 2,
    isActivated: true,
  });
});
 
console.log(activatedRobots);
 
/*
 Result:
 [
   { name: 'Johnny 5', modes: 10, isActivated: true },
   { name: 'C3PO', modes: 6, isActivated: true },
   { name: 'Sonny', modes: 5, isActivated: true },
   { name: 'Baymax', modes: 3, isActivated: true }
 ]
*/
```

```js
const tutorials = [
  'what does the this keyword mean?',
  'What is the Contutorialuctor OO pattern?',
  'implementing Blockchain Web API',
  'The Test Driven Development Workflow',
  'What is NaN and how Can we Check for it',
  'What is the difference between stopPropagation and preventDefault?',
  'Immutable State and Pure Functions',
  'what is the difference between == and ===?',
  'what is the difference between event capturing and bubbling?',
  'what is JSONP?'
];
```

