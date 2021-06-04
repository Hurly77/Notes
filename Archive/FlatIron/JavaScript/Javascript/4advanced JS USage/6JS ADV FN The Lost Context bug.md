## Scenario



Print Greeting for Odin; The `Object` looks like this:

```js
let configuration = {
    frontContent: "Happy Birthday, Odin One-Eye!",
    insideContent: "From Asgard to Nifelheim, you're the best all-father ever.\n\nLove,",
    closing: {
        "Thor": "Admiration, respect, and love",
        "Loki": "Your son"
    },
    signatories: [
        "Thor",
        "Loki"
    ]
}
```

To display this:

```Js
let printCard = function() {
    console.log(this.frontContent)
    console.log(this.insideContent)
 
    this.signatories.forEach(function(signatory){
        let message = `${this.closing[signatory]}, ${signatory}`
        console.log(message)
    })
}
 
printCard.call(configuration)
```

This doesn't work as planned **an error like the following:**

```shell
Happy Birthday, Odin One-Eye!
From Asgard to Nifelheim, you're the best all-father ever.
Love,
/Users/heimdall/git_checkouts/fi/jscontext/unnamed/card.js:20
        let message = `${this.closing[signatory]}, ${signatory}`
                                     ^
TypeError: Cannot read property 'Thor' of undefined
    at /Users/heimdall/git_checkouts/fi/jscontext/unnamed/card.js:20:38
    at Array.forEach (<anonymous>)
    at Object.printCard (/Users/heimdall/git_checkouts/fi/jscontext/unnamed/card.js:19:22)
    at Object.<anonymous> (/Users/heimdall/git_checkouts/fi/jscontext/unnamed/card.js:25:11)
    at Module._compile (internal/modules/cjs/loader.js:799:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:810:10)
    at Module.load (internal/modules/cjs/loader.js:666:32)
    at tryModuleLoad (internal/modules/cjs/loader.js:606:12)
    at Function.Module._load (internal/modules/cjs/loader.js:598:3)
    at Function.Module.runMain (internal/modules/cjs/loader.js:862:12)
```

 quick debug shows that there **very much** is a property called `"Thor"` in `configuration.closing`

```js
console.log(configuration.closing.Thor) //=> "Admiration, respect, and love"
```

a bug created in the shadow of the all-too-easy-to-forget fact that function expressions and declarations ***inside\*** of other functions ***do not automatically\*** use the same context as the outer function. Think about the rules of implicit context assignment before reading on.

## Debugging: Discovering the Nature of the Lost Context Bug

let's add some `console.log()` calls so we can see what `this` is.

```js
let configuration = {
    frontContent: "Happy Birthday, Odin One-Eye!",
    insideContent: "From Asgard to Nifelheim, you're the best all-father ever.\n\nLove,",
    closing: {
        "Thor": "Admiration, respect, and love",
        "Loki": "Your son"
    },
    signatories: [
        "Thor",
        "Loki"
    ]
}
 
let printCard = function() {
    console.log(this.frontContent)
    console.log(this.insideContent)
 
    console.log("Debug Before forEach: " + this)
    this.signatories.forEach(function(signatory){
        console.log("Debug Inside: " + this)
        // let message = `${this.closing[signatory]}, ${signatory}`
        console.log(message)
    })
}
 
printCard.call(configuration)
```

Produces:

```
Happy Birthday, Odin One-Eye!
From Asgard to Nifelheim, you're the best all-father ever.
 
Love,
Debug Before forEach: [object Object]
Debug Inside: [object Window]
Debug Inside: [object Window]
```

`console.log()` shows where the bug is hiding. *inside* the `forEach`, the execution context **is not** `configuration Object` we used as a this argument. instead, `inside` function expression passed to `forEach` is the global object(`window` or `global`)

when a function is called whiteout "anything to the left of a dot". is does not get ites parent functions execution context automatically there are three common ways to solve this problem

1. Pass a `thisArg`
2. Use a closure
3. Use(something new) the arrow function expression

## Solution 1: Use a `thisArg` to avoid the lost context bug

Per the [forEach documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach), 

**TO FIX BUG:**  If we pass a `thisArg` argument to `forEach` as a **second** **argument**, it will **explicitly** provide context for the function used inside the `ForEach`.  THIS WILL WORK FOR BOTH (`forEach` & `map`)

