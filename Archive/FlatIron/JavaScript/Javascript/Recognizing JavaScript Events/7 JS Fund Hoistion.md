## How Function and Variable Declarations Are Hoisted

JavaScript file from top-to-bottom, it would make sense if we had to define a function before we invoked it:

```js
function myFunc () {
  return 'Hello, world!';
}
 
myFunc();
// => "Hello, world!"
```

However, we can invert those two steps and everything works fine:

```js
myFunc();
 
function myFunc () {
  return 'Hello, world!';
}
// => "Hello, world!"
```

Java Script Engine**During the compilation phase**, the engine **skips** right over the **invocation** and **stores** the declared **function** in memory:

```js
// The engine ignores all function invocations during the compilation phase.
myFunc();
 
function myFunc () {
  return 'Hello, world!';
}
```

```js
// During the execution phase, the engine invokes myFunc(), which was already initialized during the compilation phase.
myFunc();
 
// During the execution phase, the engine will simply ignore this function declaration that was already carried out in the compilation phase.
function myFunc () {
  return 'Hello, world!';
}
```

The engine **starts over** at the top of the code and begins executing it **line-by-line**:

***Top Tip\***: The best way to avoid any confusion brought on by function hoisting is to simply declare your functions at the very top of your code.

## Best Variable Declaration Practices

use `const` and `let` and you'll have no variable hoisting issues.

```js
function myFunc () {
  console.log(hello);
 
  var hello = 'World!';
}
// => undefined
```

In JavaScript, hoisting only applies to variable *declarations*; not variable *assignments*. As a quick refresher on that terminology:

```js
// Declaration:
let hello;
 
// Assignment:
hello = 'World!';
 
// Declaration and assignment on the same line:
let goodnight = 'Moon';
```



## If, for whatever reason, your current project requires that you use `var`

follow our rule for function declarations and declare everything at the **top** of its scope. For example, if you need to declare a variable within a function, declare it at the **top** of that function:

```js
// BAD
function myBadFunc () {
  console.log('Just doing some other stuff before we get around to variable declarations.');
 
  var myVar = 42;
}
 
// GOOD
function myGoodFunc () {
  var myVar = 42;
 
  console.log("Much better! The variable declaration is at the top of the scope created by 'myGoodFunc()', so there's no chance it gets 'hoisted'.");
}
```

# lab Task Lister Lite

```js
document.addEventListener("DOMContentLoaded", () => {
  // your code here  
  let form = document.getElementById("create-task-form");
  let ul = document.getElementById("tasks");
  let task = document.getElementById("new-task-description");

  
  function addTask(e){
    let li = document.createElement('li')
    ul.appendChild(li).innerHTML = `${task.value} `
    let button = document.createElement('button')
    li.appendChild(button).innerHTML = 'x';
    e.target.reset();
    e.preventDefault()
  }

  ul.addEventListener("click", function(e){
    let tgt = e.target;
    if (tgt.tagName === "BUTTON") {
      tgt.parentNode.remove();
    }
  });

  form.addEventListener("submit", addTask);
});

```

# lab DOM Challenge