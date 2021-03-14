JavaScript represents everything as a double-precision floating-point number, or *float*. This imposes some interesting technical limitations on the precision of the arithmetic we can perform with JavaScript. For example:

```js
0.1 * 0.1;
//=> 0.010000000000000002
 
0.1 + 0.1 + 0.1;
//=> 0.30000000000000004
 
1 - 0.9;
//=> 0.09999999999999998
```

The problem that the above code snippet highlights is that it's really easy to represent something like `1/10` in decimal (`0.1`) but impossible to do it in binary (`0.0001100110011...`). It's the exact same problem that the decimal system has in trying to represent `1/3` as `0.33333333333...`.

## Employ Operators to Perform Arithmetic and Assign Values to Variables

### Arithmetic Operators

+
We've used the addition operator to concatenate strings, but it's also used to add numbers together:

```js
40 + 2;
//=> 42
```

#### `-`

The subtraction operator returns the difference between two numbers:

```js
9001 - 9000;
//=> 1
```

#### `*`

The multiplication operator returns the product of two numbers:

```js
6 * 7;
//=> 42
```

#### `/`

The division operator returns the result of the left number divided by the right number:

```js
9001 / 42;
//=> 214.3095238095238
```

#### `%`

The remainder operator returns the remainder when the left number is divided by the right number:

```js
9001 % 42;
//=> 13
```

#### `**`

The exponentiation operator returns the left number raised to the power of the right number:

```js
2 ** 8;
//=> 256
```

### Order of Operations

JavaScript evaluates compound arithmetic operations by following the standard order of operations used in basic math. Anything in parentheses has highest priority; exponentiation is second; then multiplication, division, and remainder; and, finally, addition and subtraction, in order from left to right. This is how the JavaScript compiler works. You can learn more about this in the **resources** section at the end of the lesson. For example:

```js
2 - (2 % 2) + (2 / 2 ** 2) * 2;
//=> 3
 
2 - ((2 % (2 + 2)) / 2 ** 2) * 2;
//=> 1
```

### Incrementing and Decrementing

#### `++`

The `++` operator increments the stored number by `1`. If the `++` operator comes after the variable (e.g., `counter++`), the variable's value is *returned first and then incremented*:

```js
let counter = 0;
//=> undefined
 
counter++;
//=> 0
 
counter;
//=> 1
```

If the `++` operator comes before the variable (e.g., `++counter`), the variable's value is *incremented first and then returned*:

In both cases, `counter` contains the value `1` after incrementing. The difference is in whether we want the operation to return the original or incremented value.

#### `--`

The `--` operator decrements the stored number by `1` and has the same pair of prefix and postfix options as the `++` operator:

```js
let counter = 0;
//=> undefined
 
// Return the current value of 'counter' and then decrement by 1
counter--;
//=> 0
 
// Check the new value of 'counter'
counter;
//=> -1
 
// Decrement 'counter' and then return the new value
--counter;
//=> -2
 
// Check the new value of 'counter'
counter;
//=> -2

```

## Assignment operators

JavaScript has a number of operators for assigning a value to a variable. We've already used the most basic, `=`, but we can also couple it with an arithmetic operator to perform an operation *and* assign the value of the operation:

```js
let counter = 0;
//=> undefined
 
counter += 10;
//=> 10
 
counter -= 2;
//=> 8
 
counter *= 4;
//=> 32
 
counter /= 2;
//=> 16
 
counter %= 6;
//=> 4
 
counter **= 3;
//=> 64
```

## Explain What `NaN` Is

NaN or not a number is when we try to calculate the incalculable.

```js
counter++;
//=> NaN
```

The JavaScript engine can't add `1` to `undefined`, so it tells us the result is **Not a Number** — `NaN`.

***Top Tip\***: Much like `undefined`, you should never assign `NaN` as the value of a variable and instead let it be a signal that some weird maths are happening in your code.

## Use built-in objects like `Math` and `Number` to accomplish complex tasks

## Use built-in objects like `Math` and `Number` to accomplish complex tasks

#### `Number.isInteger()`

Checks whether the provided argument is an integer:

```js
Number.isInteger(42);
//=> true
 
Number.isInteger(0.42);
//=> false
```

#### `Number.isFinite()`

```js
Number.isFinite(9001);
//=> true
 
Number.isFinite(Infinity);
//=> false
```

#### `Number.isNaN()`

Checks whether the provided argument is `NaN`:

```js
Number.isNaN(10);
//=> false
 
Number.isNaN(undefined);
//=> false
 
Number.isNaN(NaN);
//=> true

```

#### `Number.parseInt()`

Accepts a string as its first argument and parses it as an integer. The second argument is the base that should be used in parsing (e.g., `2` for binary or `10` for decimal). For example, `100` is `100` in decimal but `4` in binary:

```js
Number.parseInt('100', 10);
//=> 100
 
Number.parseInt('100', 2);
//=> 4
```

#### `Number.parseFloat()`

`Number.parseFloat()` only accepts a single argument, the string that should be parsed into a floating-point number:

```js
Number.parseFloat('3.14159');
//=> 3.14159
```

### `Math`

The `Math` object contains some properties representing common mathematical values, such as `Math.PI` and `Math.E`, as well as a number of methods for performing useful calculations.

#### `Math.ceil()` / `Math.floor()` / `Math.round()`

JavaScript provides three methods for rounding numbers. `Math.ceil()` rounds the number *up*, `Math.floor()` rounds the number *down*, and `Math.round()` rounds the number either up or down, whichever is nearest:

```js
Math.ceil(0.5);
//=> 1
 
Math.floor(0.5);
//=> 0
 
Math.round(0.5);
//=> 1
 
Math.round(0.49);
//=> 0
```

#### `Math.max()` / `Math.min()`

These two methods accept a number of arguments and return the lowest and highest constituent, respectively:

```js
Math.max(1, 2, 3, 4, 5);
//=> 5
 
Math.min(1, 2, 3, 4, 5);
//=> 1
```

#### `Math.random()`

This method generates a random number between `0` (inclusive) and `1` (exclusive):

```js
Math.random();
//=> 0.4495507082209371
```

to generate a random integer between `1` and `10`:

```js
Math.floor(Math.random() * 10) + 1;
//=> 8
 
Math.floor(Math.random() * 10) + 1;
//=> 1
 
Math.floor(Math.random() * 10) + 1;
//=> 6
```

`Math.random()` returns a number between `0` and `0.999...`, which we multiply by `10` to give us a number between `0` and `9.999...`. We then pass that number to `Math.floor()`, which returns an integer between `0` and `9`. That's one less than the desired range (`1` to `10`), so we add one at the end of the equation. Try it out in the JS console!