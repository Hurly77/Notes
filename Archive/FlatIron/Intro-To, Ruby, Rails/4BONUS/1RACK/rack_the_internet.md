## **How the Internet Works**

## Client and Server

So seriously, how does this:

```
https://www.youtube.com/user/AdeleVEVO
```

turn into a web page

By **typing** in that **URL** into your browser, you (**the client**) are ***requesting*** a **web**. Then a sever either does or doesn't responed to that request.

**H**yper **T**ext **T**ransfer **P**rotocol or HTTP.Your server will receive requests from the browser that follow HTTP. It then responds with an HTTP response that all browsers are able to parse.

### URI

**U**niform **R**esource **I**dentifiers or URIs you've probably also heard of these as URLs

`http://www.youtube.com/adelevevo`

### This URI is broken into three parts:

**The Protocol** - The `protocol` is the **way** we're **sending** our request. There are **several** different types of internet **protocols** (SMTP for emails, **HTTPS** for **secure** requests, **FTP** for file transfers). To load a website, we use **HTTP**.

**The  Domain** - The `domain name` is a string of characters that identifies the unique location of the web server that hosts that particular website. This will be things like `youtube.com`

**The resource**  - The `resource` is the particular part of the website we want to load

The **domain** is the entire **building**. Within that **building** though there are hundrbeds of **apartments**

### HTTP Verbs

| VERB    | Description                                                  |
| ------- | ------------------------------------------------------------ |
| HEAD    | Asks for a response like a GET but without the body          |
| GET     | Retrieves a representation of a resource                     |
| POST    | Submits data to be processed in the body of the request      |
| PUT     | Uploads a representation of a resource in the body of the request |
| DELETE  | Deletes a specific resource                                  |
| TRACE   | Echoes back the received request                             |
| OPTIONS | Returns the HTTP methods the server supports                 |
| CONNECT | Converts the request to a TCP/IP tunnel (generally for SSL)  |
| PATCH   | Apply a partial modification of a resource                   |

### Request Format

 The **server** then responds with all the code associated with that resource ` .....`, including all images, CSS files, JavaScript files, videos, music, etc.

## Responses

Once your server receives the request, it will do some processingand then send a response back. The **server's** **response** is separated into two **sections**, the **headers** and the **body**.

**headers** are all of the metadata about the response

 The ***body*** of the response is what you see rendered on the page  It is all of that **HTML/CSS** that you see!

### Status Codes

You probably already know status codes like **a successful response** 200 or **file not found** 404 **Status codes** are **separated** into categories based on their **first** digit.

- 100's - informational
- 200's - success
- 300's - redirect
- 400's - error
- 500's - server error

A full list of status codes is [up on Wikipedia](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

## Servers

There are two different types of **webapps**: **static** and **dynamic**. A **static** - **webapp** is one that doesn't change. A **dynamic** -  webapps are sites where the content changes based on user input