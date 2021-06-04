## What Is a Data Type?

***Everything in JavaScript is data\*** except:

1. **Operators**: `+`, `!`, `<=`, etc.
2. **Reserved words**: `function`, `for`, `debugger`, etc.

*very piece of data falls into one of JavaScript's seven data types*: **numbers**, **strings**, **booleans**, **symbols**, **objects**, `null`, and `undefined`.

**CAREFUL** `typeof` is an operator, just like `+` or `!`. We get used to operators being only one character, but JavaScript (and many other languages) have operators with ***more than one\*** character. As such, **we don't need parentheses with `typeof`**. That said, JavaScript also supports `()` after `typeof`, but it's commonly not done.

### Numbers

JavaScript only has a single, all-encompassing number type:

```js
typeof 42;
//=> "number"
 
typeof 3.141592653589793;
//=> "number"
 
typeof 5e-324;
//=> "number"
 
typeof -Infinity;
//=> "number"
```

> **Think About This:** As JavaScript has become a language for the back-end as well as the front-end, it's imprecision around numbers keep it from entering many banking or engineering applications where precision is vital.

### Strings

Strings are how we represent text in JavaScript. A string consists of a matching pair of `'single quotes'`, `"double quotes"`, or ``backticks`` with zero or more characters in between:

```js
typeof 'I am a string.';
//=> "string"
 
typeof "Me too!";
//=> "string"
 
typeof `Me three!`;
//=> "string"

typeof '';
//=> "string"
```

##### Booleans

A **boolean** can only be one of two possible values: true or false. Booleans play a big role in if statements and looping in JavaScript.

```js

typeof true;
//=> "boolean"
 
typeof false;
//=> "boolean"
```

### Objects

JavaScript objects are a collection of properties bound by curly braces ({ }), similar to a hash in Ruby or a dictionary in Python.

The properties can point to values of any data type — even other objects:

```js
{
  "name": "JavaScript",
  "createdBy": {
    "firstName": "Brendan",
    "lastName": "Eich"
  },
  "firstReleased": 1995,
  "isAwesome": true
}
 
typeof {}
//=> "object"
```

From JavaScript's perspective, what we call "arrays" are just special cases of an object where the keys are all numbers. So while JavaScript has arrays like `let dogs = ["Byron", "Cubby", "Boo Radley", "Luca"]`, JavaScript really thinks that `typeof dogs` is `"object"`.

```js
let dogs = ['Byron', 'Cubby', 'Boo Radley', 'Luca'];
typeof dogs;
//=> "object"
```

### `null`

The `null` data type represents an intentionally absent object. For example, if a piece of code returns an object when it successfully executes, we could have it return `null` in the event of an error. Confusingly, the `typeof` operator returns `"object"` when called with `null`:

```js
typeof null;
//=> "object"
```

### `undefined`

The bane of many JS developers, `undefined` is a bit of a misnomer. Instead of 'not defined,' it actually means something more like 'not yet assigned a value.'

```js
typeof undefined;
//=> "undefined"
 
let unassignedVariable;
typeof unassignedVariable;
//=> "undefined"
 
unassignedVariable = '';
typeof unassignedVariable;
//=> "string"
```

### Symbols

Symbols are a relatively new data type (introduced in ES2015) that's primarily used as an alternative way to add properties to objects. Don't worry about symbols for now.

### Primitive Types

Six of the seven JavaScript data types — everything except object — are **primitive**. All this means is that they represent *single* values, such as `7`

## How Different JavaScript Data Types Interact

Every programming language has its own rules governing the ways in which we can operate on data of a given type. For example, it's rather uncontroversial that numbers can be subtracted from other numbers...

```js
3 - 2;
//=> 1

...and that strings can be added to other strings:

'Hello' + ", " + `world!`;
//=> "Hello, world!"
```

Some programming languages, such as Python, are strict about how data of different types can interact, and they will refuse to compile a program that blends types. Well, that's rather strict.

Other languages, such as Ruby, will attempt to handle the interaction by converting one of the data types so all data is of the same type. For example, instead of throwing an error when an integer (`3`) is added to a floating-point number (`0.14159`), Ruby will simply convert the integer into a floating-point number and correctly calculate the sum:

```ruby
3 + 0.14159
#=> 3.14159
```

Ruby throws errors when some stranger cases come up:

```js
> "THX-" + 1138
TypeError: no implicit conversion of Fixnum into String
```

That seems like a good baseline. However, JavaScript is a little *too* nice when handling conflicting data types. **No matter what weird combination of types you give it, JavaScript won't throw an error and will return \*something\* (though that \*something\* might make no sense at all).**

```js
'High ' + 5 + '!';
//=> "High 5!"
```

...to the [comical](https://www.destroyallsoftware.com/talks/wat):

```js
null ** 2; // null to the power of 2
//=> 0
 
undefined ** null; // undefined to the power of null
//=> 1
 
{}+{}; // empty object plus empty object
//=> "[object Object][object Object]" <-- That's a string!
```

```js
1 + 2 + 3 + 4 + 5;
//=> 15
 
'1' + 2 + 3 + 4 + 5;
//=> "12345"
 
1 + '2' + 3 + 4 + 5;
//=> "12345"
 
1 + 2 + '3' + 4 + 5;
//=> "3345"
 
1 + 2 + 3 + '4' + 5;
//=> "645"
 
1 + 2 + 3 + 4 + '5';
//=> "105"

//Anything plus a string returns a string
```

