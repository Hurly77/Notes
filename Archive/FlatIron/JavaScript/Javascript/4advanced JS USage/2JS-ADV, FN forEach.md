##   it's a good idea to avoid `forEach` method

Programmers, including you, recognize that `map` has a specific use, `reduce` has a specific use, `max` has a specific use. But `forEach` is generic.

## Cases for `forEach`

The best time to use `forEach` is when you need to enumerate a collection to (1) cause some sort of "side-effect" or (2) are changing the elements ("mutating") as you go along.

The best example of "doing a side-effect" is using `console.log()`. This function doesn't return anything back. We don't care about the data that returns from this action

This is pretty common in debugging:

```js
  empCollection.forEach(function(e){
    console.log("DEBUG: WHAT ARE YOU?!?" + e)
  })
```

The other time we want to use `forEach` is when the element will be changed by the process of `forEach`-ing. 

```js
function addFullNameToEmployees(empCollection){
  empCollection.forEach(function(e){
    e.fullName = `${e.firstName} ${e.familyName}`
  })
}
 
addFullNameToEmployees([
  {firstName: "Byron", familyName: "Karbitii"},
  {firstName: "Luca", familyName: "Tuexedensis"}
])
```

There's **nothing generated** by the `forEach` that we want to preserve (**if** that were so, we'd **want to use** `map`).

