# An Introduction to JavaScript

prologue

The indended purpose of this article is to provide a guide for poeple who have either just begun thier journey into JavaScript or are considering starting JavaScript.



## OOP

oop is stands for (Object, Orientated, Programming) which by technicality JavaScript is not. However, you can pretend to use OOP with JavaScript and applying the S.O.L.I.D principles. OOJS or Object-Orientated-JavaScript is a bit of a touchy subject in development world. There are plenty of developers who swear by and a plenty more who scuffle at the very Idea. The controversy makes alot  of since when the think about how JavaScript is really just a C++ application. It is quite a paradox and can sometimes be confusing for people but long story short JS can't be object-orientated because it is the production of an object-orientated language it would kind of defeat the whole purpose and we acenctially have a full loop for one language to next just with a ton of performance lost. But it is very benificial to acknowledge the advantages OOP practices can have when programming with JavaScript.

## resources

If you want to be a good JavaScript engineer you have to learn you resources, and for any engineering for that matter. Now I do want to give a fair amount of options on Resources. I find it very helpful to watch video tutorials but, that fact remains you won't learn from them because all the hard work has already been done for you. To truly understand a technology you have to get into the weeds and look over the documentation. What makes a good developer is the ablity to solve problems unfortunately you will never learn or have a enough information in you head to have and know all the information and solutions to most problems. You have to learn how to dynamically problem solve. What does this mean, It means you have to constantly practice the art of Taking in new information, to solve a problem you've never faced. The best developers out there are the ones that can easily be tasked to solve a problem they've never faced using new information. With out this skill it won't matter how many tools/languages you learn you won't get any better at programming. 

## Function Programing,

So JavaScript by technicality isn't a OOP language, nope it is a scripting language hence the "Script" part of JavaScript. Now you can like I had discussed before use OOP practices in JavaScript but some time the lines between OOP and Function programming can get crossed on account of JS not being really OOP language and can easily turn to function programming. so What is function programming. Function programming is exactly what it sounds like, you build functions that sever a purpose like adding two numbers.

```js
class Animal {
  constructor({id, name}) {
    this.id = id;
    this.name = name;
  }
}

class Animals {
  constructor(array = []) {
    this.all = array;
  }

  create(...args) {
    let animal = new Animal(...args);
    this.all.push(animal);
    return animal;
  }

  fromName(name) {
    return this.all.filter((animal) => {
      if (animal.name === name) {
        return animal;
      }
    });
  }
}

let animalArr = [
  new Animal({id: 1, name: "Dragon"}),
  new Animal({id: 2, name: "Unicorn"}),
  new Animal({id: 3, name: "Hawk"}),
];

let animals = new Animals(animalArr);
console.log(animals.fromName("Hawk"));
//=> [ Animal { id: 3, name: 'Hawk' } ]



//Functional programing
let animalArr2 = [
  {id: 1, name: "Dragon"},
  {id: 2, name: "Unicorn"},
  {id: 3, name: "Hawk"},
];

const funcPro = {
  fromName: (arr, name) => {
    return arr.filter((animal) => {
      if (animal.name === name) {
        return animal;
      }
    });
  },
};

console.log(funcPro.fromName(animalArr2, 'Hawk'))

//=> [ { id: 3, name: 'Hawk' } ]
```

For Example

## function Types

## Data Types,