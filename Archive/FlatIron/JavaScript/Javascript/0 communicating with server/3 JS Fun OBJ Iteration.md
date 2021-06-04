```
i = 0
c = 1
while i < array.length + 2
array[i].union(array[c])
 i = i + 2
 c = c + 2
```



## Looping vs. iteration

Looping is the process of executing a set of statements **repeatedly until a condition is met**. 

Iteration is the process of executing a set of statements **once for each element in a collection**

## `for...of`

```js
const myArray = ['a', 'b', 'c', 'd', 'e', 'f', 'g'];
 
for (const element of myArray) {
  console.log(element);
}
```

 On each trip into the loop body (which is a *block* — note the curly braces), we assign the next element in the collection to a **new** `element` variable. Upon reaching the end of the block, the block-scoped variable vanishes, and we return to the top. Then we repeat the process, assigning the next element in the collection to a **new** `element` variable.

### Iterating over... strings?

```
for (const char of 'Hello, world!') {
  console.log(char);
}
 
// LOG: H
// LOG: e
// LOG: l
// LOG: l
// LOG: o
// LOG: ,
// LOG:
// LOG: w
// LOG: o
// LOG: r
// LOG: l
// LOG: d
// LOG: !
```

### Usage

Use a `for...of` statement anytime you want to iterate over an array.

## Iterating over objects

`for...in` statement has been around for a long time, and it's usually used for iterating over the properties in an object.

```js
for (const <KEY> in <OBJECT>) {
  // Code in the statement body
}
```

The `for...in` statement iterates over the properties in an object, but it doesn't pass the entire property into the block. Instead, it only passes in the *keys*:

```js
const address = {
  street1: '11 Broadway',
  street2: '2nd Floor',
  city: 'New York',
  state: 'NY',
  zipCode: 10004
};
 
for (const key in address) {
  console.log(key);
}
 
// LOG: street1
// LOG: street2
// LOG: city
// LOG: state
// LOG: zipCode
```

#### Accessing the object's values

```js
const address = {
  street1: '11 Broadway',
  street2: '2nd Floor',
  city: 'New York',
  state: 'NY',
  zipCode: 10004
};
 
for (const key in address) {
    console.log(address[key]);
}
 
// LOG: 11 Broadway
// LOG: 2nd Floor
// LOG: New York
// LOG: NY
// LOG: 10004
```

#### DONT DO THE DOT OPERATOR

variables don't work with the dot operator because it treats the variable name as a literal key — that is, `address.key` is trying to access the property on `address` with a key of `key`. Since there is no `key` property in `address`, it returns `undefined`. To prove this, let's add a `key` property to `address`:

```
variables don't work with the dot operator because it treats the variable name as a literal key — that is, address.key is trying to access the property on address with a key of key. Since there is no key property in address, it returns undefined. To prove this, let's add a key property to address:
```

### Usage

Use a `for...in` statement whenever you want to **enumerate** the properties of an object.

> **enumerate** - is when an **enumerated** type (also called **enumeration** or enum [..]) is a data type consisting of a set of named values called elements, members or enumerators of the type. ... A **variable** that has been **declared** as having an **enumerated** type can be **assigned** any of the **enumerators** as a value

### `for...in` and order

As a general rule, **don't use `for...in` with arrays**. When iterating over an array, an **ordered** collection, we would expect the elements in the array to be dealt with **in order**. However, because of how `for...in` works under the hood, there's no guarantee of order. From the [MDN documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in):

> A `for...in` loop iterates over the properties of an object in an **arbitrary order** ... one cannot depend on the seeming orderliness of iteration, at least in a cross-browser setting).

 in the pre-ES2015 days, using `for...in` would often result in different browsers iterating over the same object's properties in different orders. That's not cool! Cross-browser consistency is very important. A lot of progress has been made towards standardizing the behavior of `for...in` across all major browsers, but there's still no reason to use `for...in` with arrays when we have the wonderfully consistent `for...of` tailor-made for the job