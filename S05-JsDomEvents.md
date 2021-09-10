---
title: JavaScript DOM Events
---

HTML DOM elements have events.

**Glossary:**

* **Trigger**: The action that launches the event. Such as clicking on a button.
* **Handler**: A JavaScript function that is invoked when the _trigger_ occurs.
* **Registration**: Connecting an event to a handler.

## Setup

Create a simple HTML page like this:

```html
<!DOCTYPE html>
<html>

<head>
    <title>JavaScript Events Demo</title>
</head>

<body>
    <h1>JavaScript Events Demo</h1>

    <nav>
    </nav>

    <article>
    </article>

    <script>
    </script>
</body>

</html>
```

## Attaching Events Using Inline JavaScript

To the `<script>` section add the following function

```js
function loadLorem() {
    let words = ["lorem", "ipsum", "dolor", "sit", "amet"];

    let article = document.getElementsByTagName("article")[0];
    for (let i=0; i<100; ++i)
    {
        let word = words[Math.floor(Math.random() * words.length)];
        let span = document.createElement("span");
        span.textContent = word;
        article.appendChild(span);
        article.appendChild(document.createTextNode(" "));
    }
}
```

To the `<nav>` section add the following button.

```html
<button onclick="loadLorem()">Load Article</button>
```

The **inline JavaScript** within the `onclick` attribute connects the onclick event of the button to the corresponding JavaScript function.

Load your page into a browser and click the **Load Article** button.

## Attaching Events Using Element Properties

Add the following function to the `script` section of your page.

```js
function makeBold(e) {
    e.target.style.fontWeight = "bold";
}
```

In the `loadLorem()` function, right before the `article.appendChild` lines, add the following:

```js
span.onclick = makeBold;
```

Refresh the page, click **"Load Article"** and then click on some of the words in the article.

The line you added to the `loadLorem()` function connects the `onclick` event of every span in `<article>` event to the `makeBold()` function.

Notice that you don't use parentheses when making the assignment. That's because you are assigning the function to the event. If you were to put parentheses on the function then you cause the function to be called and set the return value from the function to the event. Since `makeBold` doesn't return anything (and it requires an argument) that wouldn't work.

An event handler receives an argument of type [Event](https://developer.mozilla.org/en-US/docs/Web/API/Event). The `target` property of that event is the element (or other object) that generated the event. In teh case of `makeBold()` that's a handy way to know which element to bold.

Just for fun, try using the `onmouseover` event instead of `onclick`. Also try changing other style attributes such as the color.

## Attaching Events Using addEventListener()

Some events are likely to be used by several different libraries. For those, it's better to use `addEventListener() because it will add your event handler while preserving all existing handlers.

A good example is the `window.onload` event which is triggered when the document has finished loading.

For most events that start with "on", `addEventListener()` requires that "on" be left out. So, "onload" changes to "load".

At the end of your `<script>` section add the following:

```js
window.addEventListener("load", loadLorem);
```

Now you don't have to click "Load Article" to get the random text.

Again, when assigning an event handler, leave the parentheses off. Otherwise, it will call the function and assign the return value, _which you don't want_.

## Some Useful Events

* load
* mouseover
* click
* keypress
* submit
* dblclick
* resize

See a full list [here](https://www.w3schools.com/jsref/dom_obj_event.asp)

## Useful Properties of the Event object

The [Event](https://developer.mozilla.org/en-US/docs/Web/API/Event) object, which is passed to event handlers, has its own properties. Some of the most useful are the following.

* `target` - the DOM element that triggered the event.
* `button` - which mouse button was clicked.
* `altKey` - was the Alt key held down when the event occurred.
* `timeStamp` - the time when the event was triggered.
* `type` - name of the event.

Events also have methods. For example, `preventDefault()` prevents other handlers (including build-in ones) from being called.