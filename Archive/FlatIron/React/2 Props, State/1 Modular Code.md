## Modularizing React Code

React makes the modularization of code easy by introducing the component structure.

```JSX
class Hogwarts extends React.Component {
    render() {
        return (
            <div className="Hogwarts">
                "Harry. Did you put your name in the Goblet of Fire?"
            </div>
        );
    }
}
```

 It is **not uncommon** to see a React program file tree that looks something like this:

```
├── README.md
├── public
└── src
     ├── App.js
     ├── Hogwarts.js
     └── Houses.js
```

## Import and Export

On a simplified level, `import` and `export` enable us to use code from one file in other locations across our projects. Let's look at how we can do this.

#### Export

We can only use `export default` once per module. The syntax allows us to disregard naming conventions when we want to import the given module.

For example:

```jsx
// src/houses/HagridsHouse.js
import React from 'react';
 
function whoseHouse() {
    console.log(`HAGRID'S HOUSE!`);
}
 
export default whoseHouse;
```

We can then use `import` to make use of that function elsewhere. `export default` allows us to name the exported code whatever we want when importing it. For example, `import nameThisAnything from './HagridsHouse.js'` will provide us with the same code as `import whoseHouse from './HagridsHouse.js'` -**- which is called aliasing!**

```jsx
// src/Hogwarts.js
import whoseHouse from './HagridsHouse.js'
import ReactDOM from 'react-dom'
 
render() {
  return (
    whoseHouse()
    // > `HAGRID'S HOUSE!`,
    document.getElementById('root')
  )
}
```

If we can `export default` functions, we can `export default` components! like so...

```jsx
// src/houses/Hufflepuff.js
import React from 'react';
 
export default class Hufflepuff extends React.Component {
    render() {
        return <div>NOBODY CARES ABOUT US</div>;
    }
}
```

Then, we can import the entire component to any other file in our application, using whatever naming convention that we see fit:

```jsx
// src/Hogwarts.js
import React from 'react';
import HooflePoof from './houses/Hufflepuff.js';
 
export default class Hogwarts extends React.Component {
    render() {
        return (
            <div>
                <HooflePoof />
                //> Will render `NOBODY CARES ABOUT US`, even though we renamed `Hufflepuff`
                // to `HooflePoof`
            </div>
        );
    }
}
```

You will commonly see a slightly different way of writing this:

```jsx
// src/Hogwarts.js
import React from 'react'
import HooflePoof from './houses/Hufflepuff.js'
 
class Hogwarts extends React.Component{
  ...
}
 
export default Hogwarts
```

Moving the `export default` to the bottom can make it easier to find exactly what a file is exporting.

**Named Exports**

Named exports allow us to export several specific things at once

```jsx
// src/houses/Gryffindor.js
export function colors() {
    console.log('Scarlet and Gold');
}
 
function values() {
    console.log('Courage, Bravery, Nerve and Chivalry');
}
 
export function gryffMascot() {
    console.log('The Lion');
}
```

We will go into detail on the `import` line in just a moment, but briefly: `import * from './houses/Gryffindor.js'` imports everything from `./houses/Gryffindor.js` that is *exported*.

We can also move named exports to the bottom of a file:

```jsx
// src/houses/Gryffindor.js
function colors() {
  console.log("Scarlet and Gold")
}
 
function values() {
  console.log("Courage, Bravery, Nerve and Chivalry")
}
 
function gryffMascot() {
  console.log("The Lion")
}
 
export {
  colors,
  gryffMascot
}
```

## Import

#### import * from

`import * from` imports all of the functions that have been exported from a given module. This syntax looks like:

```jsx
// src/Hogwarts.js
import * as GryffFunctions from './houses/Gryffindor.js';
 
GryffFunctions.colors();
// > 'Scarlet and Gold'
```

We have the option to rename the module when we `import` it, as we did above. However, importing all of the functions by name is also an option:

```jsx
// src/Hogwarts.js
import * from './houses/Gryffindor.js'
 
colors()
// > 'Scarlet and Gold'
```

#### import {function()} from

`import { function() } from` allows us to grab a specific function by name, and use that function within the body of a new module.

We're able to reference the function imported by its previously declared name:

```jsx
// src/Hogwarts.js
import { colors } from './houses/Gryffindor.js';
import { gryffMascot } from './houses/Gryffindor.js';
 
colors();
// > 'Scarlet and Gold'
 
gryffMascot();
// > 'The Lion'
```

...or rename it inside of our `import` statement:

```jsx
// src/Hogwarts.js
import { colors } from './houses/Gryffindor.js';
import { gryffMascot as mascot } from './houses/Gryffindor.js';
 
colors();
// > 'Scarlet and Gold'
 
mascot();
// > 'The Lion'
```

## Importing Node Modules

```jsx
// src/Hogwarts.js
 
import React from 'react';
import Gryffindor from './houses/Gryffindor';
import Ravenclaw from './houses/Ravenclaw';
import Hufflepuff from './houses/Hufflepuff';
import Slytherin from './houses/Slytherin';
 
export default class Hogwarts extends React.Component {
    render() {
        return (
            <div>
                <Gryffindor />
                <Ravenclaw />
                <Hufflepuff />
                <Slytherin />
            </div>
        );
    }
}
```

`import React from 'react'`. Here, we are referencing the React library's default export. The React library is located inside of the `node_modules` directory, a specific folder in many Node projects that holds packages of third-party code.