# **Class Extension and Inheritance Extends**

## Use the `extends` Keyword

`extends` is used in class declarations to create a class which is a *child* of another class.

```js
class Pet {
  constructor(name, sound) {
    this.name = name;
    this.sound = sound;
  }
 
  speak() {
    return `${this.name} says ${this.sound}!`
  }
}
 
class Dog extends Pet {
  // inherits constructor from Pet
}
 
class Cat extends Pet {
  // inherits constructor from Pet
}
 
class Bird extends Pet  {
  // inherits constructor from Pet
  fly() {
    return `${this.name} flies away!`
  }
}
 
let dog = new Dog("Shadow", "woof");
let cat = new Cat("Missy", "meow");
let bird = new Bird("Tiki", "squawk");
 
dog.speak(); // Shadow says woof!
cat.speak(); // Missy says meow!
bird.speak(); // Tiki says squawk!
bird.fly(); // Tiki flies away!
```

# lab

```js
// Your code here
class Polygon {
	constructor(sidesArray) {
		this.sidesArray = sidesArray;
	}
	get countSides() {
		return this.sidesArray.length;
	}

	get perimeter() {
		return this.sidesArray.reduce((acc, currVal) => {
			return acc + currVal;
		});
	}
}

class Triangle extends Polygon {
	get isValid() {
		let [ a, b, c ] = this.sidesArray;
		return a + b <= c || b + c <= a || a + c <= b ? false : true;
	}
}

class Square extends Polygon {
	get isValid() {
		let [ a, b, c, d ] = this.sidesArray;
		return a && b && c == d ? true : false;
	}

	get area() {
		return this.sidesArray[0] ** 2;
	}
}
```

# OOJS Class EXT. and Inheritance Super

## Recognize How to Use the `super` Method

