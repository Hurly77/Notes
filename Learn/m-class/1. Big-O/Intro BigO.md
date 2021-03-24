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

