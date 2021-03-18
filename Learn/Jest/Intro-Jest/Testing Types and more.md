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

## Testing Library and Accessibility

- Testing Library recommends finding elements by accessibility handles.
  - https://testing-library.com/docs/guide-which-query/
- create-react-app’s example test users `getByText` 
  - ok for non-interactive elements
  - better: `getByRole`
- Roles documentation: https://www.w3.org/TR/wai-aria/#role_definitions
  - some elements have built-in roles: `button, a` 
- Can’t find an element like a screen reader would?]
  - Then your app isn’t friendly to screen readers
- Much more about queries and roles latter

