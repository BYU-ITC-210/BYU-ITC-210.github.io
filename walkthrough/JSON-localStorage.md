---
title: JSON and localStorage
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

For this walkthrough, prepare a simple web page for JSON experimentation. Call it "page.html" and put it in an empty folder:

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

Open the page in your browser and see your empty page.

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
    {"firstName": "Micky", "lastName": "Dolenz"}
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

Now, refresh the page and view the result.

The `addToArticle()` function uses the [DOM](JsAndDom) to add a paragraph to the `<article>` with a name and a value. We'll use it through the rest of this walkthrough to insert information into the page.

## Converting To and From JSON

JavaScript has a built-in [JSON namespace](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON) with methods to convert between objects and JSON strings. When in string form. JSON can be sent to a server, received from a server, or stored in [Browser Local Storage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage).

Add the following to the end of the `<script>` element:

```js
let car = {
    make: "MINI",
    model: "Cooper",
    year: "2004"
}

addToArticle("Car", JSON.stringify(car));
```

The code above takes a JavaScript object and converts it into a JSON string. Refresh the page to see that.

> Notice that the JavaScript object, `car`, doesn't have quotes around the property names `make`, `model`, and `year`. In JSON, all property names must be quoted. In JavaScript, property names don't require quotes so long as they are composed of letters, digits, and underscore and they don't start with a digit.

Now, let's take a JSON string and convert it into an object.

```js
let jsonString = '{"name": "Sir Lancelot", "favoriteColor": "blue"}';
let knight = JSON.parse(jsonString);

addToArticle("Knight", knight.name);
```

## Converting Form Contents into JSON

For the next step, we'll let the user enter information into a form and convert the results into JSON.

First, create a form and add it to the body of the document right after the `<article>` element.

```html
<form onsubmit="submitForm(event)">
    <label for="name">Name:</label> <input type='text' id='name' name='name'/><br/>
    <label for="color">Favorite Color:</label> <input type='text' id='color' name='color'/><br/>
    <input type='submit' value='Submit'/>        
</form>
```

Now, create an event handler that will convert the form data into JSON. Add this to the end of the `<script>` element:

```js
function submitForm(event) {
    event.preventDefault();
    let formData = new FormData(event.target);
    let json = JSON.stringify(Object.fromEntries(formData));
    addToArticle("Form", json);
}
```

This event handler does three things when you submit a form. First, it calls `event.preventDefault()`. That's because the default action for a form is to send the form data to the server and then reload the page. We don't want action. Next, it converts the data into JSON form in two steps, `Object.fromEntries()` and `JSON.stringify()`. Finally, it writes the JSON version of the form data out to the page using the `addToArticle()` function we were using before.

Refresh the page, enter something into the form, and click `Submit`.

## window.localStorage

Web browsers have a [window.localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) collection where you can store strings that will be retained between page refreshes and even between browser sessions. We'll store the form data in that collection and retrieve it when the page loads.

Update the `submitForm` function with one more line like this:

```js
function submitForm(event) {
    event.preventDefault();
    let formData = new FormData(event.target);
    let json = JSON.stringify(Object.fromEntries(formData));
    addToArticle("Form", json);

    localStorage.setItem("form", json);
}
```

The `localStorage.setItem()` call will now store the form data in local storage whenever you click `Submit`.

Now, set up the page so that whenever the page loads, it retrieves the previously-submitted form data and puts it back into the form.

Add this function to the `<script>`:

```js
function loadFormFromStorage() {
    let json = localStorage.getItem("form");
    if (!json) return;
    let data = JSON.parse(json);
    document.getElementById("name").value = data.name;
    document.getElementById("color").value = data.color;
}
```

Finally, we need to make that function run whenever the page is loaded. Since the page will run all JavaScript from beginning to end, we could just put `loadFormFromStorage()` at the end of the script. But if we really want to be sure that the script runs after the entire page is loaded then the best option is to attach to the window load event.

```js
window.addEventListener("load", loadFormFromStorage)
```

Refresh the page, enter something into the form, and click `Submit`. It does just what the previous version did.

Now, refresh the page again and you'll find that the form is pre-filled with the same values as you used last time. Close the browser entirely, open the page in your browser and the data will be displayed in the form again. The data was saved even across browser sessions. In fact, you could close the browser, reboot your computer, and browse back to the page to find that the data are still there.