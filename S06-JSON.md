---
title: JSON and JavaScript
---
JavaScript Object Notation (JSON) is a simple data format that works with any language and platform. It is particularly well-suited for use with web technologies and JavaScript.

JSON Example:

```json
{
  "groupName": "The Monkees",
  "members": [
    {"firstName": "Davey", "lastName": "Jones"},
    {"firstName": "Peter", "lastName": "Tork"},
    {"firstName": "Michael", "lastName": "Nesmith"},
    {"firstName": "Micky", "lastName": "Dolenz"},
  ]
}
```

JSON is composed of objects, arrays, and values. A JSON object is surrounded by braces: `{` and `}`. A JSON array is surrounded by brackets: `[` and `]`.

In the object above, the main object has a member named `groupName` with the value `The Monkees`. Then it has a member named `members` with the value of an array. Each element in the array has a `firstName` and a `lastName`. Members of an array don't have to follow a consistent format.

## Setup

For this walkthrough, prepare a simple web page for JSON experimentation:

```html
<html>
<head>
    <title>JSON Experiments</title>    
</head>
<body>
    <h1>JSON Experiments</h1>

    <article></article>

    <script>
        
    </script>
</body>
</html>
```

## JSON Embedded in JavaScript
JSON is a proper subset of the JavaScript language. Because of that, you can simply embed JSON in JavaScript and assign it to a variable.

To the `<script>` element, add the following:

```js
let band = {
  "groupName": "The Monkees",
  "members": [
    {"firstName": "Davey", "lastName": "Jones"},
    {"firstName": "Peter", "lastName": "Tork"},
    {"firstName": "Michael", "lastName": "Nesmith"},
    {"firstName": "Micky", "lastName": "Dolenz"},
  ]
}
```

For this to be of value, we must write some code to use the data. Add this to the end of the `<script>`:

```js
function addToArticle(name, value) {
    let p = document.createElement("p");
    let span = document.createElement("span");
    span.textContent = name;
    p.appendChild(span);
    p.appendChild(document.createTextNode(": "));
    span = document.createElement("span");
    span.textContent = value;
    p.appendChild(span);
    document.getElementsByTagName("article")[0].appendChild(p);
}

addToArticle("Band", band.groupName);
for (let entry of band.members) {
    addToArticle("Member", entry.firstName);
}
```

Now, load the page into a browser and view the result.

The `addToArticle()` function uses the [DOM](S05-JsAndDom) to add a paragraph to the `<article>` with a name and a value. We'll use it through the rest of this walkthrough to insert information into the page.

## Converting To and From JSON

JavaScript has a built-in JSON object with methods to convert between objects and JSON strings. When in string form. JSON can be sent to a server, received from a server, or stored in [Browser Local Storage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage).

Add the following to the end of the `<script>` element:

```js
let car = {
    make: "MINI",
    model: "Cooper",
    year: "2004"
}

addToArticle("Car", JSON.stringify(car));
```

The code above takes a JavaScript object and converts it into a JSON string.

Now, let's take a JSON string and convert it into an object.

```js
let knight = JSON.parse('{"name": "Sir Lancelot", "favoriteColor": "blue"}');

addToArticle("Knight", knight.name);
```

## Converting Form Contents into JSON

For our last step, we'll let the user enter information into a form and convert the results into JSON.

First, create the form and add it to the body of the document right after the `<article>` element.

```html
<form onsubmit="submitForm(event)">
    <label for="name">Name:</label> <input type='text' id='name' name='name'/><br/>
    <label for="color">Favorite Color:</label> <input type='text' id='color' name='color'/><br/>
    <input type='submit' value='Submit'/>        
</form>
```

Now, create an event handler that will convert the form data into JSON.

```js
function submitForm(event) {
    let formData = new FormData(event.currentTarget);
    let json = JSON.stringify(Object.fromEntries(formData));
    console.log(json);
    addToArticle("Form", json);
    event.preventDefault();
}
```
