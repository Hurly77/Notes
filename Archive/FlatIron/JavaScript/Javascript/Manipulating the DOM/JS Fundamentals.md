**Document**

you can use `document.querySelector('header').remove()` to remove elements in the console

## **JS Fundamentals: Variables**

- Start every variable name with a lowercase letter. **Variable names starting with a number are not valid**.
- Don't use spaces — `camelCaseYourVariableNames` (see the camel humps?) instead of `snake_casing_them` (like the underscore is a snake that swallowed the words).
- Don't use JavaScript [reserved words](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Reserved_keywords_as_of_ECMAScript_2015) or [future reserved words](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Future_reserved_keywords).

It's important to note that case matters, so `javaScript`, `javascript`, `JavaScript`, and `JAVASCRIPT` are four different variables.

#### Creating variables

```javascript
var pi;

pi = 3.14

var pi = 3.14

pi 
//=> 3.14
```

#### multi line Variables

To condense variable naming

```javascript
//change this
var a = 5;
var b = 2;
var c = 3;
var d = {};
var e = [];
//to this
var a = 5,
    b = 2,
    c = 3,
    d = {},
    e = [];
//which can be converted to:
var a = 5, b = 2, c = 3, d = {}, e = [];
```

#### Retrieving Variables

Changing the value of a variable in JavaScript

```javascript
var pi = 3.14159;
pi;
//=> 3.14159
pi = 3.14;
pi;
//=> 3.14;
```

### Variable Values

use the `typeof` operator to check the data type of the value currently stored in a variable:

```javascript
var language;
//=> undefined
 
typeof language;
//=> "undefined"
 
language = "JavaScript";
//=> "JavaScript"
 
typeof language;
//=> "string"
```

***Top Tip\***: When writing JavaScript code, it's good practice to ***never\*** set a variable equal to `undefined`. Variables will be `undefined` until we explicitly assign a value, so encountering an `undefined` variable is a strong signal that the variable was declared but not assigned prior to the reference. That's valuable information that we can use while debugging, and it comes at no additional cost to us.

## Identify When to Use `const`, `let`, and `var` for Declaring Variables

Because of its **ubiquity** there is almost **no reason** to use `var` with the features JavaScript has **post-2015**. `var` **comes with** a ton of baggage in the form of **scope issues**.

#### `let`

ES2015 introduced two new ways to create variables: `let` and `const`.  Both throw an error if you try to declare the same variable a second time:

```javascript
let pi = 3.14159;
//=> undefined
 
let pi = "the ratio between a circle's circumference and diameter";
//=> Uncaught SyntaxError: Identifier 'pi' has already been declared
```

**note:** in some browsers the console won't throw this error if you try the above code there. However, you will get the error when developing your app.

Just like with `var`, we can still reassign a variable declared with `let`:

```javascript
let pi = 3.14159;
//=> undefined
 
pi = "the ratio between a circle's circumference and diameter";
//=> "the ratio between a circle's circumference and diameter"
 
typeof pi;
//=> "string"
```

1. When we assign a primitive value (any type of data *except* an object) to a variable declared with `const`, we know that variable will *always* contain the same value.
2. When we assign an object to a variable declared with `const`, we know that variable will *always* point to the same object (though the object's properties can still be modified — more on this in the lesson about objects in JavaScript).
3. When another developer looks at our code and sees a `const` declaration, they immediately know that variable points to the same object or has the same value every other time it's referenced in the program. For variables declared with `let` or `var`, the developer cannot be so sure and will have to keep track of how those variables change throughout the program. The extra information provided by `const` is valuable, and it comes at no extra cost to you! Just use `const` as much as possible and reap the benefits.

```javascript
const pi = 3.14159;
//=> undefined
 
pi = 2.71828;
//=> Uncaught TypeError: Assignment to constant variable.
```

***NOTE\***: With `let`, it's possible to declare a variable without assigning a value, just like `var`:

```javascript
let pi;
//=> undefined
 
pi = 3.14159;
//=> 3.14159
```

However, because `const` doesn't allow reassignment after the variable is initialized, we **must** assign a value right away:

```javascript
const pi;
//=> Uncaught SyntaxError: Missing initializer in const declaration
 
const pi = 3.14159;
//=> undefined
```

##### **Good Rule of Thumb:**

- ***Use `var`...\*** never.
- ***Use `let`...\*** when you know the value of a variable will change. For example, a `counter` variable that starts at `0` and is subsequently incremented to `1`, `2`, `3`, and so on. In the lessons on looping and iteration in JavaScript, `let` will have its moment in the spotlight.
- ***Use `const`...\*** for *every* other variable.