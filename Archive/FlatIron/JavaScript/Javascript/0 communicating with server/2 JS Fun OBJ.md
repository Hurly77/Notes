**IMPORTANT**

> **ASIDE**: Un-helpfully JavaScript called this thing with curly braces (`{}`) an `Object`.
>
> It's similar to Ruby's `Hash`, Python's `Dictionary` or C-like languages' `struct`(ure).
>
> There *is* a relationship to object-oriented programming where we create classes and instances, but that's not what's at play here. Don't confuse the two.
>
> When JavaScript was created, it was thinking it'd only be a lightweight language for doing DOM-based programming. It didn't think that it would ever need "object orientation" like Java or C++ did. But JavaScript turned out to be way more popular than even it expected and **later** gained that class- and instance-based thing known as "Object-Oriented Programming."
>
> So for this lesson and the next few that follow, try not to think of `Object` as being like "Object-Oriented Programming," but instead think of it as being closer to a key/value based data structure.

We can have empty `Object`s:	

```js
{}
```

Or `Object`s with a single *key-value* pair:

```js
{key: value}
```

When we have to represent multiple key-value pairs in the same `Object` (which is most of the time), we use commas to separate them out:

```javascript
{
key1: value1,
key2: value2
}
```

Address as an object

```js
const address = {
  street1: '11 Broadway',
  street2: '2nd Floor',
  city: 'New York',
  state: 'NY',
  zipCode: 10004
};
```

### Dot Notation

*dot notation*, we use the *member access operator* (a single period) 

```javascript
address.street1;
// => "11 Broadway"
 
address.street2;
// => "2nd Floor"
 
address.city;
// => "New York"
 
address.state;
// => "NY"
 
address.zipCode;
// => 10004
```

 try to access the `country` property of our `address` `Object`

```js
address.country;
// => undefined
```

It returns `undefined` because there is no matching key on the `Object`

```js
ddress['street1'];
// => "11 Broadway"
 
address['street2'];
// => "2nd Floor"
 
address['city'];
// => "New York"
 
address['state'];
// => "NY"
 
address['zipCode'];
// => 10004
```

there are two main situations in which to use bracket notation.

### With Nonstandard Keys

##### Bracket Notation

If (for whatever reason) you need to use a nonstandard string as the key in an `Object`, you'll only be able to access the properties with bracket notation. For example, this is a valid `Object`:

```js
const wildKeys = {
  'Cash rules everything around me.': 'Wu',
  'C.R.E.A.M.': 'Tang',
  'Get the money.': 'For',
  "$ $ bill, y'all!": 'Ever'
};
//It's impossible to access those properties with dot notation:
//But bracket notation works just fine:
```

follow these rules when naming your keys, everything will work out:

- camelCaseEverything
- Start the key with a lowercase letter
- No spaces or punctuation

#### Accessing Properties Dynamically

we can access the `zipCode` property from our `address` `Object` like so:

```js
address['zip' + 'Code'];
// => 10004
```

the real strength of bracket notation is its ability to compute the value of variables on the fly

```js
const meals = {
  breakfast: 'Oatmeal',
  lunch: 'Caesar salad',
  dinner: 'Chimichangas'
};
 
let mealName = 'lunch';
 
meals[mealName];
// => "Caesar salad"
```

If we try to use the `mealName` variable with dot notation, it doesn't work:

```js
mealName = 'dinner';
// => "dinner"
 
meals.mealName;
// => undefined
```

Essentially, dot notation is for when you know the exact name of the property in advance, and bracket notation is for when you need to compute it when the program runs.

The need for bracket notation doesn't stop at dynamically setting properties on an already-created `Object`. Since the ES2015 update to JavaScript, we can also use bracket notation to dynamically set properties *during the creation of a new `Object`*. For example:

```js
const morningMeal = 'breakfast';
const middayMeal = 'lunch';
const eveningMeal = 'dinner';
 
const meals = {
  [morningMeal]: 'French toast',
  [middayMeal]: 'Personal pizza',
  [eveningMeal]: 'Fish and chips'
};
 
meals;
// => { breakfast: "French toast", lunch: "Personal pizza", dinner: "Fish and chips" }
```

 try doing the same thing without the square brackets:

```js
const morningMeal = 'breakfast';
const middayMeal = 'lunch';
const eveningMeal = 'dinner';
 
const meals = {
  morningMeal: 'French toast',
  middayMeal: 'Personal pizza',
  eveningMeal: 'Fish and chips'
};
 
meals;
// => { morningMeal: "French toast", middayMeal: "Personal pizza", eveningMeal: "Fish and chips" }
```

