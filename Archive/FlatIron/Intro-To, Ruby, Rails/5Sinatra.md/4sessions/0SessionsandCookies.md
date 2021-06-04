## How do websites recognize individual users?

 A **cookie** is a hash that gets stored in the browser and sent back to the server along with every subsequent request.

Similarly to cookies, a **session** is an object, like a hash, that stores data describing a client's interactions with a website at a given point in time. 

**web application know** who is interacting with it, by using both **cookies and session **data.**

## How cookies work

1. You visit a webpage, such as [facebook.com](https://www.facebook.com/).
2. **Facebook's** web **application** **receives the reques**t and **creates a cookie** with some information about you as a user.
3. Facebook **sends that cookie *back** to the browser*, where it is stored. Cookies will last until they expire or are deleted.
4. **Every subsequent request** you make to [facebook.com](https://www.facebook.com/) sends the cookies *back to the server*, where Facebook can access them, retrieve information, and alter the cookies.

A **cookie** will usually contain a URL to the generating website, the date on which it was created (and the date on which it is set to expire, if applicable), and other pertinent information that the web application has requested to persist (such as remembered login information, user preferences, etc).

#### Session Cookies vs. Persistent Cookies

There are two primary kinds of cookies: **session** and **persistent**.

**Session cookies** allow a website to keep track of your movement from page to page for that specific session (from the time you log in to the time you log out or close the browser). **Session** cookies **simply** allow you to **navigate** through a **site** 

**Persistent cookies** allow a website to remember your user information and preferences for *future* visits. A **persistent cookie** is **stored** on **your computer**, while a **session cookie** is **temporarily** stored in your **web browser**. 

### Cookies and Sessions: Teamwork!

You can think of cookies as the **client-side** **counterpart to sessions**.