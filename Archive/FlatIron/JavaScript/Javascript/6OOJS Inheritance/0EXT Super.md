```JS
class Pet {
  constructor(name) {
    this._name = name;
    this._owner = null;
  }
 
  get name() {
    return this._name;
  }
 
  get owner() {
    return this._owner;
  }
 
  set owner(owner) {
    this._owner = owner;
  }
 
  get speak() {
    return `${this.name} speaks.`;
  }
}
 
// Inherits from Pet
class Dog extends Pet {
  //this is a child constructor
   constructor(name, breed) {
    super(name); /* new */ //pasing in name then stets it to this.breed. We are able to use SUPER to call the pet constructor. By doing this will set up NAME and OWNER properties
    this.breed = breed;
  }
}
 
let creature = new Pet('The Thing');
let dog = new Dog('Spot', 'Foxhound');
 
dog;
// => Dog { _name: 'Spot', _owner: null, breed: 'Foxhound' }
```

**In a child class constructor, `super`** is used as a `method` and calls the parent class constructor before continuing with the child. This lets us extend a parent's constructor inside a child. If we need to define custom behavior in a child constructor, we can do so without having to override or ignore the parent.

## Recognize How to Use the `super` Object

When outside of the constructor, the `super` keyword is used as an `object`

```Js
// Inherits from Pet
class Dog extends Pet {
  constructor(name, breed) {
    super(name); /* new */
    this._breed = breed;
  }
 
  get breed() {
    return this._breed;
  }
 
  get info() {
      //here SUPER is an OBJECT
    if (super.owner) {//This accesses the owner getter from the PARENT class PET
        //SUPER.owner == THIS.owner => TRUE;
        //though - INSTANCE methods and PROPERTIES are already INERITED form PARENT class PET
      return `${this.name} is a ${this.breed} owned by ${super.owner}`;
    }
    return `${this.name} is a ${this.breed}`;
  }
}
 
let charlie = new Dog('Charlie B. Barkin', 'Mutt');
 
dog.info;
// => 'Charlie B. Barkin is a Mutt'
 
let lady = new Dog('Lady', 'Cocker Spaniel');
lady.owner = 'Darling Dear';
 
lady.info;
// => 'Lady is a Cocker Spaniel owned by Darling Dear'
```

`super` **is useful when =>** a **parent** class contains a **static**-*method* that we want to **expand** on *in* **child**-*class*

```JS
class Pet {
  constructor(name) {
    this.name = name;
    this._owner = null;
  }
 
  get owner() {
    return this._owner;
  }
 
  set owner(owner) {
    this._owner = owner;
  }
 
  static definition() {
    return `A pet is an animal kept primarily for a person's company.`;
  }
}
 
// Inherits from Pet
class Dog extends Pet {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }
 
  static definition() {
    return (
        //expands the PARENT-class-method with SUPER in CHILD-class
      super.definition() + ' Dogs are one of the most common types of pets.'
         //we are able to use SUPER.definition() to access that STATIC-(method), then |add| to it
    );
  }
}
 
let creature = new Pet('The Thing');
let dog = new Dog('Spot', 'foxhound');
 
Pet.definition();//"A pet is an animal kept primarily for a person's company."
//extending the definition to specifically reference dogs.
Dog.definition()
//only when CHILD-CLASS.defintion()is called does it return both 
//"PARENT-STATIC-method[A pet is an animal kept primarily for a person's company.] add-from-CHILD-class [Dogs are one of the most common types of pets]."
```

# lab

```js
class Tree {
	constructor(species) {
		this.species = species;
	}
	static definition() {
		return 'A tree is a perennial plant with an elongated stem, or trunk, supporting branches and leaves.';
	}
}

class Deciduous extends Tree {
	constructor(species, name) {
		super(species);
		this.name = name;
	}

	static definition() {
		return super.definition() + ' Deciduous trees shed their leaves annually.';
	}
}

class Evergreen extends Tree {
	constructor(species, name) {
		super(species);
		this.name = name;
	}

	static definition() {
		return super.definition() + ' Evergreens keep their leaves all year round.';
	}
}

```

