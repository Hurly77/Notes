[TOC]

# Intro to Big O

O = operations

**O(n)** means that the number of operations is dependent on the size of the array or length of the data type.

Lets say we have an array of n strings 

```js
//the number of strings
const operationN = (n, s) => {
	for(let i = 0; i < n.length; i++){
		if (n[i] === s) {
			break
		}
	}
	return i
}
```

O(1) means that the number of operations is linear and for each line of code exactly equal to one operation `const string = "this string"`

Measuring the performance of 

```js
const nemo = ['nemo']

const findNemo = (array) => {
	let t0 = performance.now()
   for(let i = 0; i < array.length; i++){
     if(array[i] === 'nemo') {
       console.log("Found Nemo!")
       return '><> '
     }
   }
    let t1 = performance.now()
    console.log("call to nemo took", + (t1-t0) + 'milliseconds')
}

console.log(findNemo(nemo))
```
