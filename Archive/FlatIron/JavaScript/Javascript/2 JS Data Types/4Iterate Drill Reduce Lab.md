we're interested in getting a total price of all products in our basket. The store data looks like this:

```js
const products = [
  { name: 'Shampoo', price: 4.99 },
  { name: 'Donuts', price: 7.99 },
  { name: 'Cookies', price: 6.49 },
  { name: 'Bath Gel', price: 13.99 }
];
```

 Let's create a function that has an initial value, then iterates the given products and adds their price to the total price. When the loop has finished, we return our `totalPrice` result:

```js
function getTotalAmountForProducts(products) {
  let totalPrice = 0;
 
  products.forEach(function(product) {
    totalPrice += product.price;
  });
 
  return totalPrice;
}
 
console.log(getTotalAmountForProducts(products)); // 
```

We could make our solution more abstract by writing a generalized function that accepts two additional arguments: an initial value and a callback function that implements the reduce functionality we want.

let's count the number of coupons we have lying around the house:

```js
const couponLocations = [
  { room: 'Living room', amount: 5 },
  { room: 'Kitchen', amount: 2 },
  { room: 'Bathroom', amount: 1 },
  { room: 'Master bedroom', amount: 7 }
];
 
function ourReduce(arr, reducer, init) {
    let accum = init;
    arr.forEach(element => {
        accum = reducer(accum, element);
    });
    return accum;
}
 
function couponCounter(totalAmount, location) {
  return totalAmount + location.amount;
}
 
console.log(ourReduce(couponLocations, couponCounter, 0)); // prints 15
```

 for example, we already have three coupons in our hand, we can easily account for that by adjusting the initial value when we call `ourReduce`, without having to change any code:

```js
console.log(ourReduce(couponLocations, couponCounter, 3)); // prin
```

## Demonstrate `reduce`

With JavaScript’s `reduce()` method, we don't need to write our own version. Just like `ourReduce`, the `reduce()` method is used when we want to get something useful out of each element in the collection and gather that information into a final summary object or value. Let's take the native implementation with our previous example for a spin:

```js
console.log(couponLocations.reduce(couponCounter, 0)); // also prints 15!
```

Another simple numerical example:

```js
let doubledAndSummed = [1, 2, 3].reduce(function(total, element){ return element * 2 + total}, 0)
// => 12
```

The initialization value can be left out but it might lead to a real surprise. If *no* initial value is supplied, the first element is used *without having been used in the function*:

```js
let doubledAndSummed = [1, 2, 3].reduce(function(total, element){ return element * 2 + total})
// => 11
```

The initialization value can be changed:

```js
let doubledAndSummedFromTen = [1, 2, 3].reduce(function(total, element){ return element * 2 + total}, 10)
// => 22
```

For more powerful uses, we could use:

```js
let hogwarts_houses = {
  "Slytherin": [],
  "Gryffindor": [],
  "Hufflepuff": [],
  "Ravenclaw": []
}
 
/*
Assume sorting_hat.assign() returns a String ("Slytherin", "Gryffindor",
"Hufflepuff", "Ravenclaw") based on the argument passed in.
*/

incoming_students.reduce(function(houses, student) { houses[sorting_hat.assign(student)].push(student)} , hogwarts_houses)
```

Here we iterate a collection of students and assign each one to a pre-existing accumulator `Object`. We ask the `Object` to look up an `Array` keyed by the name of the houses. We then `push()` the student into that `Array`. Later in the code:

```js
hogwarts_houses["Gryffindor"] //=> [hermioneGranger, ronWeasley, harryPotter]
```

