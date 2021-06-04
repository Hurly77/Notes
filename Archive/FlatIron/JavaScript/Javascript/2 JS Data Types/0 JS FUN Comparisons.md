## equality operators

There are four equality operators built into JavaScript:

- **strict equality operator** (`===`)
- **strict inequality operator** (`!==`)
- **loose equality operator** (`==`)
- **loose inequality operator** (`!=`)

Even if the values on both sides of the operator look similar (e.g., `'42' === 42`), the `===` operator will only return `true` if the data types also match:

```js
42 === 42
// => true
 
42 === '42'
// => false
 
true === 1
// => false
 
'0' === false
// => false
 
null === undefined
// => false
 
' ' === 0
// => false
```

The **strict inequality operator** returns `true` if two values are *not* equal and does not perform type conversions:

```js
9000 !== 9001
// => true
 
9001 !== '9001'
// => true
 
[] !== ''
// => true
```

### Loose Equality Operator `==` and Loose Inequality Operator `!=`

```js
42 == 42
// => true
```

it will *also* return `true` if it can perform a type conversion (e.g., changing the string `'42'` into the number `42`) that makes the two values equal:

```js
42 == '42'
// => true
 
true == 1
// => true
 
'0' == false
// => true
 
null == undefined
// => true
 
' ' == 0
// => true
```

The **loose inequality operator** is the opposite of `==`. It returns `true` if two values are *not* equal, performing type conversions as necessary:

```js
9000 != 9001
// => true
 
9001 != '9001'
// => false
 
[] != ''
// => false
```

## Compare Numbers with Relational Operators

There are four relational operators built in to JavaScript:

- **greater than** (`>`),
- **greater than or equals** (`>=`)
- **less than** (`<`)
- **less than or equals** (`<=`)

These operators work in a very similar way to the equality operators:

```js
88 > 9// => true
```

However, beware of type conversion when comparing non-numbers against numbers. For instance, when a string is compared with a number, the JavaScript engine tries to convert the string to a number:

```js
88 > '9'
// => true
```

If the engine can't convert the string into a number, the comparison will always return `false`:

```js
88 >= 'hello'
// => false
 
88 <= 'hello'
// => false
```

The following returns `false` because the Unicode value of `8`, the first character in `88`, is less than the Unicode value of `9`.

```js
'88' > '9'
// => false
```

***Top Tip\***: Stick to comparing *numerical* values with the relational operators and you'll be golden.

```
if hungry 
make a meal 
else 
don't
```

**Writing code involves the same type of logic -- we only want an action to happen *if* a certain condition is met 

this is called **control flow** because, it helps *control* the *flow* of an application.

## What Constitutes an Expression in JavaScript

A JavaScript expression is **a unit of code that returns a value**. Primitive values are expressions because they resolve to a value:

```js
9;
// => 9
 
('Hello, world!');
// => "Hello, world!"
 
false;
// => false
```

So are arithmetic and string operations. This code resolves to the number `64`:

```js
8 * 8;
// => 64
```

his resolves to the string `"Hello, world!"`:

```js
Hello, ' + 'world!';
// => "Hello, world!"
```

Same for comparison and assignment operations. This comparison resolves to the boolean `true`:

```
2 > 1;
// => true
```

Variable declarations are **NOT** expressions...

```
let answer;
```

Variable assignments **ARE**, resolving to the assigned value (`42`, in this case):

```js
answer = 42;
// => 42
```

Finally, references to variables are also expressions, resolving to the value contained in the variable:

```js
const fullName = 'Ada Lovelace';
 
fullName;
// => "Ada Lovelace"
```

## Organize Code Using Block Statements

A *block statement* is a pair of curly braces (`{ }`) used to group JavaScript statements. It plays a role in conditional statements, loops, and functions.

```js
{
  ('This line is a JavaScript statement nested inside a block statement!');
 
  // This is also a statement nested inside a block:
  5 * 5 - 5;
 
  // And so are these:
  const weCan = 'group multiple statements';
 
  const suchAs = 'these variable declarations';
 
  const insideA = 'block statement.';
}
// => 20
```

**Note**: The statement above *implicitly* returns `20`, the value returned by `5 * 5 - 5`, when evaluated. The **implicit return is something unique to block statements** like the ones we use for `if...else` and loop statements. In the case of functions, we need to *explicitly* use the word `return` to tell JavaScript what we want the output to be (if we want one, at all).

## Describe the Difference Between Truthy and Falsy Values

