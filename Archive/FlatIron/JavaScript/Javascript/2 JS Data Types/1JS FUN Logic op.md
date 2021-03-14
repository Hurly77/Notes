##  How to Convert an Expression to a Boolean Using `!!`

### `!!`

```js
const truthyValue = 'This value is truthy.';
 
!truthyValue;
// => false
 
!!truthyValue;
// => true
```

## Define the `&&` and `||` Operators

### `&&` AND

The logical AND (`&&`) operator takes two expressions:

```js
Define the && and || Operators
&& AND
The logical AND (&&) operator takes two expressions:
```

The return value of the `&&` operator is always **one of the two expressions**. If the first expression is falsy, `&&` returns the value of the first expression. If the first expression is truthy, `&&` returns the value of the second expression.

Again, if the first expression is falsy, `&&` returns that value and exits *without ever checking the second expression*:

```js
false && 'Anything';
// => false
 
// 4 * 0 returns 0, which is falsy
4 * 0 && 'Anything';
// => 0
```

If the first expression is truthy, `&&` then returns whatever the second expression evaluates to:

```js
true && false;
// => false
 
1 + 1 && 'Whatever';
// => "Whatever"
 
'The truthiest of truthy strings' && 9 * 9;
// => 81
```

1. If the left-side expression is falsy, the right-side expression doesn't matter at all. The `&&` operator returns the left side's falsy value and finishes.
2. If the left-side expression is truthy, the `&&` operator returns the right side's value (whether it's truthy or falsy) and finishes. If you're feeling a little confused, that's ok. This is one of those concepts that's a a bit hard to understand unless you've played around with it in code, so make sure you're testing all of these new operators out in your browser's JavaScript console to get a feel for how they work.

### `||` OR

The logical OR (`||`) operator also takes two expressions:

```js
expression1 || expression2;|| OR
The logical OR (||) operator also takes two expressions:

expression1 || expression2;
```

The return value of the `||` operator is always **one of the two expressions**. If the first expression is truthy, `||` returns the value of the first expression. If the first expression is falsy, `||` returns the value of the second expression.

If the first expression is truthy, that value is immediately returned and the second expression is never evaluated:

```js
true || 'Whatever';
// => true
 
1 + 1 || 'Whatever';
// => 2
```

If the first expression is falsy, `||` returns whatever the second expression evaluates to:

```js
false || 'Whatever';
// => "Whatever"
 
1 === 2 || 8 * 8;
// => 64
 
'' || 'Not ' + 'an ' + 'empty ' + 'string';
// => "Not an empty string"
```

There are three different ways the `||` operator can be evaluated:

| eft side | Right side     | Return value | Truthiness of return value |
| -------- | -------------- | ------------ | -------------------------- |
| Truthy   | Doesn't matter | Left side    | Truthy                     |
| Falsy    | Truthy         | Right side   | Truthy                     |
| Falsy    | Falsy          | Right side   | Falsy                      |

1. If the left-side expression is truthy, the right-side expression doesn't matter at all. The `||` operator returns the left side's truthy value and completes.
2. If the left-side expression is falsy, the `||` operator returns the right side's value (regardless of whether it's truthy or falsy) and completes. As with the `&&` operator, make sure you're testing out all of these outcomes in your browser's JS console!

## Describe How to Link Conditions Using the `&&` and `||` Operators

The `&&` and `||` operators are often used to link multiple conditions in a conditional statement:

```js
let friendCount = 3;
let message, messageColor;
 
if (user && friendCount) {
  message = `Hi ${user}! You have ${friendCount} ${
    friendCount === 1 ? 'friend' : 'friends'
  }!`;
} else if (user) {
  message = 'Link up with your friends to get the most out of Flatbook!';
} else {
  message = 'Please sign in.';
}
// => "Hi Charles Babbage! You have 3 friends!"
if (
  message === 'Please sign in.' ||
  message === 'Link up with your friends to get the most out of Flatbook!'
) {
  messageColor = 'red';
} else if (friendCount >= 1 && friendCount <= 10) {
  messageColor = 'blue';
} else if (friendCount >= 11 && friendCount <= 50) {
  messageColor = 'purple';
} else {
  messageColor = 'rainbow';
}
// => "blue"
```

