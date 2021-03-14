[TOC]



# TDD (Test-Driven Development)

- Write tests before  writing code
  - then write code according to “spec” set by tests
- “red-green” testing
  - Tests fail before code is written

![image-20210313212419137](C:\Users\camer\AppData\Roaming\Typora\typora-user-images\image-20210313212419137.png)

Why TDD?

- Makes a huge difference in how it feels to write tests
  - part of the coding process, not a “chore” to do at the end
- More efficient
  - Re-run tests “for free” after changes

- Unit tests
  - Tests on unit of code in isoloation
- Integration tests
  - How multiple units work together
- Functional Tests
  - Tests a particular function of software
- Acceptance / End-to-end (E2E) Tests
  - Use actual browser and server (Cypress, Selenium)

## Functional Testing

difference mindset from unit testing

***Unit Testing***

> 
>
> **PRO** +> Very easy to pinpoint failures
>
> 
>
> | **Unit Testing**                                      | **Functional Testing**                            |
> | ----------------------------------------------------- | ------------------------------------------------- |
> | **Con** Further from how users interact with software | **Pro** Include all relevant units, test behavior |
> | **Con** Further from how users interact with software | **Pro** Close to how users interact with software |
> | **Con** More likely to break with refactoring         | **Pro** Robust tests                              |
> |                                                       | **Con** More difficult to debug failing tests     |
>
> 

### **TDD VS BDD**

- Quick detour for BDD: Behavior-Driven Development
- Testing Library encourages testing *behavior* over implementation
- So Shouldn’t we be calling this BDD instead of TDD
  - involves collaboration between lots of roles
    - developers, QA, business partners, etc
  - defines process for different groups to interact
- in this course, only developers, so TDD!