If, upon conversion, the value becomes `true`, we say that it's a **truthy** value. If it becomes `false`, we say that it's **falsy**.

In JavaScript, the following values are **falsy**:

- `false`
- `null`
- `undefined`
- `0`
- `NaN`
- An empty string (````, `''`, `""`)

***Every other value is truthy\***.

we can pass it to the global `Boolean` object, which converts the value into its boolean equivalent:

```js
Boolean(false);
// => false
 
Boolean(null);
// => false
 
Boolean(undefined);
// => false
 
Boolean(0);
// => false
 
Boolean(NaN);
// => false
 
Boolean('');
// => false
 
Boolean(true);
// => true
 
Boolean(42);
// => true
 
Boolean('Hello, world!');
// => true
 
Boolean({ firstName: 'Brendan', lastName: 'Eich' });
```

**Note**: `document.all` is also falsy, but don't worry about too much -- it's an imperfect solution for legacy code compatibility.

## Use Conditional Statements

 three structures for implementing condition-based control flow: the `if...else` statement, `switch` statement, and ternary operator.

### `if` statement

`if` statements are the most common type of conditional, and they're pretty straightforward:

```js
if (condition) {
  // Block of code
}
//If the condition returns a truthy value, run the code inside the block:
const age = 30;
 
let isAdult;
 
if (age >= 18) {
  isAdult = true;
}
// => true
 
isAdult;
// => true

```

If the condition returns a **falsy** value, do nothing:

### `else`

If we want to run some code when the condition returns a `falsy` value, we can use an `else` clause:

```js
const age = 14;
 
let isAdult;
 
if (age >= 18) {
  isAdult = true;
} else {
  isAdult = false;
}
// => false
 
isAdult;
// => false
```

### Nested Conditionals

```js
const age = 17;
 
let isAdult, canWork, canEnlist, canDrink;
 
if (age >= 16) {
  canWork = true;
 
  if (age >= 18) {
    isAdult = true;
    canEnlist = true;
 
    if (age >= 21) {
      canDrink = true;
    }
  }
}
 
isAdult;
// => undefined
 
canWork;
// => true
 
canEnlist;
// => undefined
 
canDrink;
// => undefined
```

### `else if`

```js
const age = 20;
 
let isAdult, canWork, canEnlist, canDrink;
 
if (age >= 21) {
  isAdult = true;
  canWork = true;
  canEnlist = true;
  canDrink = true;
} else if (age >= 18) {
  isAdult = true;
  canWork = true;
  canEnlist = true;
} else if (age >= 16) {
  canWork = true;
}
// => true
 
isAdult;
// => true
 
canWork;
// => true

canEnlist;
// => true
 
canDrink;
// => undefined
```

###### **Note**

 that, unlike the nested `if` statements above, **at most one code block will run**. In the absence of an `else` statement, it's possible that none of the `if` conditions return a truthy value and **no block is run**. However, it's impossible for more than one block in a linked `if...else if...else` control flow to run.

## `switch`

Running multiple blocks in the same control flow is one of the many talents of the `switch` statement. The general structure is as follows:

```js
switch (expression) {
  case value1:
    // Statements
    break;
  case value2:
    // Statements
    break;
  default:
    // Statements
    break;
}
```

The JavaScript engine evaluates the expression and then compares the returned value against each of the case values using strict equality (===):

```js
const hunger = 'famished';
 
let food;
 
switch (hunger) {
  case 'light':
    food = 'grapes';
    break;
  case 'moderate':
    food = 'sushi';
    break;
  case 'famished':
    food = 'lasagna';
    break;
}
// => "lasagna"
 
food;
// => "lasagna"
```

he `switch` statement here doesn't require us to repeat the `if (order === _____)` line for each possibility:

```js
const order = 'cheeseburger';
 
let ingredients;
 
switch (order) {
  case 'cheeseburger':
    ingredients = 'bun, burger, cheese, lettuce, tomato, onion';
    break;
  case 'hamburger':
    ingredients = 'bun, burger, lettuce, tomato, onion';
    break;
  case 'malted':
    ingredients = 'milk, ice cream, malted milk powder';
    break;
  default:
    console.log("Sorry, that's not on the menu right now.");
    break;
}
```

If we'd like to write out the same code with an `if` conditional, it will look like this:

