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

Web pages opened directly from the file system cannot access Browser localStorage (Window.localStorage). So, we need a minimal web server. Create a file named "docker-compose.yml" in the same folder as your "page.html" load it with the following contents:

```yaml
version: "3"
services:
    web:
        image: httpd
        container_name: apache
        ports:
            - 80:80
        volumes:
            - .:/usr/local/apache2/htdocs
```

> This `docker-compose.yml` file is slightly different from the one we used for Lab-1a and Lab-1b. Like the one used for the labs, it loads the 'httpd' container which is a simple Apache web server. However, this one maps the current directory (represented by the dot) to the directory where Apache expects to find the contents of the website (`/usr/local/apache2/htdocs`);

Open a console in the current folder and type the following command:

```
docker compose up -d
```

Verify that your page can be viewed by browsing to `http://localhost/page.html`

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

Now, load the page into a browser and view the result.

The `addToArticle()` function uses the [DOM](/S05-JsAndDom) to add a paragraph to the `<article>` with a name and a value. We'll use it through the rest of this walkthrough to insert information into the page.

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

The code above takes a JavaScript object and converts it into a JSON string. Refresh the page to see that.

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

Refresh the page, enter something into the form, and click `Submit`.

## localStorage

Web browsers have a localStorage collection where you can store strings that will be retained between page refreshes and even between browser sessions. We'll store the form data in that collection and retrieve it when the page loads.

Update the `submitForm` function like this:

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

Now, lets set it up so that whenever the page loads, it retrieves the previously-submitted form data and puts it back into the form.

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

Now, refresh the page again and you'll find that the form is pre-filled with the same values as you used last time. Close the browser entirely, open the browser, and go to the URL `http://localhost/page.html` and the data will be displayed in the form again. The data was saved even across browser sessions. In fact, you could close the browser, reboot your computer, and browse back to the page to find that the data are still there.

Of course, if you reboot you'll have to start the docker server again before browsing to the page.