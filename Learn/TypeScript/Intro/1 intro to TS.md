# The Problem with JavaScript

When we want to retrieve the value of an input value for and HTML element we regardless of the input type would retrieve a string. Take for example


```js
const button = document.querySelector("button");
const input1 = document.getElementById("num1");
const input2 = document.getElementById("num2");
//Because when any html.value will be a string if we have to seperate inputs will get back a two numbers combinded.
// so if num1 was 100 and num two was 12 we'd get 10012.
function add(num1, num2) {
  return num1 + num2;
}

button.addEventListener("click", function() {
  console.log(add(input1.value, input2.value));
});
```

So when we add out event listener to `button` we will find that in the console that `num1 + num2` will give us a sting like this `"num1num2"` because `html.value` will return a string.

Now the purpose of using typescript isn't to replace JavaScript it is to add an extra layer of protection against errors.

So if we install typescript on our local maching like so 

```
npm install -g typescript
```

Then care a new file lets call it `my-ts.ts` then we add in our about JavaScript

```typescript
const button = document.querySelector('button');
const input1 = document.getElementById(
	'num1',
)! as HTMLInputElement;
const input2 = document.getElementById(
	'num2',
)! as HTMLInputElement;

function add(num1: number, num2: number) {
	return num1 + num2;
}

button.addEventListener('click', function () {
	console.log(add(+input1.value, +input2.value));
});
```

Instead of just create args in side the function call we can do "**Type checking**."

In side the function body of our event listener we prefixed our two arguments with `+` which will turn stings into integers. then once we've caught any potentiol errors we can run.

```
tsc our.ts
```

which will generate a JavaScript file with the same name as our Type Script file and alert us of any potential errors that may have been caught.

