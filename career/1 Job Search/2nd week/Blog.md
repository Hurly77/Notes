# Single Thread, Event Loop & Blocking Code

It is important to keep in mind that node only uses one JS Thread.

The question here is is how do we make multiple requests?

**Event** **Loop** - the ***event loop*** does not handle the incoming file but, it does handle the `callback()` so that when our code is sent to the *(worker pool)*, the heavy lifting for our app, once the *(worker pool)* is done like when we write a file it will then trigger the `callback()` which is then handle by the ***event loop***.

**Worker Pool** the worker pool works **independently** of both the **event loop** and the **request**. this will run on a separate thread from event loop.



When the fs 

![image-20210128172300498](C:\Users\camer\AppData\Roaming\Typora\typora-user-images\image-20210128172300498.png)

**More on The Event Loop**

The event loop is run by node.js and keeps node.js running and handles all the callbacks from the Worker Pool. It does this by, and to no surprise lopping over node.js code. **KEEP IN MIND** that the Event Loop has and order in which it goes through the call backs. 

###### Event Loop Order

At the beginning of each loop the event loop

- **First**- checks to see if there are any **Timers** such as `SetTimeout, setInterval` these are all `callbacks()`

- **Then** - Event Loop will checks to see if there are and **Pending** `callbacks()` that might have been deferred. If those` callback()` operations are finish event loop would  then in turn execute said `callback()`.

- **Poll** -  after the above have been looped through, the **poll phase** will begin. This basically means that event loop will retrieve any new `I/O `events and execute their `callbacks()`. If the `callbacks()` can not be executed it will then defer that `callback()` and come back on the next iteration and checkback to see if it is ready to be executed. Also the poll phase will check if there are any Timer call backs due to be executed and if so will jump to  Timer Execution.

- **Check** - will execute **Immediate callbacks()** and will only execute after all other open callbacks have been either executed or deferred.

- **Close** - now at the end of event loop iterations Node.js will execute any `closed` event `callbacks()`.

- **Finally** - we might exit the process but only if there are any open event listeners. 

  *Poll Phase aesthetic example*

![image-20210128175416732](C:\Users\camer\AppData\Roaming\Typora\typora-user-images\image-20210128175416732.png)

​			*Poll phase aesthetic example(when checking timer callbacks())*

![image-20210128175635885](C:\Users\camer\AppData\Roaming\Typora\typora-user-images\image-20210128175635885.png)

#### Event listeners on Node sever

when we open and event listener on with NodeJS `createServer()` this is a infinite reference that will never close so our event loop will never exit.

in other words the `createServer()` method by default never exits.