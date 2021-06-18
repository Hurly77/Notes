## Define a JavaScript Event

JavaScript has the ability to "listen" for things that happen inside the browser. It can listen for events like whether the browser resized, or whether someone clicked on a specific image on the screen. The event you're probably most familiar with is "click."

We'll cover a few more common types of events below.

## Identify Different Types of User Events

Let's take a look at some of the more common events.

### Mouse Click

The mouse / trackpad is a primary pointing device when working with browsers. In response to click, double-click, right-click, etc. we can do work like, changing the background of the document to a random color every time someone clicks on the page.

### Key Press

While click events make up the majority of events you'll use, the keyboard is *also* an important source of events. We can listen for `keypress`, `keydown`, and `keyup`. When these events are handled, we can find out which keys were pressed (like if a space was hit to make the character jump over the hole).

### Form Submission

HTML pages often use a submit button to submit a form to a server. When a user submits a form, the `submit` event is fired. An event handler here might pop up a thank you overlay, play a song, or provide some other sort of interactivity depending on what values were entered in the form.

### List of Events

As you seek to build more complicated applications, you'll need to handle and trigger work on more events. Here's a list of a [ton of browser events](http://help.dottoro.com/larrqqck.php).0.

## **JavaScript Event Listening**

## `addEventListener()`

`addEventListener()` takes two arguments:

1. the name of the event
2. a *callback function* to "handle" the event

```javascript
const input = document.getElementById('input');
input.addEventListener('click', function(event) {
  alert('I was clicked!');
});
// when input is clicked an alert saying "I was clicked "
```

# lab solution

```javascript
function addingEventListener() {
  const input = document.getElementById('input');
input.addEventListener('click', function(_event) {
  alert('I was clicked!');
});
}
```

Typing notes

left = `123456

right = 7890-= and backspace