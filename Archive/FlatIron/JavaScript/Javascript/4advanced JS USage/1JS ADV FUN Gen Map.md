## Term "callback"

When we pass a function expression (an anonymous function) or the *pointer* (variable name, declared function name) to a function as an argument, the passed function is called a *callback*. 

## Duplication That Can Be Avoided With Callbacks

Let's look at this solution code from the previous lesson:

```js
function mapToNegativize(src) {
  let r = []
  for (let i = 0; i < src.length; i++ ) {
    r.push(-1 * src[i]) // Unique work
  }
  return r
}
 
function mapToNoChange(src) {
  let r = []
  for (let i = 0; i < src.length; i++ ) {
    r.push(src[i]) // Unique work
  }
  return r
}
 
function mapToDouble(src) {
  let r = []
  for (let i = 0; i < src.length; i++ ) {
    r.push(2 * src[i]) // Unique work
  }
  return r
}
 
function mapToSquare(src) {
  let r = []
  for (let i = 0; i < src.length; i++ ) {
    r.push(src[i] * src[i]) // Unique work
  }
  return r
}
```

In this code, `functionUsingCallback` will receive each of the three functions passed to it and will store those functions in `en`, `es`, and `zh`.

## Call a Callback from Within a Function

```js
function sayHello(name="") {
  console.log(`Hello${name}`)
}
 
let sayHola = function(name="") {
  console.log(`Hola${name}`)
}
 
functionUsingCallback(sayHello, sayHola, function(name="") {
  console.log(`Ni Hao${name}`)
})
 
function functionUsingCallback(en, es, zh, name){
  en()
  es()
  zh()
}
 
/* Prints */
Hello
Hola
Ni Hao
```

## Pass Data Between Functions and Callbacks

As we're already familiar with, we can pass arguments when we *call* functions by putting them inside the `()`. By doing so we can share knowledge from within the function to its passed-in callback(s)

```js
function sayHello(name="") {
  console.log(`Hello${name}`)
}
 
let sayHola = function(name="") {
  console.log(`Hola${name}`)
}
 
functionUsingCallback(sayHello, sayHola, function(name="") {
  console.log(`Ni Hao${name}`)
}, " Gary")
 
function functionUsingCallback(en, es, zh, name){
  en(name)
  es(name)
  zh(name)
}
 
/* Prints */
Hello Gary
Hola Gary
Ni Hao Gary
```

## Identify JavaScript's Non-enforcement of *Arity*

One interesting point about calling functions in JavaScript is that JavaScript doesn't require you to provide the number of arguments to the function that the function's parameters expect.

```js
function accidentProneMen(larry, curly, moe){
  return `${larry}, ${curly}, and ${moe} are an insurance risk.`
}
accidentProneMen() //=> undefined, undefined, and undefined are an insurance risk.
accidentProneMen("Fine") //=> Fine, undefined, and undefined are an insurance risk.
accidentProneMen("Fine", undefined, "Howard") //=> Fine, undefined, and Howard are an insurance risk.
```

```js
// Add your functions here
function map(a, fun){
  let n = []
  for (let i = 0; i < a.length; i++) {
    const el = a[i];
    n.push(fun(el))
  }
  return n

}

function reduce(a, fun, start){
  let cV = (!!start) ? start : a[0];
  let i = (!!start) ? 0 : 1;

  for (; i < a.length; i++) {
      cV = fun(a[i], cV)
  }
  
  return cV
}
```

