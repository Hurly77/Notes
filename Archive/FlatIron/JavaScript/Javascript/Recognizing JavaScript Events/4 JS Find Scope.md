## How Scope Works in JavaScript

*Scope* is the concept of **where something is available**.

In JavaScript, scope is defined by where declared variables and methods are available within our code

## Different Types of Scope

#### Global Scope

 *global scope* — are accessible everywhere in your JavaScript code.

![image-20200817194729706](C:\Users\camer\AppData\Roaming\Typora\typora-user-images\image-20200817194729706.png)

 what that looks like in JavaScript:

```js
// 'myFunc' is declared in the global scope and available everywhere in your code:
function myFunc () {
  return 42;
}
// => undefined
 
// 'myVar' is able to reference and invoke 'myFunc' because both are declared in the same scope (the global execution context):
const myVar = myFunc() * 2;
// => undefined
 
myVar;
// => 84
```

***Top Tip\***: If a variable or function is **not** declared inside a function or block, it's in the global execution context.

#### Function Scope

The **function body**, isn't in the global execution context. **function** **creates** a new execution context with its **own scope.**

```js
function myFunc () {
  const myVar = 42;
 
  return myVar * 2;
}
// => undefined
 
myFunc();
// => 84c

//However, from outside the function, we can't reference anything declared inside of it:

function myFunc () {
  const myVar = 42;
}
// => undefined
 
myVar * 2;
// Uncaught ReferenceError: myVar is not defined
```

#### Block Scope

A **block statement** also creates its **own** scope.

Variables declared with `var` are **not** block-scoped:

```js
if (true) {
  var myVar = 42;
}
 
myVar;
// => 42
```

However, variables declared with `const` and `let` **are** block-scoped:

```js
if (true) {
  const myVar = 42;
 
  let myOtherVar = 9001;
}
 
myVar;
// Uncaught ReferenceError: myVar is not defined
 
myOtherVar;
// Uncaught ReferenceError: myOtherVar is not defined
```

**never use `var`**

## Best Practices in Defining Scope

Variables created without a `const`, `let`, or `var` keyword are **always globally-scoped**, regardless of where they sit in your code.

```js
if (true) {
  lastName = 'Lovelace';
}
 
lastName;
// => "Lovelace"
```

If you create one inside of a function, it's still available globally:

```js
function bankAccount () {
  secretPassword = 'il0v3pupp135';
 
  return 'bankAccount() function invoked!';
}
 
bankAccount();
// => "bankAccount() function invoked!"
 
secretPassword;
// => "il0v3pupp135"

```

