# JS Event Loop.

[TOC]

## Section1

> section 1 [source](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/) 
>
> ### What is the Event Loop?
>
> 
>
> ### Event Loop Explained
>
> 
>
> 





# Blog (First Draft)

The first thing we need to know about JavaScript is that it is a non-blocking language. If you are new to JavaScript, you might wonder what the heck (NON-BLOCKING) means? Let us start with this. **Blocking** is a form of code execution that will explicitly execute each line of code sequentially and not move on to the next process until the first process is finished. Well, NON-Blocking is pretty much the opposite. It means that if you have two functions on the call stack, JS won't wait for the first process to finish before moving on. You've quite possibly heard about asynchronous vs. synchronous code. JS is both async and synch. Let's look at an example to make it more clear

```js
function add(a, b) {
  return a + b
}

function average(a, b){
  return add(a, b) / 2
}

console.log(average(10, 20))
//=> 15
```

As you can see JavaScript returns 15 but how does that work. Before JS executes the stack would look something like this

1. add()
2. average()
3. console.log(average())
4. main()

If you don't already know `main()` is the whole program or file call. JavaScript will then execute from the top bottom. The process here is called LIFO(last in, first out). You may have noticed that `add()` is both “last out” as well as our first line of code. Because during the first pass through JavaScript is compiled from bottom to top. Then during the Runtime phase of our program lifecycle, the previous line of code is actually the first line of code in the Stack do to the way it was compiled. 







### checklist

- [ ] Rhetoric conversation (runtime)  
- [ ] 

### Definitions

**non-blocking I/O operations**