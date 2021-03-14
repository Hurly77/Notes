## Define How the `filter()` Method Works

You can think of `filter()` as a `for` loop that specifically keeps or filters out certain values from an array. Consider the following code:

```js
let arr = [1, 2, 3, 4, 5, 6];
let even = [];
for(var i = 0; i < arr.length; i++) {
  if (arr[i] % 2 === 0) even.push(arr[i]);
}
// even = [2,4,6]
```

## Demonstrate `filter()`

**Filter** - receives the same arguments as `map` (current item, index, and entire array) in the callback function, and works very similarly. The only difference is that the callback needs to return either *true* or *false*.  If it returns *true*, the array keeps that element. If it returns false that element is filtered out.

Here is the earlier example written with `filter()`:

```js
let even = arr.filter(n => {
  return n % 2 === 0;
});
// even = [2,4,6]
```

