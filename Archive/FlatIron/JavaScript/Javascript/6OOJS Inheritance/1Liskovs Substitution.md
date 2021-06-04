# Recognize the Meaning of Strong Behavioral Subtyping

**Liskov's substitution principle:** Objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program.

In terms to JavaScript inheritance, **an instance of a parent class should be replaceable with an instance of a child class.**

Below are two examples, one that violates Liskov's principle, and one that upholds it:

#### Violates Substitution Principle:

```js
class Reptile {
    constructor(name) {
        this.name = name;
    }
    get move() {
        return `${this.name} crawls away`;
    }
}
 
// Lizard inherits `move` because it crawls
class Lizard extends Reptile {}
 
//  Snake overrides `move` because it cannot crawl
class Snake extends Reptile {
    get move() {
        return `${this.name} slithers away`;//overrides the move getter because the original doesn't apply
        //THIS DOES NOT UPHOLD (LISKOV'S)-PRINCIPLE
    }
}

let tricky = new Reptile('Tricky');
let basilisk = new Snake('Basilisk');

//We see that tricky cannot be replaced with basilisk without changing behavior

tricky.move; // => "Tricky crawls away"
basilisk.move; // => "Basilisk slithers away"
```



#### Upholds Substitution Principle:

we can create a grandparent `Reptile` class. This grandparent class can still contain all shared data and behavior for all parent and child classes. Since the definition of `move` is *not* shared by all, we can move these definitions down a level, defining them in two new parent classes:

```js
// all reptiles have a name
class Reptile {
    constructor(name) {
        this.name = name;
    }
}
 
// legless reptiles slither
class LeglessReptile extends Reptile {
    move() {
        return `${this.name} slithers away`;
    }
}
 
// legged reptiles crawl
class LeggedReptile extends Reptile {
    move() {
        return `${this.name} crawls away`;
    }
}
 
class Lizard extends LeggedReptile {}// inherits from GRANDPARENT-CLASS ie.REPTILE but is the CHILD-Class of LegLessReptile-CLASS.
class Snake extends LeglessReptile {}
```

> **Why does this work**? We've removed some of behavior of `Reptile`. An instance of `Reptile` will never be required to utilize a method it hasn't defined or inherited.

## Recognize the Benefits of Upholding Liskov's Substitution Principle

There is no syntax error if you choose to ignore Liskov's substitution principle. This is considered a purely *semantic* design choice.

**The benefit** of following this principle is that **no matter** how complicated inheritance gets, you can always assume that whatever a parent class has, a child class will have too.

If we have **chains of inheritance** where **children** fundamentally **change** the **data** and behaviors they inherit, we **can potentially introduce bugs.**

> **Note:** In general, when dealing with inheritance, the fewer levels of inheritance, the better. If you've got great grandparent, grandparent, parent and child classes, it can be difficult to figure out which class contributes what to a child. It also makes our code more difficult to change. You may not be able to modify code on a grandparent class without fundamentally changing how a parent or child class functions. Too much inheritance can make our code inflexible.