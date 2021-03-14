*sublist* (e.g. Animals > Vertebrate; Animals > Invertebrate). We link the connections between these lists in our heads, and it doesn't cause any issues to think of one list containing other lists. The index itself is, after all, a kind of list.

## Explain What a Nested Object Looks Like

 the values in an object **can also be other objects**.

```js
const numberCollections = [1, [2, [4, [5, [6]], 3]]]
//Given the above nested array, how would we get the number 6?

//First, we'd need the second element of numberCollections:
numberCollections[1]
//=> [2, [4, [5, [6]], 3]

//Then we'd need the second element of that element:
numberCollections[1][1]
//=> [4, [5, [6]], 3]

//Then get the second element of that element:
numberCollections[1][1][1]
//=> [5, [6]]

//And again:
numberCollections[1][1][1][1]
//=> [6]

//Finally, the first element of that element to return the number, instead of an Array:
numberCollections[1][1][1][1][0]
//=> 6
```

remember that each lookup (square brackets) selects a different array for each subsequent lookup. To recap, what we're really doing is:

```js
[1, [2, [4, [5, [6]], 3]]] // numberCollections
[2, [4, [5, [6]], 3]]      // numberCollections[1]
[4, [5, [6]], 3]           // numberCollections[1][1]
[5, [6]]                   // numberCollections[1][1][1]
[6]                        // numberCollections[1][1][1][1]
6                          // numberCollections[1][1][1][1][0]
```

## Find an Element in a Nested Array

### Use `for`

`find` function that takes two arguments: an array (which can contain sub-arrays) and a function that returns `true` for the thing that we're looking for.

```js
function find(array, criteriaFn) {
  for (let i = 0; i < array.length; i++) {
    if (criteriaFn(array[i])) {
      return array[i]
    }
  }
}
```

**REMEMBER** In JavaScript, functions are "first class data" just like `Number`s. It's probably not surprising to imagine writing: `find( [1,2,["a", "b"]], 3.14)` or `find( [1,2,["a", "b"]], "Byron the Poodle")`. Since functions are first-class data, just like `Number`s, it means that the following code shouldn't stay foreign to you: `find( [1,2,["a", "b"]], function(someNumber){ return someNumber % 2 === 0} )`. In fact, writing functions that test some condition (a "criteria function") is ***so\*** common, JavaScript even has an advanced syntax for writing functions that do one evaluation and return the value: *the arrow function syntax*. Thus we could write: `find( [1,2,["a", "b"]], someNumber => someNumber % 2 === 0 )`. This code would do something like find all the members in the array that are even.

```js
function find(array, criteriaFn) {
  // initialize two variables, `current`, and `next`
  // `current` keeps track of the element that we're
  // currently on, just like we did when unpacking the
  // array above; `next` is itself an array that keeps
  // track of the elements (which might be arrays!) that
  // we haven't looked at yet
  let current = array
  let next = []
 
  // hey, a `while` loop! this loop will only
  // trigger if `current` is truthy — so when
  // we exhaust `next` and, below, attempt to
  // `shift()` `undefined` (when `next` is empty)
  // onto `current`, we'll exit the loop
  //
  // Note that we had to add on this || statement
  // if current is the number 0 it won't be run in the
  // loop. Recall your truthy / falsey rules! This is
  // a subtle bug that went unnoticed in this code for
  // many years!
  while (current || current === 0) {
    // if `current` satisfies the `criteriaFn`, then
    // return it — recall that `return` will exit the
    // entire function!
    if (criteriaFn(current)) {
      return current
    }
 
    // if `current` is an array, we want to push all of
    // its elements (which might be arrays) onto `next`
    if (Array.isArray(current)) {
      for (let i = 0; i < current.length; i++) {
        next.push(current[i])
      }
    }
 
    // after pushing any children (if there
    // are any) of `current` onto `next`, we want to take
    // the first element of `next` and make it the
    // new `current` for the next pass of the `while`
    // loop
    current = next.shift()
  }
 
  // if we haven't
  return null
}
```

### Try This to Understand better

```js
const numberCollections = [1, [2, [4, [5, [6]], 3]]]

function find(array, criteriaFn) {
	let current = array;
	let next = [];
	while (current || current === 0) {
		if (criteriaFn(current)) {
			return current;
		}
		if (Array.isArray(current)) {
			for (let i = 0; i < current.length; i++) {
				next.push(current[i]);
			}
		}
		current = next.shift();
	}
	return null;
}
```

#### THEN RUN:

```JS
find(numberCollections, number => number > 5)
//=> [6]
find(array, number => (typeof number === 'number' && number > 5))
//=> 6

```

just implemented your first **[breadth-first search](https://en.wikipedia.org/wiki/Breadth-first_search)**!

### A Challenge, Should You Choose to Accept It

Can you modify the breadth-first search algorithm in such a way that it will traverse both nested objects and nested arrays (or even a mix of both)?

```js
function find(object, criteriaFn) {
	let current = object;
	let next = [];

	while (current || current === undefined) {
		if (criteriaFn(current)) {
			return current;
		}

		if (Array.isArray(current)) {
			for (let i = 0; i < current.length; i++) {
				next.push(current[i]);
			}
		}

		if (typeof current === 'object') {
			for (const key in current) {
				current = current[key];
				next.push(current);
			}
		}
		current = next.shift();
	}
}

a = { 1: [ 3, [ 3, { 1: [ 4, { here: 5 } ] } ] ] };
find(mix, num => num > 4)
```

