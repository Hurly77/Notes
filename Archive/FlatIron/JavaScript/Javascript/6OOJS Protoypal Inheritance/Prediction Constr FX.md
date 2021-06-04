## **OOJS: Predicting Constructor Effects**

## Questions

```JS
class Rectangle {
  constructor(sideA, sideB) {
    this.sideA = sideA;
    this.sideB = sideB;
    this.area = sideA * sideB;
    this.perimeter = sideA * 2 + sideB * 2;
  }
}
```

1. Given the example `class` above, what will all four properties be if `let rectangle = new Rectangle(2,4)` is run?

2. What if `let rectangle = new Rectangle(10,-4)` is run?

   > Answer. 1
   >
   > ```JS
   > insance.sideA //=>2
   > instance.sideb //=>4
   > insantce.area //=>  8
   > instance.perimeter//=> 16
   > 
   > 
   > ```
   >
   > Answer. 2
   >
   > ```JS
   > insance.sideA //=> 10
   > instance.sideb //=> -4
   > insantce.area //=>  -40
   > instance.perimeter//=> 12
   > ```
   >
   > 

```JS
class Book {
  constructor(title, author) {
    this.title = this.titleize(title);
    this.author = this.titleize(author);
  }
 
  titleize(string) {
    let words = string.split(' ');
    for (let n = 0; n < words.length; n++) {
      words[n] = words[n].charAt(0).toUpperCase() + words[n].slice(1);
    }
    return words.join(' ');
  }
}
```

1. What would the resulting instance look like if we ran `new Book("Shawshank Redemption", "Stephen King")`?

   > ```JS
   > book.title//=> Shawshank Redemption
   > book.author//=> Stephen King
   > ```
   >
   > 

2. What about `new Book("all the pretty horses", "cormac mccarthy")`?

3. > ```JS
   > book.title //=> All The Pretty Horses
   > book.author// Cormac Mccarthy
   > ```
   >
   > 

```JS
class Transaction {}
```

1. What would happen if `let transactions = new Transaction(14.50, new Date(), "Subway tickets")` was run?

   > ```JS
   > //the class would have nothing to set the agrs too
   > transation //=> transaction {}
   > 
   > ```
   >
   > 

# **OOJS: Using Prototypes**

## Introduction

```JS
function User(name, email) {
  this.name = name;
  this.email = email;
  this.sayHello = function() {
    console.log(`Hello everybody, my name is ${this.name} whom you've been
mailing at ${this.email}!`);
  };
}
 
let lauren = new User('lauren', 'lauren@gmail.com');
lauren.sayHello(); //=> Hello everybody, my name is lauren whom you've been mailing at lauren@gmail.com!
```

Let's assume that the `name` property costs 32 bytes of storage

Let's also assume that the `email` property costs 32 bytes of space

 lastly assume that a function costs 64 bytes of space.

So to create our `lauren` instance we pay a cost of `32 + 32 + 64 = 128` bytes

 let's assume that we want to create many more `User`s - Facebook numbers of users. Lets suppose a paltry 1 million users. That would be: 128 million bytes of space or 128 Megabytes of space.

The **key** to gaining **efficiency** is the *prototype*.

## Identify Inefficiency In Constructor Function Object Orientation

In our example the `name`s vary, so we can't economize there. The `email`s vary, so we can't economize there either. But the method, `sayHello` is the same in every instance: "in my current context return a template string with this current context's values."

We would like to tell all instances of `User` that they have a shared place to find methods. That place is called the "prototype."

## Recognize the Prototype as a Means for Reducing Inefficiency

We access the prototype of a constructor function by typing the constructor function's name, and adding the attribute `.prototype`. So for `User` it's `User.prototype`. Attributes that point to functions in this JavaScript `Object` will be shared to all instances made by that constructor function.

```js
function User(name, email) {
  this.name = name;
  this.email = email;
}
 
User.prototype.sayHello = function() {
  console.log(`Hello everybody, my name is ${this.name}`);
};
 
let sarah = new User('sarah', 'sarah@example.com');
let lauren = new User('Lauren', 'lauren@example.com');
 
sarah.sayHello(); //=> // "Hello everybody, my name is sarah!"
lauren.sayHello(); //=> // "Hello everybody, my name is Lauren!"
```

The prototype is just a JavaScript `Object`.

```JS
prototype is just a JavaScript Object.

User.prototype;
// {sayHello: ƒ, constructor: ƒ}
typeof User.prototype;
// object
```

To prove the efficiency of sharing methods via prototype:

```js
lauren.sayHello === sarah.sayHello; //=> true
//however the instance themselfs are NOT
lauren === sarah//=> false
```

Your pattern for writing OO in JavaScript (Prototype-based) is the following:

```JS
function User(name, email) {
  this.name = name;
  this.email = email;
}
 
User.prototype.sayHello = function() {
  console.log(`Hello everybody, my name is ${this.name}`);
};
 
let sarah = new User('sarah', 'sarah@example.com');
```

# lab

- `veto` — returns `No, I must disagree`
- `approve` — returns `You can do that!`
- `doCharity` — returns `I like to help people.`
- `releasePressStatement` — returns `You will see great things from Scuber.`
- `sayHi` — returns `"Hi, my name is <name>. I am from <homestate>, and I was trained in <training>.`

```JS
function BoardMember(name, homeState, training){
  this.name = name
  this.homeState = homeState
  this.training = training
}
BoardMember.prototype.veto = function (){
  return `No, I must disagree`
}

BoardMember.prototype.approve = function (){
  return `You can do that!`
}

BoardMember.prototype.doCharity = function (){
  return `I like to help people.`
}

BoardMember.prototype.releasePressStatement = function (){
  return `You will see great things from Scuber.` 
}

BoardMember.prototype.sayHi = function (){
  return `Hi, my name is ${this.name}. I am from ${this.homeState}, and I was trained in ${this.training}.`
}
```

