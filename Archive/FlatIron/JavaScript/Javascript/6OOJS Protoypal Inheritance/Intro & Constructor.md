## Recognize that the Inheritance Model of JavaScript is Prototypal

> **IMPORTANT**: This is a common interview question used to weed-out front end developers. When asked what kind of inheritance model JavaScript has, boldly and proudly say "Prototypal!" To demonstrate mastery of this vocabulary word, you'll need to do the rest of the work in this section, though. :)

 **prototype-based** models are ways of describing how to create instances from a common ancestor. Neither is "better" than the other. They're just different ways of seeing objects (no pun intended) in the world.

# **OOJS: Constructor Functions**

## Create a Constructor Function

we could represent a user as:

```js
const bobby = {name: 'bobby', age: 20, hometown: 'Philadelphia'}

//add more
const bobby = {
  name: 'bobby',
  age: 20,
  hometown: 'Philadelphia'
}
 
const susan = {
  name: 'susan',
  age: 28,
  hometown: 'Boston'
}
```

**Note**, that with both objects sharing exactly the same keys, and only the values differing, **we are** *[repeating ourselves](https://en.wikipedia.org/wiki/Don't_repeat_yourself)*.

We call the mechanism that constructs objects with key => value; pairs a **factory function** because it spits out new instances

```JS
function User(name, age, hometown) {
    return {
    name, // don't forget ES6 power-tools, this is the same as `name: name`
    age,
    hometown,
  }
}
 
let byronPoodle = User("Karbit's Byron By the Bay", 5, "Manhattan")
byronPoodle.age // => 5
> typeof byronPoodle
'object'

// if we wanted to see what made our OBJ
> byronPoodle.constructor
[Function: Object]
```

## Explain What `new` Is and How It Works With the Constructor Function

Constructor functions must be paired with the `new` keyword

```js
function User(name, email) {
    this.name = name;
    this.email = email;
}
//Now with more comments
--------------------------------------------------------------------
function User(name, email) {
    this.name = name;   // [2] Set my context's property name to what
                      //     came in in the first argument (name)
    this.email = email; // [3] Set my context's property email to what
                      //     came in in the second argument (email)
}
 
// [1] Create a new "context", that's what `new` does
// Use _that_ new context inside of the execution of the `User` function
// also pass two parameters, "Lauren" and "lauren@example.com"
 
// [4]: Assign the new context thing with its this properties set to the
// variable `lauren`
let lauren = new User("Lauren", "lauren@example.com");
 
// [5]: Ask the new context for what's in its `.name` property
lauren.name //=> [6] "Lauren"

//you can also ask what Type Of is  
> typeof lauren
'object'

//what constructed OBJ
> lauren.constructor
[Function: User]

```

if we can set a property to point to a value like `"Lauren"` or `"lauren@example.com"`, we should be able to set an anonymous function to a property. That function would have access to the `this` context created when the instance was `new`'d into existence.

```JS
function User(name, email) {
    this.name = name;
    this.email = email;
    this.sayHello = function() {
        console.log(`Hello everybody, my name is ${this.name} whom you've been
mailing at ${this.email}!`);
    };
}
 
let lauren = new User('lauren', 'lauren@example.com');
lauren.sayHello(); //=> Hello everybody, my name is lauren whom you've been mailing at lauren@example.com!
```

# lab

```JS
function Scooter(year, color, model){
  this.year = year
  this.color = color
  this.model = model
}

function Driver(name, age, experience){
  this.name = name
  this.age = age 
  this.experience = experience
}

function PickupLocation(address, city){
  this.address = address 
  this.city = city
}
```

