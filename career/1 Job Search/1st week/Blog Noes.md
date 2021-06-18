# Blog Notes (How Node.js Works)

## Understanding JS Runtime

To understand the basics of a **runtime** first we need to understand what **BLOCKING** is?

**Blocking** ([Wikipedia](https://en.wikipedia.org/wiki/Blocking_(computing))) is and instance of a computer program that is being executed, A process always exists in exactly one process stat. A process that is blocked is one that is waiting for some event such as a resource becoming available or the completion of an I/O

**Non-Blocking** - Is accentually the **opposite** of what **Blocking** code is, instead of waiting for the event to complete it goes to the next line and continues to run then when all synchronous code has finished executing will come back to the async code.

So now that we have a grasp on what blocking means, and if not that is okay we’ll dive a little deeper on that later we can discuss what a **RUNTIME** is

a [**Runtime**](https://blog.stackpath.com/runtime/) - is a system primarily in software development to describe the period of time during which a program is running.

Now that is all great and all but what does that even mean? well first it is good to note that runtime is similar to a **Library** well technically it is, however the difference is that library usually reference an external resource that use in tandem of the code that your writting in such as **react** and **JS**

**[! IMPORTANT  !]** :One very Important detail about **Runtime** is the final phase of the program lifecycle in which the machine executes the program’s code.

> The other phases include:
>
> - **Edit time** – When the source code of the program is being edited. This phase includes bug fixing, [refactoring](https://www.altexsoft.com/blog/engineering/code-refactoring-best-practices-when-and-when-not-to-do-it/), and adding new features.
> - **Compile time** – When the source code is translated into machine code by a compiler. The result is an executable.
> - **Link time** – When all the necessary machine code components of a program are connected such as external libraries. [These connections](http://cs-fundamentals.com/tech-interview/c/difference-between-static-and-dynamic-linking.php) can be made by the compiler (called static linking) or by the operating system (called dynamic linking).
> - **Distribution time** – When a program is transferred to a user as an executable or source code. Most of the time a program is downloaded from the Internet but it can also be distributed via CD or USB drive.
> - **Installation time** – When the distributed program is being installed on the user’s computer.
> - **Load time** – When the operating system places the executable in active memory in order to begin execution.
>
> ## Key Takeaways
>
> - Runtime is the **phase of the program lifecycle** that executes and keeps a program running; other phases include edit time, compile time, link time, distribution time, installation time, and load time.
> - Developers often test their programs in runtime environments (RTE) before moving to production in order to check for performance glitches and runtime errors.
> - **Runtime errors** can be customized in most languages; in JavaScript, a custom message can be displayed with a standard error block by using throw.