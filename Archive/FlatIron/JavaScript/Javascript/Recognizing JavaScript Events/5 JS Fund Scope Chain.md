## Scope Chain

For a function declared at the top level of our JavaScript file (that is, **not** declared inside of another function)its outer scope is the *global scope* That means it can access all of the variables and functions declared in the **global scope**

 When that new function is invoked, it creates a new scope and **retains a reference to the \*outer scope\* in which it was declared**. Inside the new function's body, in addition to variables and functions declared in that function, **we also have access to variables and functions declared in the outer scope**. Let's see that in action:

```js
const globalVar = 1;
 
function firstFunc () {
  const firstVar = 2;
 
  return firstVar + globalVar;
}
 
firstFunc();
// => 3
```

##  How to Create Nested Functions



![image-20200817200754701](C:\Users\camer\AppData\Roaming\Typora\typora-user-images\image-20200817200754701.png)

All variables and functions declared in outer scopes are available in inner scopes via the scope chain. This can go on forever, with functions nested in functions nested in functions, each new level creating a new scope that can reference functions and variables declared in its outer scopes through the scope chain:

```js
const globalVar = 1;
 
function firstFunc () {
  const firstVar = 2;
 
  function secondFunc () {
    const secondVar = 3;
 
    return secondVar + firstVar + globalVar;
  }
 
  const resultFromSecondFunc = secondFunc();
 
  return resultFromSecondFunc;
}
 
firstFunc();
// => 6
```

![image-20200817201043090](C:\Users\camer\AppData\Roaming\Typora\typora-user-images\image-20200817201043090.png)

***NOTE\***: The scope chain only goes in *one* direction. An outer scope **does not have access to things declared in an inner scope**.

```js
const fruit = 'Apple';
 
function first () {
  const vegetable = 'Broccoli';
 
  console.log('fruit:', fruit);
  console.log('vegetable:', vegetable);
  console.log('legume:', legume);
}
 
function second () {
  const legume = 'Peanut';
 
  console.log('fruit:', fruit);
  console.log('legume:', legume);
  console.log('vegetable:', vegetable);
}
```

## What Happens During the Execution Phase

#### The JavaScript Engine

We call those names *identifiers* because they allow us to **identify** the variable or function we're referring to.

When our JavaScript code is run in the browser, the **JavaScript** engine actually **makes** two **separate** **passes** over our code:

#### Compilation Phase

#### Compilation Phase

The first pass is the *compilation phase*, in which the engine steps through our code line-by-line:

1. When it reaches a variable declaration, the engine allocates memory and sets up a reference to the variable's identifier, e.g., `myVar`.
2. When the engine encounters a function declaration, it does three things:

```
- Allocates memory and sets up a reference to the function's identifier,
  e.g., `myFunc`.
 
- Creates a new execution context with a new scope.
 
- Adds a reference to the parent scope (the outer environment) to the scope
  chain, making variables and functions declared in the outer environment
  available in the new function's scope.
```

#### Execution Phase

The second pass is the *execution phase*. The JavaScript engine again steps through our code line-by-line, but this time it actually runs our code, assigning values to variables and invoking functions.

One of the engine's tasks is the process of matching identifiers to the corresponding values stored in memory. Let's walk through the following code:

```js
const myVar = 42;
 
myVar;
// => 42
```

During the compilation phase, a reference to the identifier `myVar` is stored in memory. The variable isn't yet assigned a value, and the second line (`myVar;`) is skipped over entirely because it isn't a declaration.

During the execution phase, the value `42` is assigned to `myVar`. When the engine reaches the second line, it sees the identifier `myVar` and resolves it to a value through a process known as *identifier resolution*. The engine first checks the current scope to see if `myVar` has been declared in it. If it finds no declaration for `myVar` in the current scope, the engine then starts moving up the scope chain, checking the parent scope and then the parent scope's parent scope and so on, until it finds a matching declared identifier or reaches the global scope. If the engine traverses all the way up to the global scope and still can't find a match, it will throw a `ReferenceError` and inform you that the identifier is not declared anywhere in the scope chain.

Let's look at an example. Remember, the engine will continue to move up the scope chain **only** if it can't find a matching identifier in the current scope. Because of this, we can actually use the same identifier to declare variables or functions in multiple scopes:

```js
const myVar = 42;
 
function myFunc () {
  const myVar = 9001;
 
  return myVar;
}
 
myFunc();
// => 9001
```

During the compilation phase, a reference to `myVar` is created in the global scope, and a reference to a **different** `myVar` is created in `myFunc()`'s scope. The global `myVar` exists in the scope chain for `myFunc()`, but the engine never makes it that far. The engine finds a matching reference within `myFunc()`, and it resolves the `myVar` identifier to `9001` without having to traverse up the scope chain.

# lab

```js
const animal = ""

function myAnimal() {
  let animal = 'dog';

  return animal
}

function yourAnimal() {
  let animal = 'cat';
  // How can we make sure that this function
  // and the above function both pass?
  // P.S.: You can't just hard-code 'cat' below
  return animal
}

function add2(n) {
  let two = 2;
  
  return n + two
}
```

