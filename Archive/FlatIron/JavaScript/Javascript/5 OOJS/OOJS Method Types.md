## Standard Methods

Most `class` methods you will see use the following, standard syntax:

```js
area() {
  return this.sideLength * this.sideLength;
}
```

Methods can be called from inside other methods just like properties:

```js
class Square {
  constructor(sideLength) {
    this.sideLength = sideLength;
  }
 
  area() {
    return this.sideLength * this.sideLength;
  }
 
  areaMessage() {
    return `The area of this square is ${this.area()}`;
  }
}
square.area(); // => 25
square.areaMessage(); // => LOG: The area of this square is 25
```

## Static Methods

Static methods are `class` level methods - they are not callable on instances of a `class`, only the `class` itself. These are often used in 'utility' `class`es - `class`es that **encapsulate** a set of related methods but don't need to be made into instances. For example, we could write a `CommonMath` `class` that stores a series of math related methods:

example, we could write a `CommonMath` `class` that stores a series of math related methods:

```js
class CommonMath {
  static triple(number) {
    return number * number * number;
  }
 
  static findHypotenuse(a, b) { //don't use key word this to interact with an instance of class
    return Math.sqrt(a * a + b * b);
  }
}
```

To access, these static methods:

```js
let num = CommonMath.triple(3);
num; // => 27
let c = CommonMath.findHypotenuse(3, 4);
c; // => 5
```

## Define `get` Keyword in JavaScript Class Context

The `get` keyword turns a method into a 'pseudo-property', that is - it allows us to write a method that interacts like a property. To use `get`, write a `class` method like normal, preceded by `get`:

```js
class Square {
  constructor(sideLength) {
    this.sideLength = sideLength;
  }
 
  get area() {
    return this.sideLength * this.sideLength;
  }
}

let square = new Square(5);
square.sideLength; // => 5
square.area; // => 25
 //As a result of this, area will now be available as though it is a property just like sideLength:


```

If you try to use `this.area()`, you'll receive a TypeError - `area` is no longer considered a function!

we could:

```js
class Square {
  constructor(sideLength) {
    this.sideLength = sideLength;
    this.area = sideLength * sideLength;
  }
}
```

This is valid code, **but** what we've done is load our `constructor` with more **calculations**.

If included in the `constructor`, an expensive process will be called every time a new instance of a `class` is created. When dealing with many instances, this can result in decreased performance.



Using `get`, an expensive process can be delayed - only run when we need it, distributing the workload more evenly.

`get` is useful in general when deriving or calculating data from properties. Since properties can change, any values dependent on them should be calculated based on the current property values, otherwise we will run in to issues like this:

```js
class Square {
  constructor(sideLength) {
    this.sideLength = sideLength;
    this.area = sideLength * sideLength;
  }
}
let square = new Square(5);
square.area; // => 25
square.sideLength = 10;//sideLength should return 5
square.area; // => 25 
```

## Define `set` Keyword in JavaScript Class Context

`get` it is only used for retrieving data from an instance. To change data, we have `set`

The `set` keyword allows us to write a method that interacts like a property being assigned a value. By adding it in conjunction with a `get`, we can create a 'reassign able' pseudo-property.

For example, to make `area` fully act like a real property, we create both `get` and `set` methods for it:

```js
class Square {
  constructor(sideLength) {
    this.sideLength = sideLength;
  }
 
  get area() {
    return this.sideLength * this.sideLength;
  }
 
  set area(newArea) {
    this.sideLength = Math.sqrt(newArea);
  }
}
//We can now 'set' the pseudo-property, area, and modify this.sideLength based on a reverse of the calculation we used in get:
let square = new Square(5);
square.sideLength; // => 5
square.area; // => 25
square.area = 64;
square.sideLength; // => 8
square.area; // => 64
//We can now interact with area as though it is a modifiable property, even though area is derived.
```

applying conditional statements:

```js
set area(newArea) {
  if (newArea > 0) {
    this.sideLength = Math.sqrt(newArea)
  } else {
    console.warn("Area cannot be less than 0");
  }
}
```

#### Using `get` and `set` with 'Private' Properties

 With `get` and `set`, we can define the 'public' facing methods for updating a 'private' property:

```js
class Square {
  constructor(sideLength) {
    this._sideLength = sideLength;
  }
 
  get sideLength() {
    this._sideLength;
  }
 
  set sideLength(sideLength) {
    this._sideLength = sideLength;
  }
}
```

A square's side can't have negative length. Now with our pseudo-property in place, we write code to make sure that `_sideLength` is always valid, both when an instance property is created and when it is modified:

```js
class Square {
  constructor(sideLength) {
    if (sideLength > 0) {
      this._sideLength = sideLength;
    } else {
      throw new Error('A Square cannot have negative side length');
    }
  }
 
  get sideLength() {
    this._sideLength;
  }
 
  set sideLength(sideLength) {
    if (sideLength > 0) {
      this._sideLength = sideLength;
    } else {
      throw new Error('A Square cannot have negative side length');
    }
  }
}
```

`Student` class. We are tasked with making sure names do not have any non-alphanumeric characters except for those that appear in names. This is sometimes referred to as *sanitizing* text.

With `set`, we can make sure that we sanitize input text both when an instance is created as well as later, if the property needs to change:

```js
class Student {
  constructor(firstName, lastName) {
    this._firstName = this.sanitize(firstName);
    this._lastName = this.sanitize(lastName);
  }
 
  get firstName() {
    return this.capitalize(this._firstName);
  }
 
  set firstName(firstName) {
    this._firstName = this.sanitize(firstName);
  }
 
  capitalize(string) {
    // capitalizes first letter
    return string.charAt(0).toUpperCase() + string.slice(1);
  }
 
  sanitize(string) {
    // removes any non alpha-numeric characters except dash and single quotes (apostrophes)
    return string.replace(/[^A-Za-z0-9-']+/g, '');
  }
}
 
let student = new Student('Carr@ol-Ann', ')Freel*ing');
student; // => Student { _firstName: 'Carrol-Ann', _lastName: 'Freeling' }
 
student.firstName = 'Hea@)@(!$)ther';
student.firstName; // => 'Heather'
```

## When to Use Methods Over `get` and `set`

. **We can use `get` and `set` whenever we are handling input or output of a `class`**

Using this design, all remaining methods can be considered *private*. They don't deal with input and output; they are only used internally as helper methods. It is **important to note** that in JavaScript currently, we can *always* order off the menu. All `class` methods and properties are exposed for use 'publicly'. Using `get` and `set` in this way is purely design. In designing this way, however, we produce better organized, easier to understand `class`es.

# lab

```JS
class Circle {
	constructor(radius) {
		this.radius = radius;
	}

	get diameter() {
		return this.radius * 2;
	}

	set diameter(newR) {
		this.radius = newR / 2;
	}

	get circumference() {
		return Math.PI * this.diameter;
	}

	set circumference(newD) {
		this.diameter = newD / Math.PI;
	}

	get area() {
		return Math.PI * this.radius ** 2;
	}

	set area(newArea) {
		newArea = Math.PI * newArea ** 2;
	}
}
```

