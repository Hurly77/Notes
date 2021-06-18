## Identify `Object.assign()`

The first argument passed to `Object.assign()` is the first `Object` into which all of the properties are merged. Every additional argument is an `Object` whose properties we want to merge into the first `Object`:

```js
Object.assign(firstObject, secondObject, thirdObject, ...);
```

The return value of `Object.assign()` is the first `Object` after all of the additional `Object`s' properties have been merged in:

```js
Object.assign({ eggs: 3 }, { flour: '1 cup' });
// => { eggs: 3, flour: "1 cup" }
 
Object.assign({ eggs: 3 }, { chocolateChips: '1 cup', flour: '2 cups' }, { flour: '1/2 cup' });
// { eggs: 3, chocolateChips: "1 cup", flour: "1/2 cup" }
```

Let's look at the `flour` property in the above example. **If multiple `Object`s have a property with the same key, the last key to be defined wins out**. Essentially, the last call to `Object.assign()` in the above snippet is wrapping all of the following assignments into a single line of code:

```js
const recipe = { eggs: 3 };
 
recipe.chocolateChips = '1 cup';
 
recipe.flour = '2 cups';
 
recipe.flour = '1/2 cup';
```

## Non-Destructively Assign New Data with `Object.assign()`

A common pattern for `Object.assign()` is to provide an empty `Object` as the first argument. That way we're providing an entirely new `Object` instead of modifying or overwriting the properties of an existing `Object`.

```js
function nondestructivelyUpdateObject(obj, key, value) {
  return Object.assign({}, obj, { [key]: value });
}
 
const recipe = { eggs: 3 };
 
const newRecipe = nondestructivelyUpdateObject(recipe, 'chocolate', '1 cup');
// => { eggs: 3, chocolate: "1 cup" }
 
newRecipe;
// => { eggs: 3, chocolate: "1 cup" }
 
recipe;
// => { eggs: 3 }
```

In other languages (like Ruby), this behavior is called "merging." You take a original base `Object` (maybe with some typical "standard" attribute / value pairs already set), and then you "merge" in additional Object(s).

It's important that we merge everything into a new, empty `Object`. Otherwise, we would be modifying the original `Object`. In your browser's console, test what happens if the body of the above function were `return Object.assign(obj, { [key]: value });`.

Uh-oh! Back to being destructive! Fortunately, we can avoid that by following the example above.

## Explain Performance Gains from non-Destructive Updates

Doing non-destructive updates (i.e. "creating new things and merging on top") is a really important pattern. It turns out that, in many places, non-destructive updates are *more performant*. The main reason on this is when you add something to an existing `Object`, the computer has to make sure that the `Object` has enough room to add what you're saying to add. If it doesn't, the computer needs to do cleanup work, find some more space, copy the old thing over, add the new, thing, and then resume work, etc. That "accounting" process is actually quite slow.

`Object.assign`, however, avoids all that and says: "Hey, I created a thing that looks like this, give me space for it." This is faster.

*Advanced: For Your Information*

Furthermore, in the cloud-based world of programming we're moving more and more to, we can't be sure that two computers will share the same memory. They might be servers separated by centimeters or kilometers. When a design like this is used, we'll need to be sure to make sure the functions have "all they need" to run a function call *independently* i.e. they have their own copy of the data they need and aren't sharing memory with other machines.

We won't encounter these concerns in this module; however, cloud-based languages like Google's Go require us to think in this different paradigm to achieve massive scale. So, file it away for later!