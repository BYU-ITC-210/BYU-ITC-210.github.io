---
title: CRUD and REST using Node.js Walkthrough
---

[CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) is an acronym for Create-Read-Update-Delete. These are the four basic database functions. Realistically, however, most systems have a few more operations such as `Search` and `Find`. And `Read` can refer to reading an individual object/record or reading a set of them.

[REST](https://en.wikipedia.org/wiki/REST) is an acronym for REpresentational State Transfer. It is an API design philosophy where a representation of an object is transferred from server to client, manipulated or updated on the client, and then sent back to the server. Originally, object representations were coded in [XML](https://en.wikipedia.org/wiki/XML) but today [JSON](https://en.wikipedia.org/wiki/JSON) encoding is more common and that's what we will use in this class. APIs designed according to REST principles are called RESTful APIs.

The two concepts work together as representations of objects are typically Created, Read, Updated, and Deleted. In this walkthrough, you will use an existing RESTful API to Create, Read, Update, and Delete objects that represent cars.

## Setup

Since this walkthrough leads into Lab 4, which uses Node.js, you should install Node.js from this link: [Node.js Downloads](https://nodejs.org/en/download). 

Open a terminal and launch node (type "node" at the terminal).

> **Note:** Since this walkthrough uses JavaScript exclusively in interactive mode, you can, in theory, perform this walkthrough from the JavaScript console in your favorite browser without installing Node. To do so, open your browser and browse to any web page (a blank page won't work). Then, open the JavaScript console in your browser (Developer Tools > Console). Ignore the web page and just enter the code from this walkthrough at the console.

## The API

The API we will be using is located at `https://restspace.dicax.org/api`. It has a `login` function for opening a session. All of the CRUD calls use the `items` url. Here is the full set:

<table>
<tr><th>Operation</th><th>HTTP Verb</th><th>URL</th><th>Payload</th><th>Response</th></tr>
<tr><td>Login</td><td>POST</td><td>https://restspace.dicax.org/api/login</td><td>{"username": "myusername", "password": "mypassword"}</td><td>{"authorizationToken": "tokenvalue"}</td></tr>
<tr><td>Create</td><td>POST</td><td>https://restspace.dicax.org/api/items</td><td>{"make": "Pontiac", "model": "Firebird", "year": "1995"}</td><td>{"id": "1"}</td></tr>
<tr><td>Read All</td><td>GET</td><td>https://restspace.dicax.org/api/items</td><td>(None)</td><td>A JSON list with all objects in it.</td></tr>
<tr><td>Read By Id</td><td>GET</td><td>https://restspace.dicax.org/api/items/1</td><td>(None)</td><td>The JSON object value.</td></tr>
<tr><td>Find</td><td>GET</td><td>https://restspace.dicax.org/api/items?make=Pontiac</td><td>(None)</td><td>A JSON list with all matching objects in it.</td></tr>
<tr><td>Update</td><td>PUT</td><td>https://restspace.dicax.org/api/items/1</td><td>{"make": "AMC", "model": "Gremlin", "year": "1976"}</td><td>{"id": "1"}</td></tr>
<tr><td>Delete</td><td>DELETE</td><td>https://restspace.dicax.org/api/items/1</td><td>(none)</td><td>{"id": "1"}</td></tr>
</table>

## Calling the API

Each of these operations involves multiple lines of code. It's unusual to do such things from the Node.js console but it works and it's convenient to do so when experimenting with an API like this. For most things, you can just copy the code from this walkthrough and paste it into your console. You may get a warning about pasting multiple lines. If so, just click "Paste Anyway." In some cases you'll want to paste the code into a text editor (e.g. VS Code), modify it, and then paste it into the console.

The use of `fetch()` in these calls should be familiar from the previous walkthrough. If it looks confusing, please do the Node.js Fetch walkthrough first.

### Login

Before any of the other functions will work, you must first log in and obtain an authorization token. Paste the following into your console:

```js
let authToken = ""
function Login(username, password)
{
    const body = {
        username: username,
        password: password
    };
    const options = {
        method: "POST",
        headers: {
            "Content-Type": "application/json"
        },
        body: JSON.stringify(body)
    };
    fetch("https://restspace.dicax.org/api/login", options)
        .then(response => {
            if (response.ok) {
                return response.json();
            }
            else {
                console.log(response.status, response.statusText);
            }
        })
        .then(json => {
            console.log(json);
            authToken = json.authorizationToken;
        })
        .catch(error => console.error(error));
}
Login("me", "me-9455")
```

This should log you in and print the response. It will also store the authorization token in the global `authToken` variable. We'll need it later.

> This API will accept any username so long as the password is the same as the username but with "-9455" tacked onto the end. This is, of course, "Security by Obscurity" which isn't very secure at all because once someone figures out the secret they can make up other accounts. Nevertheless, it's handy for demo and practice purposes and there are other safeguards against abuse that will be described later.

You can verify the value of `authToken` by just typing it at the console:

```bash
authToken
```

Let's examine this code one piece at a time:

```js
const body = {
    username: username,
    password: password
}
```

Creates a "model" object representing the body to be written in the HTTP request using the username and password that were supplied to the function.

```js
const options = {
    method: "POST",
    headers: {
        "Content-Type": "application/json",
    },
    body: JSON.stringify(body)
}
```

Sets up the details of the request including the method, HTTP headers to be passed, and the body of the content. Notice that the `Content-Type` header indicates that the body is in JSON format and the model we created in the previous step is encoded using `JSON.stringify()`.

```js
fetch("https://restspace.dicax.org/api/login", options)
```

This initiates the request to the server. `fetch()` returns a promise. The `Promise.then()` methods which follow indicate what should be done upon successful or failed calls.

```js
.then(response => {
    if (response.ok) {
        return response.json();
    }
    else {
        console.log(response.status, response.statusText);
    }
})
```

The first `.then()` contains an [anonymous or "arrow" function](https://www.w3schools.com/js/js_function_definition.asp) that handles the response from the server. In this case it checks for an "ok" response. If the response is `ok`, it calls `Response.json()` to parse the JSON response into a JavaScript object. If the resonse is not `ok` it reports the error to the console.

```js
.then(json => {
    console.log(json);
    authToken = json.authorizationToken;
})
```

The call to `Response.json()` returns another `Promise` so we chain to another `.then()` clause to manage the asynchronous call. Here, we report the entire object to the console (using `console.log()`) and then store the authorization token for future use.

### Create

The following script creates one car in the database. Copy and paste it into your console.

```js
function CreateCar(make, model, year) {
    const body = {
        make: make,
        model: model,
        year: year
    };
    const options = {
        method: "POST",
        headers: {
            "Authorization": "Bearer " + authToken, 
            "Content-Type": "application/json"
        },
        body: JSON.stringify(body)
    };
    fetch("https://restspace.dicax.org/api/items", options)
        .then(response => {
            if (response.ok) {
                return response.json();
            }
            else {
                console.log(response.status, response.statusText);
            }
        })
        .then(json => {
            console.log(json);
        })
        .catch(error => console.error(error));
}
CreateCar("MINI", "Cooper", 2004);
```

Here are the differences from the `Login()` call:
* The `body` model has different values appropriate to the task.
* The `headers` include an `Authorization` header with the token obtained from login.

Call `CreateCar` a couple more times to add a few cars to the collection. The function already exists so you just have to call it with other arguments.

```js
CreateCar("Willys", "Jeep", 1946);
```

### Read All

```js
function ReadAll() {
    const options = {
        method: "GET",
        headers: {
            "Authorization": "Bearer " + authToken
        }
    };
    fetch("https://restspace.dicax.org/api/items", options)
        .then(response => {
            if (response.ok) {
                return response.json();
            }
            else {
                console.log(response.status, response.statusText);
            }
        })
        .then(json => {
            console.log(json);
        })
        .catch(error => console.error(error));
}
ReadAll();
```

Since this uses the `GET` method, it's simpler. There's no `Content-Type` header and no body. Through the rest of this walkthrough you can call ReadAll() at any time to see what's in the collection.

### Read by ID

```js
function ReadById(id) {
    const options = {
        method: "GET",
        headers: {
            "Authorization": "Bearer " + authToken
        }
    };
    fetch("https://restspace.dicax.org/api/items/" + id, options)
        .then(response => {
            if (response.ok) {
                return response.json();
            }
            else {
                console.log(response.status, response.statusText);
            }
        })
        .then(json => {
            console.log(json);
        })
        .catch(error => console.error(error));
}
ReadById(0);
```

If you get a 404 error you need to change the ID from `0` to the ID of something that's actually in the database. Look at the IDs in the items returned from `ReadAll()`.

The only difference between this function and `ReadAll()` is that the `id` argument is added to the end of the URL path.

### Find

```js
function Find(query) {
    const options = {
        method: "GET",
        headers: {
            "Authorization": "Bearer " + authToken
        }
    };
    fetch("https://restspace.dicax.org/api/items?" + query, options)
        .then(response => {
            if (response.ok) {
                return response.json();
            }
            else {
                console.log(response.status, response.statusText);
            }
        })
        .then(json => {
            console.log(json);
        })
        .catch(error => console.error(error));
}
Find("make=MINI");
```

Similarly, `Find` only differs from `ReadAll()` by adding a query string to the URL. If user input were being used, you would need to call `encodeURIComponent()` on the values in the query string so that special characters don't cause problems.

### Update

```js
function UpdateCar(id, make, model, year) {
    const body = {
        make: make,
        model: model,
        year: year
    };
    const options = {
        method: "PUT",
        headers: {
            "Authorization": "Bearer " + authToken, 
            "Content-Type": "application/json"
        },
        body: JSON.stringify(body)
    };
    fetch("https://restspace.dicax.org/api/items/" + id, options)
        .then(response => {
            if (response.ok) {
                return response.json();
            }
            else {
                console.log(response.status, response.statusText);
            }
        })
        .then(json => {
            console.log(json);
        })
        .catch(error => console.error(error));
}
UpdateCar(0, "MINI", "Cooper", 2008);
```

In a real application, `Update` is generally preceded by a call to `ReadById` (or its equivalent) and the existing value is modified before sending it back through the `Update` call.

If you get a 404 error you need to change the ID from `1` to the ID of something that's actually in the database. Use `ReadAll()` to find those IDs.

### Delete

```js
function Delete(id) {
    const options = {
        method: "DELETE",
        headers: {
            "Authorization": "Bearer " + authToken
        }
    };
    fetch("https://restspace.dicax.org/api/items/" + id, options)
        .then(response => {
            if (response.ok) {
                return response.json();
            }
            else {
                console.log(response.status, response.statusText);
            }
        })
        .then(json => {
            console.log(json);
        })
        .catch(error => console.error(error));
}
Delete(1);
```

`Delete` is just like `ReadById` except for the verb being `DELETE` instead of `GET`.

If you get a 404 error you need to change the ID from `1` to the ID of something that's actually in the database. Use `ReadAll()` to find those IDs.

### Using Async - Await syntax

In these functions we used anonymous functions to handle the asynchronous responses from fetch calls. ECMAScript 8 (version 8 of the standard for JavaScript) introduced the `async` - `await` syntax for working with promises. Here is a version of `CreateCar()` rewritten to use `async` - `await`.

For more information, see [this article](https://javascript.info/async-await).
```js
async function CreateCar(make, model, year) {
    const body = {
        make: make,
        model: model,
        year: year
    };
    const options = {
        method: "POST",
        headers: {
            "Authorization": "Bearer " + authToken, 
            "Content-Type": "application/json"
        },
        body: JSON.stringify(body)
    };
    try {
        response = await fetch("https://restspace.dicax.org/api/items", options);
        if (response.ok) {
            json = await response.json()
            console.log(json);
        }
        else {
            console.log(response.status, response.statusText);
        }
    }
    catch(error) {
        console.error(error);
    }
}
```

### Experiment

And that's it! You've called all four CRUD functions plus Find and a variation on Read. By now the CRUD and REST patterns should be making sense plus the syntax of how to call Fetch.

Before leaving this walkthrough, experiment a bit. You'll find that `RestSpace` will store more than just cars. Try storing other items. Or add properties to the car such as Owner or Price. Try automating the creation of ten cars at a time and other interesting stuff. But please don't overload the database. It has a fixed capacity of 1000 items shared between all concurrent users. Once you exceed that, it will purge older items.

### Enhancements (left to the reader)

Session tokens expire after three hours. But every request with a valid token will have an `Authentication-Info` response header containing an updated token refreshed for another three hours. This code ignores that update. A more advanced client would update the `authToken` global variable with the new value thereby allowing sessions of indefinite length.

Much of the code in each of the calls is similar. Consider how to create a more abstract function that encapsulates all or much of the common code.

## A Note on Security

It's a well-known principle that any online service that stores data will be misused. As noted before, the `RestSpace` service uses a password that's derived from the username. That's deliberately weak to facilitate experiential learning. To prevent abuse, the service has some deliberate limitations:

1. Data in the database are tied to a login *session*, not to a particular *username*. If two people log in using the same username, they will still get different sessions with different sets of items.
2. Data are stored temporarily. After 24 hours of idleness, the data from a session are deleted.
3. The database is limited to 1000 items. Once the that many items are in the database it will report errors.

With these limits in place, the service is only useful for practice.