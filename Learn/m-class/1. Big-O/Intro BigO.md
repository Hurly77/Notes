[TOC]



# Intro to Big O

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