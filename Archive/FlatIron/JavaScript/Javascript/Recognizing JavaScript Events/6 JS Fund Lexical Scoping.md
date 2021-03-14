## Concept of Lexical Scoping

In JavaScript, each declared function creates its own *scope*. **Lexical scope** is scope that based on where variables and blocks of scope are defined by the programmer at the time it's written, and solidified by the time the code is processed.

## Demonstrate How Lexical Scoping Informs the Scope Chain of a Function

```js
const myVar = 'Foo';
 
function first () {
  console.log('Inside first()');
 
  console.log('myVar is currently equal to:', myVar);
}
 
function second () {
  const myVar = 'Bar';
 
  first();
}
```

JavaScript looks up the scope chain to perform identifier resolution. Given that information, what do you think will get logged out to the console when we invoke `second()`? Let's try it out:

```js
second();
// LOG: Inside first()
// LOG: myVar is currently equal to: Foo
// => undefined
```

![image-20200817210619081](C:\Users\camer\AppData\Roaming\Typora\typora-user-images\image-20200817210619081.png)

``` js
const myVar = 'Foo';
 
function second () {
  function first () {
    console.log('Inside first()');
 
    console.log('myVar is currently equal to:', myVar);
  }
 
  const myVar = 'Bar';
 
  first();
}
```

```js
second();
// LOG: Inside first()
// LOG: myVar is currently equal to: Bar
// => undefined
```

While `first()` is executing, it again encounters the reference to `myVar` and realizes it doesn't have a local variable or function with that name. `first()` looks up the scope chain again, but this time `first()`'s outer scope isn't the global scope. It's the scope of `second()` **because `first()` was declared inside `second()`**. So `first()` uses the copy of `myVar` from the `second()` scope, which contains the string `'Bar'`.