```js
const order = 'cheeseburger';
 
let ingredients;
if (order === 'cheeseburger') {
  ingredients = 'bun, burger, cheese, lettuce, tomato, onion';
} else if (order === 'hamburger') {
  ingredients = 'bun, burger, lettuce, tomato, onion';
} else if (order === 'malted') {
  ingredients = 'milk, ice cream, malted milk powder';
} else {
  console.log("Sorry, that's not on the menu right now.");
}
```

If it contains anything other than a number between `13` and `19`, none of our `case`s will hit, and it will end up at the `default`, which sets `isTeenager` to `false`:

```js
const age = 15;
 
let isTeenager;
 
switch (age) {
  case 13:
  case 14:
  case 15:
  case 16:
  case 17:
  case 18:
  case 19:
    isTeenager = true;
    break;
  default:
    isTeenager = false;
}
```

### `break`

 If instead, we wrote the following

```js
const age = 15;
 
let isTeenager;
 
switch (age) {
  case 13:
  case 14:
  case 15:
  case 16:
  case 17:
  case 18:
  case 19:
    isTeenager = true;
    console.log('case 19: ', isTeenager);
  default:
    isTeenager = false;
    console.log('default: ', isTeenager);
}
```

often see switch statements where `break` is used in every case as a way to ensure there is no unexpected behavior from multiple cases executing.

There's a neat little trick we can employ that allows us to use comparisons for `case` statements

```js
const age = 20;
 
let isAdult, canWork, canEnlist, canDrink;
 
switch (true) {
  case age >= 21:
    canDrink = true;
  case age >= 18:
    isAdult = true;
    canEnlist = true;
  case age >= 16:
    canWork = true;
}
// => true
 
isAdult;
// => true
 
canWork;
// => true
 
canEnlist;
// => true
 
canDrink;
// => undefined
```

### `default`

The `default` keyword specifies a set of statements to run after all of the `switch` statement's `case`s have been checked. The only time the `default` statements do *not* run is if the engine hits a `break` in one of the `case` statements.

```js
const mood = 'quizzical';
 
let response;
 
switch (mood) {
  case 'happy':
    response = 'Heck yea; be happy!';
  case 'sad':
    response = "You're awesome; keep your head up!";
  default:
    response = "Sorry, I don't know how to respond to that mood.";
}
// => "Sorry, I don't know how to respond to that mood."
 
response;
// => "Sorry, I don't know how to respond to that mood."
```

It's typically safer to include `break` statements because it helps avoid bugs like this:

```js
const mood = 'happy';
 
let response;
 
switch (mood) {
  case 'happy':
    response = 'Heck yea; be happy!';
  case 'sad':
    response = "You're awesome; keep your head up!";
  default:
    response = "Sorry, I don't know how to respond to that mood.";
}
// => "Sorry, I don't know how to respond to that mood."
 
response;
// => "Sorry, I don't know how to respond to that mood."
```

However, since we didn't `break` after that assignment, the `default` case *also* runs and reassigns `"Sorry, I don't know how to respond to that mood."` to `response`. Whoops!

### Ternary operator

he ternary operator, the final piece of the conditional puzzle, is a good way to represent an `if...else` statement in a single line of code:

```js
condition ? expression1 : expression2;
```

 the condition returns a truthy value, run the code in `expression1`. If the condition returns a falsy value, run the code in `expression2`.

```js
const age = 45;
let isAdult;
 
age >= 18 ? (isAdult = true) : (isAdult = false);
// => true
 
isAdult;
// => true
```

 the above example, we assign `isAdult` as `true` if the condition returns a truthy value and as `false` otherwise. We can simplify that a bit and assign the result of the ternary directly to a variable:

```js
const age = 60;
const isAdult = age >= 18 ? true : false;
 
isAdult;
// => true
```

If it helps you visualize what's going on, you can wrap the condition, the expressions, or the entire ternary in parentheses:

```js
const age = 17;
const isAdult = (age >= 18) ? true : false;
const canWork = age >= 16 ? (1 === 1) : (1 !== 1);
const canEnlist = (isAdult ? true : false);
 
isAdult;
// => false
 
canWork;
// => true
 
canEnlist;
// => false
```

**Top Tip:** Be careful to not overuse the ternary operator. It's fine for slimming down a simple `if...else`, but be conscious of how easy your code is to understand for an outsider. Remember, you generally write code once, but it gets read (by yourself and others) **far** more than once. The ternary is often more difficult to quickly interpret than a regular old `if...else`, so make sure the reduction in code is worth any potential reduction in readability.

## Conclusion