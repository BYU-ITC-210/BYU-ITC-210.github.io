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

To the `<script>` element add the following function

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

To the `<nav>` element add the following button.

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

The line you added to the `loadLorem` function connects the `onclick` event of every span in `<article>` event to the `makeBold` function.

> Notice that you don't use parentheses when making the assignment. That's because you are assigning the function to the event. The name _without_ parentheses _references_ the function. _With_ parentheses, the name _calls_ the function. So, if you were to put parentheses on the function then you cause the function to be called immediately and then assign the return value from the function to the event. Since `makeBold` doesn't return anything (and it requires an argument) that wouldn't work.

An event handler receives an argument of type [Event](https://developer.mozilla.org/en-US/docs/Web/API/Event). The `target` property of that event is the element (or other object) that generated the event. In the case of `makeBold()` that's a handy way to know which element to bold.

Just for fun, try using the `onmouseover` event instead of `onclick`. Also try changing other style attributes such as the color.

## Attaching Events Using addEventListener()

Some events are likely to be used by several different libraries. If you simply assign an event, you may disconnect an existing handler. So, it's much safer to use `addEventListener()` because the function will add your event handler while preserving all existing handlers.

> This is the preferred method for attaching events from JavaScript.

A good example is the `window.onload` event which is triggered when the document has finished loading.

Event _properties_ like we used for both of the previous methods, start with "on". When using `addEventListener()`, you use the bare event name without "on." So, "onload" becomes to "load", "onclick" becomes "click", and so forth.

At the end of your `<script>` element add the following:

```js
window.addEventListener("load", loadLorem);
```

Refresh, and you'll see the random text appear immediately without you having to click **Load Article**.

> As with event properties, you leave the parentheses off of the event handler. Otherwise, it will call the function and assign the return value, _which you don't want_.

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

Events also have methods. For example, `preventDefault()` prevents other handlers (including built-in ones) from being called.