## Function Using Function Declaration

```js
function razzle(lawyer="Billy", target="'em") {
  console.log(`${lawyer} razzle-dazzles ${target}!`);
}
razzle() //=> Billy razzle-dazzles 'em!
razzle("Methuselah", "T'challah") //=> Methuselah razzle-dazzles T'challah!
```



## Function Using a Function Expression

```js
let fn = function() {
  console.log("Yet more razzling")
} //=> undefined
fn //=> ƒ () { console.log("Yet more razzling") }
```

## Term "Anonymous Function"

`function(){....}` an "***anonymous function\***." In the previous example the following was the anonymous function:

```js
function() {
  console.log("Yet more razzling")
}
//anonymous function, there is no name infront of ()
```

## Define an IIFE: Instantly-Invoked Function Expression

**I**nstantly-**I**nvoked **F**unction **E**xpression

```js
(function(baseNumber){ return baseNumber + 3 })(2) //=> ???
```

Interestingly, any variables, functions, `Array`s, etc. that are defined *inside* of the function expression's body *can't* be seen *outside* of the IIFE.

> **(ADVANCED) SYNTAX QUESTION** Some keen-eyed readers might ask, "Why add parentheses around the function expression?" Why not:
>
> ```js
> function(x){ return x + 2 }(2) //=> ERROR
> ```
>
> instead of:
>
> ```js
> (function(x){ return x + 2 })(2) //=> 4
> ```
>
> The reason is that JavaScript gets confused by all those bits of special symbols and operators sitting right next to each other. 

##  *Term* "Function-level Scope"

JavaScript exhibits "Function-level" scope. This means that if a function is defined *inside another* function, the inner function has access to all the parameters (variables passed in) of, as well as any variables defined in, the outer function.

**ASIDE**: This is where people **really** start to get awed by JavaScript.

Consider this code:

```js
function outer(greeting, msg="It's a fine day to learn") { // 2
  let innerFunction =  function(name, lang="Python") { // 3
    return `${greeting}, ${name}! ${msg} ${lang}` // 4
  }
  return innerFunction("student", "JavaScript") // 5
}
 
outer("Hello") // 1
//=> "Hello, student! It's a fine day to learn JavaScript"
```

1. We call `outer`, passing `"Hello"` as an argument
2. The argument (`"Hello"`) is saved in `outer`'s `greeting` parameter. The other parameter, `msg` is set to a default value
3. Here's our old friend the function expression. It expects two arguments which it stores in the parameters `name` and `lang` with `lang` being set as a default to `"Python"`. This expression is saved in the local variable `innerFunction`
4. Inside `innerFunction` we make use of both the parameters `name` and `lang` ***as well as\*** the parameters of innerFunction's containing (parent) function. `innerFunction` gets access to those variables
5. Inside `outer`, we invoke `innerFunction`

This can also work like this:

```js
function outer(greeting, msg="It's a fine day to learn") { // 2
  let innerFunction =  function(name, lang="Python") { // 3
    return `${greeting}, ${name}! ${msg} ${lang}` // 4
  }
  return innerFunction
}
 
outer("Hello")("student", "JavaScript") // 1, 5
//=> "Hello, student! It's a fine day to learn JavaScript"
```

 Even if the inner function `innerFunction` is invoked **outside** the parent function, it ***still\*** has access to those parent function's variables.

 instead of assigning the function expression to `innerFunction`, let's just return the function expression.

```js
function outer(greeting, msg="It's a fine day to learn") {
  return function(name, lang="Python") {
    return `${greeting}, ${name}! ${msg} ${lang}`
  }
}
 
outer("Hello")("student", "JavaScript")
//=> "Hello, student! It's a fine day to learn JavaScript"
```

It was almost as if `outer` returned:

```js
return function(name, lang="Python") { // The "inner" function
  return `Hello, ${name}! It's a fine day to learn ${lang}`

```

We could:

```js
let storedFunction = outer("Hello")
storedFunction("student", "JavaScript")
//=> "Hello, student! It's a fine day to learn JavaScript"
```

##  *Term* "Closure"

In the previous example, we could call the "inner" function, the **anonymous function** a "closure." It "encloses" the function-level scope of its parent. And, like a backpack, it can carry out the knowledge that it saw - *even when you're out of the parent's scope*.

Recall the IIFE discussion, since what's inside an IIFE can't be seen, if we wanted to let just tiny bits of information leak back out, we might want to pass that information back out, through a closure.

*Note: We're also using destructuring assignment, don't forget your **ES6 tools**!*

>  **ES6 tools** - JavaScript **ES6** brings new syntax and new awesome features to make your code more modern and more readable. It allows you to write less code and do more. **ES6** introduces us to many great features like arrow functions, template strings, class destruction, Modules… and more
>
> The **destructuring assignment** syntax is a JavaScript expression that makes it possible to unpack values from arrays, or properties from objects, into distinct variables.
>
> ```js
> let a, b, rest;
> [a, b] = [10, 20];
> 
> console.log(a);
> // expected output: 10
> 
> console.log(b);
> // expected output: 20
> 
> [a, b, ...rest] = [10, 20, 30, 40, 50];
> 
> console.log(rest);
> // expected output: Array [30,40,50]
> 
> ```



```js
// the defined vars become the return values of the function body
let [answer, theBase] = (
  function(thingToAdd) {
    let base = 3
    return [
      function() { return base + thingToAdd },
      function() { return base }
    ]
  }
)(2)
answer() //=> 5
console.log(`The base was ${theBase()}`)
// OUTPUT: The base was 3
```

## *Term* "Scope Chain"

The mechanism behind all the cool stuff we just saw is the *scope chain* which allows functions defined in functions to access all their parent scopes' variables. Here's a simple example:

```js
function demoChain(name) {
  let part1 = 'hi'
  return function() {
    let part2 = 'there'
    return function() { // Innermost
      console.log(`${part1.toUpperCase()} ${part2} ${name}`);
    }
  }
}
 
demoChain("Dr. Stephen Strange")()() //=> HI there Dr. Stephen Strange
```

Through the *scope chain*, the function labeled `//Innermost` has access to `name`, `part1`, and `part2` when it is called and runs the `console.log()` statement. That's awesome wormhole, space-time, magic!