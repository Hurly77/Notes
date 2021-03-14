## Use JavaScript's Default Function Argument as a Parameter in a Function

e have a function that takes in an `itemPrice` as a price in dollars and a `discount` as a percentage, and returns the total amount due:

```js
function discountedPrice(itemPrice){
    return itemPrice - (itemPrice * 0.25)
}
```

But it seems unwise to hard-code the discount to `0.25`

```js
function discountedPrice(itemPrice, discount){
    return itemPrice - (itemPrice * discount)
}
```

So, calls to `discountedPrice` will look like:

```js
function discountedPrice(itemPrice, discount){
    return itemPrice - (itemPrice * discount)
}
discountedPrice(100, 0.25) //=> 75.0
```

'll be 25% off *unless* we choose to pass a new discount percentage into `discountedPrice`.

When writing functions that have *parameters* that take default values, it's best practice to put them at the **end** of the parameter list.

```js
function discountedPrice(itemPrice, discount = 0.25){
    return itemPrice - (itemPrice * discount)
}
discountedPrice(100) //=> 75.0
discountedPrice(100, 0.5) //=> 50.0
```

What would happen if we added `tax` to `discountedPrice` so that we could add a tax percentage to be added to the sales price? We'll honor best pratices and put `tax` before `discount`.

```js
function discountedAndTaxedPrice(itemPrice, tax, discount = 0.25){
    return itemPrice - (itemPrice * discount) + (itemPrice * tax)
}
discountedAndTaxedPrice(100, 0.15) //=> 90
```

Use JavaScript's rest Parameter as a Parameter in a Function

```js
 
function muppetLab(a,b){
  console.log(a,b) // Dr. Bunson Beaker
}
 
muppetLab("Dr. Bunson", "Beaker", "Miss Piggy", "Kermit", "Animal")
```

The `rest` parameter allows us to take the rest of the arguments that we pass into the function, and gather them into an array. Here's how this works:

```js
function muppetLab(a, b, ...muppets) {
  console.log(a, ' ', b); // Dr. Bunson Beaker
 
  console.log(muppets); // ["Miss Piggy", "Kermit", "Animal"]
  console.log(muppets[0]); // Miss Piggy
  console.log(muppets.length); // 3
}
 
muppetLab("Dr. Bunson", "Beaker", "Miss Piggy", "Kermit", "Animal")
```

### Use JavaScript's `spread` Operator as a Parameter in a Function

With the `rest` operator, JavaScript allowed us to put the remaining arguments into an array. The `spread` operator allows us to pass elements of an array into a function as an argument. Try it out in console with a simple add function:

```js
function add(a, b, c) {
  return a + b + c ;
}
const arr = [1, 2, 3];
 
add(...arr); // returns 6
```

So what's happening here? We have a simple add function, with three arguments, and we are passing in an array using the `spread` operator. The function is adding all three numbers contained within the array. Play around with it using a bigger array and see what happens when the array has more numbers than our function has parameters.