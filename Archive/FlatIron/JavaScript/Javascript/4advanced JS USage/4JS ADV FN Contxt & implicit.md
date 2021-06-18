## Define `this`

The JavaScript keyword `this` returns the current *execution context* while the function is being run

## Access Implicitly-Set Context in an Object-Contained Function Expression

When a function is called, it gets an execution context passed in. That context will be whatever the function was 'called on' - the object to the left of the `.` where it's called.

EX:

```js
let byronPoodle = {
  name: "Byron",
  sonicAttack: "ear-rupturing atomic bark",
  mostHatedThing: "noises in the apartment hallway",
  warn: function() {
    console.log(`${this.name} issues an ${this.sonicAttack} when he hears ${this.mostHatedThing}`)
  }
}
byronPoodle.warn()
// LOG: Byron issues an ear-rupturing atomic ba
```

A simple way of saying it: when you call `someObject.someFunction()`, the context inside of `someFunction` will be the thing to the left of the `.`: `someObject`.

Here's another interesting example:

```js
let speak = function() { return `It ain't easy being ${this.name}`}
let frog = { name: "Kermit" }
let pig = { name: "Miss Piggy" }
frog.speak = speak
pig.speak = speak
frog.speak === pig.speak //=> true
frog.speak()  //=> "It ain't easy being Kermit"
pig.speak()  //=> "It ain't easy being Miss Piggy"
```

Again, the crucial realization is that the context used *inside* the function `speak` is defined by what's "left of the dot."

## Access Implicitly-Set Global Object in a Function Expressions

a function **not** defined inside an `Object`:

```js
let contextReturner = function() {
  return this
}
 
contextReturner() //=> window
contextReturner() === window //=> true
```

When no object is to the left of the function, JavaScript invisibly adds **the global object**. Thus `contextReturner` is, from JavaScript's point of view, the same as `window.contextReturner`.

In browser-based JavaScript environment (or "JavaScript runtime"), the global object is called `window`. In NodeJS, it's called `global`.

Thus, in Chrome:

```js
let locationReturner = function() {
  return this.location.host
}
 
locationReturner() //=> URL host serving this page e.g. developer.mozilla.org
// Implicitly: window.locationReturner(); this will be `window` in the function
```

t's worth noting that even in a function inside of another function, the inner function's default context is still the global object:

```js
function a() {
  return function b() {
    return this;
  }
}
 
a()() === window //=> true
```

## Prevent Implicitly Setting a Global Object In Function Calls With `use strict`

If JavaScript sees `"use strict"` at the top of a JavaScript code file, it will apply this rule (and other strict behaviors) to *all functions*.

```js
function looseyGoosey() {
  return this
}
 
function noInferringAllowed() {
  "use strict"
  return this
}
 
looseyGoosey() === window; //=> true
noInferringAllowed() === undefined //=> true
```

### Access Implicitly-Set New Object in Object-Oriented Programming Constructor

 An important place where `this` is implicitly set is when new instances of classes are created. remember this rule for later. When you see `this` inside of a class definition in JavaScript, come back and make sure you understand this **rule.**

> "The thing we're setting up should be the default context for work during its construction in its own function that's called `constructor`."

```js
class Poodle{
  constructor(name, pronoun){
    this.name = name;
    this.pronoun = pronoun
    this.sonicAttack = "ear-rupturing atomic bark"
    this.mostHatedThing = "noises in the apartment hallway"
  }
 
  warn() {
    console.log(`${this.name} issues an ${this.sonicAttack} when ${this.pronoun} hears ${this.mostHatedThing}`)
  }
}
let b = new Poodle("Byron", "he")
b.warn() //=> Byron issues an ear-rupturing atomic bark when he hears noises in the apartment hallway
```

## Use Available JavaScript Runtimes to Validate Understanding

There are two main JavaScript environments you'll encounter as you get started with JavaScript. Those environments are also called runtimes. They are:

1. in the browser
2. in the "shell" when running the NodeJS program

For the remainder of this module, it's very important that you not only ***read\*** these lessons but that you practice, adjust, test, and explore the material within one (or both!) of those JavaScript runtimes.

Take the initiative to own your own learning and try out these samples (with some variation!) in a runtime of your choice.

# **JS Advanced Functions: Context And Explicit S...**

## Explicitly Override Context with `call` and `apply`

```js
let asgardianBrothers = [
  {
    firstName: "Thor",
    familyName: "Odinsson"
  },
  {
    firstName: "Loki",
    familyName: "Laufeysson-Odinsson"
  }
]
 
function intro(person, line) {
  return `${person.firstName} ${person.familyName} says: ${line}`
}
 
function introWithContext(line){
  return `${this.firstName} ${this.familyName} says: ${line}`
}
 
let phrase = "I like this brown drink very much, bring me another!"
intro(asgardianBrothers[0], phrase) //=> Thor Odinsson says: I like this brown drink very much, bring me another!
intro(asgardianBrothers[0], phrase) === introWithContext.call(asgardianBrothers[0], phrase) //=> true
intro(asgardianBrothers[0], phrase) === introWithContext.apply(asgardianBrothers[0], [phrase]) //=> true
 
let complaint = "I was falling for thirty minutes!"
intro(asgardianBrothers[1], complaint) === introWithContext.call(asgardianBrothers[1], complaint) //=> true
intro(asgardianBrothers[1], complaint) === introWithContext.apply(asgardianBrothers[1], [complaint]) //=> true
```

The `introWithContext` function expects only a catchphrase as an argument. Both `call` and `apply` take a `thisArg` argument as their first argument  that argument becomes the `this` *inside* the function. In the case of `call`, anything after the `thisArg` gets passed to the function like arguments inside of a `()`

## Explicitly Lock Context For a Function With `bind`

permanently bound to `asgardianBrothers[0]`. As the adjective "bound" suggests, we use `bind`:

```js
let thorIntro = introWithContext.bind(asgardianBrothers[0])
thorIntro("Hi, Jane") //=> Thor Odinsson says: Hi, Jane
thorIntro("I love snakes") //=> Thor Odinsson says: I love snakes
```

The `bind` method ***returns a function that needs to be called\***, but wherever the function that `bind` was called on had a `this` reference, the `this` is "hard set" to what was passed into `bind`.