### Add a Property to an Object

```js
const circle = {};
 
circle.radius = 5;
 
circle['diameter'] = 10;
 
circle.circumference = circle.diameter * Math.PI;
// => 31.41592653589793
 
circle['area'] = Math.PI * circle.radius ** 2;
// => 78.53981633974483
 
circle;
// => { radius: 5, diameter: 10, circumference: 31.41592653589793, area: 78.53981633974483 }
```

Multiple properties can have the same value:

```js
const meals = {
  breakfast: 'Avocado toast',
  lunch: 'Avocado toast',
  dinner: 'Avocado toast'
};
 
meals.breakfast;
// => "Avocado toast"
 
meals.dinner;
// => "Avocado toast"
```

 keys must be unique. If the same key is used for multiple properties, only the final value will be retained

```js
const meals = {
  breakfast: 'Avocado toast',
  breakfast: 'Oatmeal',
  breakfast: 'Scrambled eggs'
};
 
meals;
// => { breakfast: "Scrambled eggs" }
```

update or overwrite existing properties by assigning a new value to an existing key:

```js
const mondayMenu = {
  cheesePlate: {
    soft: 'Chèvre',
    semiSoft: 'Gruyère',
    hard: 'Manchego'
  },
  fries: 'Curly',
  salad: 'Cobb'
};
 
mondayMenu.fries = 'Sweet potato';
 
mondayMenu;
// => { cheesePlate: { soft: "Chèvre", semiSoft: "Gruyère", hard: "Manchego" }, fries: "Sweet potato", salad: "Cobb" }
```

modifying an `Object`'s properties in the way we did above is *destructive*. 

We could encapsulate this modification process in a function, like so:

```js
function destructivelyUpdateObject (obj, key, value) {
  obj[key] = value;
 
  return obj;
}
```

 It's time to update our `mondayMenu` to the `tuesdayMenu`:

```js
const mondayMenu = {
  cheesePlate: {
    soft: 'Chèvre',
    semiSoft: 'Gruyère',
    hard: 'Manchego'
  },
  fries: 'Sweet potato',
  salad: 'Cobb'
};
 
const tuesdayMenu = destructivelyUpdateObject(mondayMenu, 'salad', 'Caesar');
// => { cheesePlate: { soft: "Chèvre", semiSoft: "Gruyère", hard: "Manchego" }, fries: "Sweet potato", salad: "Caesar" }
 
tuesdayMenu.salad;
// => "Caesar"
```

Looks like our `tuesdayMenu` came out perfectly. But what about `mondayMenu`?

```js
mondayMenu.salad;
// => "Caesar"
```

### Use Convenience Methods Like `Object.assign()` and `Object.keys()`

create a method that accepts the old menu and the proposed change:

```js
function nondestructivelyUpdateObject(obj, key, value) {
  // Code to return new, updated menu object
}
```

Then, with the ES2015 *spread operator*, we can copy all of the old menu `Object`'s properties into a new `Object`:

```js
function nondestructivelyUpdateObject(obj, key, value) {
  const newObj = { ...obj };
 
  // Code to return new, updated menu object
}
```

And finally, we can update the new menu `Object` with the proposed change and return the updated menu:

```js
function nondestructivelyUpdateObject(obj, key, value) {
  const newObj = { ...obj };
 
  newObj[key] = value;
 
  return newObj;
}
 
const sundayMenu = nondestructivelyUpdateObject(tuesdayMenu, 'fries', 'Shoestring');
 
tuesdayMenu.fries;
// => "Sweet potato"
 
sundayMenu.fries;
// => "Shoestring"
```

The *data* in a `const` pointing to an `Array` or `Object` can still be changed, but a new value *cannot* be assigned to the name. Given `const x = {}` it's OK to say `x.dog = "Poodle"`, but it is ***not\*** OK to say `x = [1,2,3]`.

If we want to modify more than a single property, we'll have to completely rewrite our function! Luckily, JavaScript has a much better solution for us.

### `Object.assign()`

`Object.assign()`, which allows us to combine properties from multiple `Object`s into a single `Object`. first argument passed to `Object.assign()` is the initial `Object` in which all of the properties are merged. Every additional argument is an `Object` whose properties we want to merge into the first `Object`:

```js
Object.assign(initialObject, additionalObject, additionalObject, ...);
```

e return value of `Object.assign()` is the initial `Object` after all of the additional `Object`s' properties have been merged in:

```js
Object.assign({ eggs: 3 }, { flour: '1 cup' });
// => { eggs: 3, flour: "1 cup" }
 
Object.assign({ eggs: 3 }, { chocolateChips: '1 cup', flour: '2 cups' }, { flour: '1/2 cup' });
// { eggs: 3, chocolateChips: "1 cup", flour: "1/2 cup" }
```