```js
let configuration = {
    frontContent: "Happy Birthday, Odin One-Eye!",
    insideContent: "From Asgard to Nifelheim, you're the best all-father ever.\n\nLove,",
    closing: {
        "Thor": "Admiration, respect, and love",
        "Loki": "Your son"
    },
    signatories: [
        "Thor",
        "Loki"
    ]
}
 
let printCard = function() {
    console.log(this.frontContent)
    console.log(this.insideContent)
 
    this.signatories.forEach(function(signatory){
        let message = `${this.closing[signatory]}, ${signatory}`
        console.log(message)
    }, this)//here is the change, we use a function expression to as a call back forEach method, then as second argument use this for context when we use the function.call() method
}
printCard.call(configuration)

// as and alternitive we could have also said:
let printCard = function() {
    console.log(this.frontContent)
    console.log(this.insideContent)
    let contextBoundForEachExpr = function(signatory){ //set a variable to to function to be called later
        let message = `${this.closing[signatory]}, ${signatory}`
        console.log(message)
    }.bind(this)// here we use bind and passIn the thisArg, that way what of Object is being passed in is bound to the function in question
 
    this.signatories.forEach(contextBoundForEachExpr)// we use object.key(signatories) and call forEach passing in our function expression variable
}
printCard.call(configuration)
```

### For More reference

> In the "Context Lab" we used this approach to make sure that the reduce function in `allWagesFor` worked. Take a look at the implementation and see how `bind`-ing `reduce` saved you from falling into this bug *and* let you use the powerful `reduce` method.

## Solution 2: Use a Closure to Regain Access to the Lost Context

We took `this` that `printCard` has access to and re-passed it as both a `thisArg` to `forEach` and, provided it as context for `bind`. We have the ability to **“point to”** that context, we could also assign that value to a variable and leverage **function level scope** and ***closures*** to regain access to the outer context

```JS
let printCard = function() {
    console.log(this.frontContent)
    console.log(this.insideContent)
 
    let outerContext = this// here we express that outerCountext is the Object passedIn, or JS would call it SELF, byt doing this we set this to the parent FN-Level scope, so inside the innerfuntion this as access to the outer functions or parent scope function inside the inner function inside forEach call.
 
    this.signatories.forEach(function(signatory){
        let message = `${outerContext.closing[signatory]}, ${signatory}`
        console.log(message)
    })
}
 
printCard.call(configuration)
/*
Happy Birthday, Odin One-Eye!
From Asgard to Nifelheim, you're the best all-father ever.
Love,
Admiration, respect, and love, Thor
Your son, Loki
*/
```

when we are using an assignment with `let`, `var` or `const`, **we put the original context within the function-level scope that the inner function encloses as a closure**. This means inside the inner function, we can get "back" to the outer function's context.

What we would *really* like is for there to be a way to tell the `function` inside of `forEach` to

1. *Not* declare its own context **but also**
2. *Not* require us to do some extra work with using `bind` or a `thisArg`.

## Solution 3: Use an Arrow Function Expression to Create a Function Without Its Own Context

With ES6 we can use **arrow function expression(arrow function)** instead of plain old function expression, when we use **arrow function** instead of just function expression **we automatically bound** **the *arrow function* to its parent context and does not create a context of its own**

### **NOTE: about arrow function**

> Since *the whole point* of an arrow function is to ***not have its own execution context\***, we should not use `call`, `bind`, or `apply` when executing them.

An arrow function looks like this:

```js
// The let greeter is merely the assignment, the expression begins at `(`
let greeter = (nameToGreet) => {
    let message = `Good morning ${nameToGreet}`
    console.log(message)
    return "Greeted: " + nameToGreet
}
let result = greeter("Max") //=> "Greeted: Max"
```

Which, excluding context-switching differences, is the exact same as:

```js
let greeter = function(nameToGreet) {
    let message = `Good morning ${nameToGreet}`
    console.log(message)
    return "Greeted: " + nameToGreet
}.bind(this)
let result = greeter("Max Again") //=> "Greeted: Max Again"
```

Because arrow functions are *so often used* to take a value, do a single operation with it, and return the result, they have two shortcuts:

- If you pass only one argument, you don't have to wrap the single parameter in `()`
- If there is only one expression, you don't need to wrap it in `{}` and the result of that expression is automatically returned.
- Anti-Shortcut: If you *DO* use `{}`, you must explicitly `return` the return value

Thus Thor and Loki can fix their problem and wish their father a happy birthday most elegantly with the following code:

```js
let configuration = {
    frontContent: "Happy Birthday, Odin One-Eye!",
    insideContent: "From Asgard to Nifelheim, you're the best all-father ever.\n\nLove,",
    closing: {
        "Thor": "Admiration, respect, and love",
        "Loki": "Your son"
    },
    signatories: [
        "Thor",
        "Loki"
    ]
}
 
let printCard = function() {
    console.log(this.frontContent)
    console.log(this.insideContent)
    // Wow! Elegant! And notice the arrow function's `this` is the same
    // this that printCard has by virtue of configuration being passed
    // in as a thisArg
    this.signatories.forEach(s => console.log(`${this.closing[s]}, ${s}`)
    )
}
 
printCard.call(configuration)
/* OUTPUT:
Happy Birthday, Odin One-Eye!
From Asgard to Nifelheim, you're the best all-father ever.
Love,
Admiration, respect, and love, Thor
Your son, Loki
*/
```