---
title: Introduction to JavaScript (Walkthrough)
---
JavaScript is syntactically similar to Java or C++ but it is functionally quite different. It is interpreted or Just-In-Time compiled.

It's primary use is as a browser-side language for web applications. Using [Node.js](https://nodejs.org/), JavaScript may be used on the desktop or on a server.

## JavaScript is _NOT:_

* [JQuery](https://en.wikipedia.org/wiki/JQuery) - which is a popular JavaScript library.
* [Java](https://en.wikipedia.org/wiki/Java_(programming_language)) - which is a popular compiled language from Oracle that shares some syntax but is otherwise entirely different.
* a replacement for HTML and CSS. The three languages are best used together.

## Setup

Start with a simple web page to which you will add JavaScript

index.html
```html
<!DOCTYPE html>
<html>
    <head>
        <title>JavaScript Walkthrough</title>
    </head>
    <body>
        <h1>JavaScript Walkthrough</h1>
        <p>Hello.</p>
    </body>
</html>
```

## Basics

* Statements end with semicolon `;`
* Code blocks are delimited with curly braces `{` and `}`.
* Comments may be inline `/*` like this `*/`
* Comments may be single line `//` like this
* The escape character in strings is `\`
* Values have types (strings, numbers, objects, and arrays) but variables are typeless.

## JavaScript Placement on Web Pages

There are four ways to add JavaScript to your HTML page:

### Inline

```html
<span onmouseover="alert('You moused over.')">Hover here.</span>
```

Just like with inline CSS, you should have a good reason for this. Otherwise, choose a different option.

### Embedded

```html
<script>
    console.log("Hello, world!");
    document.write("Hello from JavaScript!");
</script>
```

Script elements can be placed anywhere in the `<head>` or `<body>`. The code is executed or compiled when it is encountered as the page is processed from top to bottom.

### External File
Script that is placed in an external file may be used in multiple HTML pages.

script.js
```js
function buttonPopup() {
    alert("Hello, Button!");
}
```

index.html
```html
<head>
    <script src="script.js"></script>
</head>
<body>
    <button onclick="buttonPopup()">Click here!</button>
</body>
```

## Variables in JavaScript

Variables, even global ones, have the lifetime of a single page.

<table>
<tr><th>Keyword</th><th>Scope</th><th>Hoisting</th><th>Can Change Value</th><th>Can be Redeclared</th></tr>
<tr><td>var</td><td>Function</td><td>Yes</td><td>Yes</td><td>Yes</td></tr>
<tr><td>let</td><td>Block</td><td>No</td><td>Yes</td><td>No</td></tr>
<tr><td>const</td><td>Function</td><td>No</td><td>No</td><td>No</td></tr>
</table>

This works:
```js
function sample(isNumber) {
    if (typeof(isNumber) == "number") {
        var t = "Number";
    }
    else {
        // var can be redeclared
        var t = "Something Else";
    }

    // t is still in scope because of hoisting
    console.log(t);
}
```

This generates an error:
```js
function sample(isNumber) {
    if (typeof(isNumber) == "number") {
        let t = "Number";
    }
    else {
        // OK because different scope
        let t = "String";
    }

    // Error because "t" is out of scope
    console.log(t);
}
```

> Using the newer keywords, `let` and `const` offers better protection against programming errors because they limit the scope of variables.

If you simply assign a value to a variable, without defining it, JavaScript will create a global variable of that name and assign a value. Even if the assignment is inside a function it will still create a global variable.

```
function globalTest() {
    globalValue = 42;
}

globalTest();
console.log(globalValue);
```

This is usually not desireable behavior. More times than not, you didn't intend to create a global variable. You can put JavaScript in *strict* mode by placing the following at the beginning of your .js file:

```
"use strict";
```

Strict mode will throw an error rather than create a global variable. It also places a few more restrictions on your code intended to prevent bugs. Read more here: [Mozilla Strict Mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode).

## Boolean Interpretation

Any variable can be interpreted as a boolean.

* Values interpreted as `false`: `false` (keyword), 0, NaN, "" (empty string), null, and undefined
* Values interpreted as `true`: Everything else: `true` (keyword), 32, "hi", [] (empty array), {} (empty object)

```js
function reportTrueFalse(x) {
    if (x) {
        console.log(x + " (" + typeof(x) + ") true");
    }
    else
    {
        console.log(x + " (" + typeof(x) + ") false");
    }
}

reportTrueFalse("");
reportTrueFalse("Hello");
reportTrueFalse(32);
reportTrueFalse(false);
reportTrueFalse(true);
reportTrueFalse("false");
```

Javascript performs type conversion when comparing values using `==` or `!=`.

When using `===` or `!==` both value and type are compared.


```js
function reportEqual(a,b) {
    let equal = a == b;
    console.log(equal);
}

function reportExactlyEqual(a,b) {
    let equal = a === b;
    console.log(equal);
}

reportEqual(1, "1");        // Reports true
reportExactlyEqual(1, "1"); // Reports false
```

## JavaScript Functions

In JavaScript, functions are simply code assigned to a variable.

These two do exactly the same thing.
```js
function giveThanks() {
    console.log("Thank you!");
}

var giveThanks = function() {
    console.log("Thank you!");
}
```

You can change the code assigned to a function.

```js
function giveThanks() {
    console.log("Thank you!");
}

giveThanks();

giveThanks = function() {
    console.log("I am grateful!");
}

giveThanks();
```

## Language Flow

These statements work much like the equivalents in C, C++, or Java. Follow the links for references.

* [if...else](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else) Conditional execution.
* [switch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch) Select an operation based on a variable value.
* [while](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/while) Loop while a condition is true.
* [do...while](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/do...while) Loop at least once while a condition is true.
* [for](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for) Loop using a control variable.
* [for...in](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in) Iterates over all properties of an object.
* [for...of](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of) Iterates over all members of a collection such as an array.

## Object-Oriented Programming

Objects have:
* Properties (variables associated with them)
* Methods (functions associated with them)
* Events (actions or triggers associated with them)

You create an object using brace syntax.

An empty object.
```js
let myObject = {};
```

An object with contents.
```js
myObject = { 
    name: "Lancelot",
    favoriteColor: "Blue",

    reportColor: function() {
        console.log(this.favoriteColor);
    }
};
```

You can add properties to an existing object.

```js
myObject.slogan = "The Brave";
```

You can add methods to existing objects.

```js
myObject.changeColor = function() {
    if (this.favoriteColor == "Blue") {
        this.favoriteColor = "Green";
    }
    else {
        this.favoriteColor = "Blue";
    }
}
```

## Global Objects
The browser defines several global objects. The most important are:

* [window](https://www.w3schools.com/js/js_window.asp): The browser window in which your page is displayed.
* [document](https://www.w3schools.com/js/js_htmldom_document.asp): The HTML document represented by your page.

Each of these has properties, methods and events that are useful to access from JavaScript.

## Events

You can subscribe to an event by assigning a function to that event.

```js
function whenWindowLoads() {
    console.log("window.load event fired!");
}

window.onload = whenWindowLoads;
```

However, using `addEventListener` will let you have multiple handlers subscribe to the same event. Doing so is especially important on global objects like `window` and `document`.

```js
function whenWindowLoads() {
    console.log("window.load event fired!");
}

window.addEventListener('load', whenWindowLoads);
```

The preceding could be written with an anonymous function.

```js
window.addEventListener('load', function whenWindowLoads() {
    console.log("window.load event fired!");
});
```

You can create custom events and raise events. However, that is an advanced subject beyond the scope of this class.