**If multiple `Object`s have a property with the same key, the last key to be defined wins out**

```js
const recipe = { eggs: 3 };
 
recipe.chocolateChips = '1 cup';
 //THIS IS DISTRUCTIVE
recipe.flour = '2 cups';
 //the last call to Object.assign() is wrapping all of the following assignments into a single line of code:
recipe.flour = '1/2 cup';
```

A common pattern for `Object.assign()` is to provide an empty `Object` as the first argument. This pattern allows us to rewrite the above `destructivelyUpdateObject()` function in a nondestructive way:

```JS
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

 make and returns a new menu **without modifying the old menu**:

```js
function createNewMenu (oldMenu, menuChanges) {
  return Object.assign({}, oldMenu, menuChanges);
}
 
const tuesdayMenu = {
  cheesePlate: {
    soft: 'Chèvre',
    semiSoft: 'Gruyère',
    hard: 'Manchego'
  },
  fries: 'Sweet potato',
  salad: 'Caesar'
};
 
const newOfferings = {
  cheesePlate: {
    soft: 'Brie',
    semiSoft: 'Fontina',
    hard: 'Provolone'
  },
  salad: 'Southwestern'
};
 
const wednesdayMenu = createNewMenu(tuesdayMenu, newOfferings);
 
wednesdayMenu;
// => { cheesePlate: { soft: "Brie", semiSoft: "Fontina", hard: "Provolone" }, fries: "Sweet potato", salad: "Southwestern" }
 
tuesdayMenu;
// => { cheesePlate: { soft: "Chèvre", semiSoft: "Gruyère", hard: "Manchego" }, fries: "Sweet potato", salad: "Caesar" }
```

**Note:** For deep cloning, we need to use other alternatives because `Object.assign()` copies property values.

For example, if `newOfferings` did not have an updated value for `hard` cheese such as:

```js
const newOfferings = {
  cheesePlate: {
    soft: 'Brie',
    semiSoft: 'Fontina'
  },
  salad: 'Southwestern'
};

//Our output for const wednesdayMenu = createNewMenu(tuesdayMenu, newOfferings); would look like this:
wednesdayMenu;
// => { cheesePlate: { soft: "Brie", semiSoft: "Fontina"}, fries: "Sweet potato", salad: "Southwestern" }

```

... instead of the desired outcome of this:

```js
wednesdayMenu;
// => { cheesePlate: { soft: "Brie", semiSoft: "Fontina", hard: 'Manchego'}, fries: "Sweet potato", salad: "Southwestern" }
```

### `Object.keys()`

When an `Object` is passed as an argument to `Object.keys()`, the return value is an `Array` containing all of the keys at the top level of the `Object`

```js
const wednesdayMenu = {
  cheesePlate: {
    soft: 'Brie',
    semiSoft: 'Fontina',
    hard: 'Provolone'
  },
  fries: 'Sweet potato',
  salad: 'Southwestern'
};
 
Object.keys(wednesdayMenu);
// => ["cheesePlate", "fries", "salad"]

//Notice that it didn't pick up the keys in the nested cheesePlate Object
```

Uh oh, we ran out of Southwestern dressing, so we have to take the salad off the menu. In JavaScript, that's as easy as:

```js
const wednesdayMenu = {
  cheesePlate: {
    soft: 'Brie',
    semiSoft: 'Fontina',
    hard: 'Provolone'
  },
  fries: 'Sweet potato',
  salad: 'Southwestern'
};
 
delete wednesdayMenu.salad;
// => true
 
wednesdayMenu;
// => { cheesePlate: { soft: "Brie", semiSoft: "Fontina", hard: "Provolone" }, fries: "Sweet potato" }
```

## Relationship Between Arrays and Objects

Why isn't an `Array` a fundamental data type in JavaScript? The answer is that **it's actually a special type of `Object`**. Yes, that's right: ***`Array`s are `Object`s\***. To underscore this point, check out what the `typeof` operator returns when we use it on an `Array`

```js
typeof [];
// => "object"
```

We can set properties on an `Array` just like a regular `Object`:

```js
const myArray = [];
 
myArray.summary = 'Empty array!';
 
myArray;
// => [summary: "Empty array!"]
```

And we can modify and access those properties, too:

```js
myArray['summary'] = 'This array is totally empty.';
 
myArray;
// => [summary: "This array is totally empty."]
 
