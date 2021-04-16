# execution context

## Execution Context

In JavaScript all we really do is assign variables and run functions. First we Write a function like so.

```js
const myFunc = () => {
	return "This my function using funtional expression"
}

const yourFunc = () => {
	return "This is another function"
}

const lastFunc = () => {
    return "This is the last Function"
}

```

Then we simply execute the function with the `()` syntax. This allows us to tell JavaScript “hey execute this function”. that way if we want to pass a certain function but not execute it we can simply just not call it with the `()` that way if we can pass it to other functions with out calling it a receiving any the return value that function produces. 

```jsx
const runFunctionOnArray = (funcArr, func) => {
    //now we can compare our function to and array of functions to see if it exist within an array
	for(let i = 0; i < funcArr; i++){
		if(funcArr[i] === func){
			return true
		}
	}
    return false
}
let array = [myFunc, myFunc2, myFunc3]

runFunctionOnArray(array, myFunc)
//=> true
```

When we use the `()` we create a execution context, which means that what ever is return by this function is the context. for example

```js
const func1 = () =>{
	return "hello World"
}

const func2 = () =>{
	return func1 ()
}

const func3 = () =>{
	return func2()
}

const func4 = () =>{
	return func3()
}

const executeAll = () => {
    return func4()
}

executeAll()
//=> "hello World"
```

First we call the function popped on to the stack will be `executeAll()` which will have and execution context  of `func4` and so on until we reach “hello world” which will then be returned by the `executeAll()` function. This is made possible by what is know as the *GLOBAL EXECUTION CONTEXT*. Whenever you create a new HTML document JavaScript automatically creates a global object and the `this` keyword. 

Whatever functions you define inside your JavaScript file such as a `index.js` will become the execution context for the global object. So when the JavaScript Engine runs a Global execution context is created and when we run a function a new execution context will be created for that function.

