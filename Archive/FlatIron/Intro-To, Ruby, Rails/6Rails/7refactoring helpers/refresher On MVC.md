## Separation of Concerns

- `View`: what an end user on a website experiences when interacting with a program in their browser: what they read, what they click, what flashes at them, and what they hear (despite the name "view"). In technology-speak, the vocabulary word would be *interface*
- `Model`: where the actual data, (be it information about restaurants, train times, or rare marsupials), resides and is altered
- `Controller`: what manages communication between the two: it takes model information and prepares it for the view and vice versa

The **MVC paradigm** is a language- and application-agnostic pattern for **separating concerns**

- MVC describes an architecture of three modularized parts: `model`, `controller`, `view`
- While Rails emulates the majority of an MVC structure, it is not a perfect "textbook" match — *by design*!