myArray.summary;
// => "This array is totally empty."
```

In fact, *everything* we just learned how to do to `Object`s can also be done to `Array`s because `Array`s **are** `Object`s. Just special ones. To see the special stuff, let's `.push()` some values into our `Array`:

```js
myArray.push(2, 3, 5, 7);
// => 4
 
myArray;
// => [2, 3, 5, 7, summary: "This array is totally empty."]
```

Cool, looks like everything's still in there. What's your guess about the `Array`'s `.length`?

```js
myArray.length;
// => 4
myArray[0];
// => 2
myArray[myArray.length - 1];
// => 7
```

**`Array`-style elements are stored separately from its `Object`-style properties**. The `.length` property of an `Array` describes how many items exist in its special list of elements. Its `Object`-style properties are not included in that calculation.

 how does the JavaScript engine know whether it should be an `Object`-style property or an `Array`-style element?

```js
const myArray = [];
 
myArray[0] = 'Will this be an `Object` property or an `Array` element?';
// => "Will this be an `Object` property or an `Array` element?"
 
// Moment of truth...
myArray.length;
// => 1
```

What happens if we enclose the integer in quotation marks, turning it into a string?

```js
myArray['0'] = 'What about this one?';
// => "What about this one?"
 
myArray.length;
// => 1
 
myArray;
// => ["What about this one?"]
```

**all keys in `Object`s and all indexes in `Array`s are actually strings**. In `myArray[0]` we're using the integer `0`, but under the hood the JavaScript engine automatically converts that to the string `"0"`. When we access elements or properties of an `Array`, the engine routes all integers and integers masquerading as strings (e.g., `'14'`, `"953"`, etc.) 

```js
const myArray = [2, 3, 5, 7];
 
myArray['1'] = 'Hi';
// => "Hi"
 
myArray;
// => [2, "Hi", 5, 7]
 
myArray['01'] = 'Ho';
// => "Ho"
 
myArray;
// => [2, "Hi", 5, 7, 01: "Ho"]
 
myArray[01];
// => "Hi"
 
myArray['01'];
// => "Ho"
```

After adding our weird `'01'` property, the `.length` property still returns `4`:

```js
myArray.length;
// => 4
```

So it would stand to reason that `Object.keys()` would only return `'01'`, right?

```js
Object.keys(myArray);
// => ["0", "1", "2", "3", "01"]
```

Unfortunately not. The reason why `Array`s have this behavior would take us deep inside the JavaScript source code, and it's frankly not that important. Just remember these simple guidelines, and you'll be just fine:

- **For accessing elements in an `Array`, always use integers**.
- **Be wary of setting `Object`-style properties on an `Array`**. There's rarely any reason to, and it's usually more trouble than it's worth.
- **Remember that all `Object` keys, including `Array` indexes, are strings**. This will really come into play when we learn how to iterate over `Object`s, so keep it in the back of your mind.





# Reading Questions

1. Identify JavaScript `Object`s

   > JavaScript `Object`s are a **collection** of **properties** bounded by curly braces (`{ }`). The properties can point to values of any data type — even other `Object`s.

2. How do you Access a value stored in an `Object`

   > *dot notation*, we use the *member access operator* (a single period)  **Obj.property** or bracket notation **address['street2'];**

3. How do you Add a property to an `Object`

   > ```js
   > const circle = {};
   >  
   > circle.radius = 5;
   > ```
   >
   > Since {}, represents an object we can just set something equal to  it
   >

4. How do you Use convenience methods like `Object.assign()` and `Object.keys()`

   > ```js
   > Object.assign({ eggs: 3 }, { flour: '1 cup' });
   > Object.keys(wednesdayMenu);
   > ```
   >
   > 

5. Remove a property from an `Object`

   > ```js
   > delete wednesdayMenu.salad;
   > ```
   >
   > Easy as that

6. Identify the Relationship Between Arrays and Objects

   > Arrays are actually a special type of `Object` but, `Array`-**style elements** are stored **separate**ly from its `Object`**-style properties**

# LAB Solution 

```js
const driver = {}

function updateDriverWithKeyAndValue(driver, key, value) {
  return Object.assign({}, driver, { [key]: value });
}
//The spread operator ... spreads each item into a seperate piece of data such as
//string = 'abc', array = [], array.push(...string) 
//returns#=>['a', 'b', 'c']

function destructivelyUpdateDriverWithKeyAndValue(driver, key, value) {
    driver[key] = value
    return driver
}

function deleteFromDriverByKey(driver, key) {
  const newDriver = Object.assign({}, driver)
  delete newDriver[key]
  return newDriver
}

function destructivelyDeleteFromDriverByKey(driver, key) {
  delete driver[key]
  return driver
}
```

