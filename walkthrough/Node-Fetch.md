---
title: Node.js and Fetch Walkthrough
---

[Node.js](https://nodejs.org/) is "an open-source, cross-platform JavaScript runtime environment. It lets you write programs, including web servers, in JavaScript much like you would with Python or other languages. Node uses the V8 JavaScript engine from the [Chromium]() project and works with the [NPM](https://www.npmjs.com/) package manager to access a rich library pf pre-built packages. Among the available packages is [Express.js](https://www.npmjs.com/package/express) which, together with the built in [http.js](https://www.w3schools.com/nodejs/nodejs_http.asp) module, forms a popular and robust web server.

But that's for a future lab. In this walkthrough we'll use Node.js to call a [Web Service](https://en.wikipedia.org/wiki/Web_service) using the [fetch()](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) function.

## Setup

Since this walkthrough leads into Lab 4, which uses Node.js, you should install Node.js. However, since we will be using javaScript exclusively in interactive mode, you can, in theory, perform this walkthrough from the JavaScript console in your favorite browser. Install Node.js from this link: [Node.js Downloads](https://nodejs.org/en/download). Pick the latest LTS (Long-Term Support) version, *not* the "Current" version.

Open a terminal and launch node (type "node" at the terminal). Or, open the JavaScript console in your browser (Developer Tools > Console).

### Experimenting with the Node console

Much like Python, Node offers an interactive console for typing JavaScript. Try typing an expression:

```js
2+2
```

Or you can call a function:

```js
console.log("Hello, world!")
```

You'll notice that Node prints "undefined" after "Hello, world!". That's because it evaluates every expression and prints the result. `console.log()` returns [undefined](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined) so that's what it prints.


## The API

We will be using the BYU ITC210 Scripture of the Day API. The URL is [https://scripture.itc210.net/api/day](https://scripture.itc210.net/api/day). The service also offers scripture of the hour and scripture of the month. You can guess the URLs for hour and month, and experiment to determine whether there might be other related calls.

A call to the API returns a JSON object with `id`, `reference`, and `text` attributes like this:

```JSON
{
    "id": 400,
    "reference": "SomeBook 4:7",
    "text": "Text of the scripture"
}
```

## Calling the API using fetch()

As we discussed in class, `fetch()` is an asynchronous function. That is, it doesn't return a value. Rather, like all asynchronous functions in JavaScript it returns a *Promise* to deliver a value. [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) is a JavaScript object class with methods. Follow the link to view the documentation.

The `Promise.then()` method takes an argument which is a function that should be called when the request is fulfilled. They made `fetch()` asynchronous like this because it can take some time to complete the request and the Node or browser application shouldn't freeze up while waiting.

Before we can call fetch, we have to write the *callback* function that will accept the [Response](https://developer.mozilla.org/en-US/docs/Web/API/Response) object that is provided by fetch. `Response` has two methods for retrieving the body of the request. `text()` retrieves the body as plain text while `json()` parses a JSON response into a JavaScript object. To make matters more complicated, `text()` and `json()` are also asynchronous and so we must write another callback function to accept the data. In this case we'll be using `json()` since the response from the scripture API is in JSON format.

When working with asynchronous functions you sometimes work backwards. We'll start by writing the function to accept the object representing the parsed JSON.

```js
function onParsed(obj) {
    console.log(`${obj.reference} | ${obj.text}`);
}
```

Copy this `onParsed()` function and paste it into your Node or browser console. It may give you a warning about a multi-line past. That's OK. After pasting, press `Enter` to get a new prompt.

Next, we need a callback to accept the response object returned by `fetch()`. Paste the following into your browser.

```js
function onFetched(response) {
  response.json().then(onParsed);
}
```

Now, we're ready to call the API. Paste the following into your browser:

```js
fetch("https://scripture.itc210.net/api/day").then(onFetched)
```

You should see the scripture reference followed by the scripture itself.

You'll notice that the prompt, ">" printed before the scripture and now your cursor is sitting there on a blank line. That's because `fetch()` returned to the console and the ">" prompt was printed before the promise called your `onFetched()` function. You can type a command or expression on that blank line but sometimes the Node autofill messes things up so best to press `Enter` and get a new prompt.

### Handling Errors

Try fetch with a bad path on the API:

```js
fetch("https://scripture.itc210.net/api/badpath").then(onFetched)
```

You get an uncaught error, "Unexpected end of JSON input". What happened? If the server returns anything, including a 404 error, then Fetch will pass that response to your `onFetched()` function. Your function assumes the response is JSON, tries to parse that, and fails. To detect this we need to check the `Response.ok` property. It will be `true` if the response code is in the 200-299 range.

Add error handling to `onFetched` by pasting the following:

```js
function onFetched(response) {
  if (response.ok) {
      response.json().then(onParsed);
  }
  else {
    console.log(`Error: ${response.status} ${response.statusText}`);
  }
}
```

Now try that same bad path:

```js
fetch("https://scripture.itc210.net/api/badpath").then(onFetched)
```

### More Error Handling

If there's no server to be found, then the promise is "rejected". Let's try that:

```js
fetch("https://wrongdomain").then(onFetched)
```

You get an uncaught error. Most of the time you will want to capture errors and report appropriately to the user. `Promise.then()` accepts an optional second argument which is a function to call upon rejection. Let's write that one:

```js
function onRejected(err) {
  console.log(`Rejected: ${err.message} | ${err.cause.message}`);
}
```

Now call `fetch()` with both callbacks.

```js
fetch("https://wrongdomain").then(onFetched, onRejected)
```

It may take a couple of seconds for the DNS lookup to conclude that `wrongdomain` doesn't exist. Try it again with the correct URL and see that all still works:

```js
fetch("https://scripture.itc210.net/api/day").then(onFetched, onRejected)
```

### Promise Chaining

The return value from `.then()` is another `Promise` and the `Promise.catch()` method is another way to specify the `rejected` callback function. So the same code as above could be written like this:

```js
fetch("https://scripture.itc210.net/api/day").then(onFetched).catch(onRejected)
```

Try it with good and bad URLs. We'll take advantage of this when we use anonymous functions.

### Using Anonymous Functions

All of these callbacks can make things complicated to follow. After all, we now have three callback functions and one call to `fetch()` just to get a result back. Sometimes it's nice to separate things out like that but most of the time it just gets confusing. To make things more concise, we can use [anonymous functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions). An anonymous function is simply a function that's not assigned to a named variable. Instead, you pass it as an argument to another function such as `.then()` or `.catch()`.

This uses anonymous functions and promise chaining to consolidate all of the above into one function:

```js
function FetchUrlAndPrint(url) {
  fetch(url)
    .then(function(response) {
      if (response.ok) {
        return response.json();
      }
      else {
        console.log(`Error: ${response.status} ${response.statusText}`);
      }
    })
    .then(function(obj) {
      console.log(obj);
    })
    .catch(function(err) {
      console.log(`Rejected: ${err.message} | ${err.cause.message}`);
    });
}
```

Notice how this uses promise chaining to handle the promise returned from `response.json()` before the `.catch()` callback is set.

Test it using one good and two bad URLs:

```js
function FetchThree() {
    FetchUrlAndPrint("https://scripture.itc210.net/api/day");
    FetchUrlAndPrint("https://scripture.itc210.net/badpath");
    FetchUrlAndPrint("https://wrongdomain");
}

FetchThree()
```

You may notice that the responses print in a different order from how they were called. Since `fetch()` is asynchronous, all three calls are launched at the same time and it's a race to see which one returns first. Most of the time the 404 error finishes first followed by the scripture and then by the bad domain name. But there's no guarantee of that order.

### Using Arrow Function Syntax

[Arrow Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) are a concise way to write short, anonymous functions. You list the arguments, in parentheses if there are more than one, then an arrow in the form of `=>` and then the expression the function should return. If the function needs more than one line you enclose them in braces `{}`.

Here's `FetchUrlAndPrint()` using Arrow Function Syntax and the [`? :` ternary operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator):

```js
function FetchUrlAndPrint(url) {
  fetch(url)
    .then(response => response.ok ? response.json() : console.log(`Error: ${response.status} ${response.statusText}`))
    .then(obj => console.log(obj))
    .catch(err => console.log(`Rejected: ${err.message} | ${err.cause.message}`));
}
```

### Using async await

An [async function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) hides the `Promise` and `.then` syntax behind the `async` and `await` keywords. The function must be designated as `async`. Then it can use `await` to mean "return now but attach the code that follows to the promise that comes from this expression." In this walkthrough we won't go into detail on how this works. But there are good tutorials out on the web. What we will do is show how to write `FetchUrlAndPrint` using `async await`.

```js
async function FetchUrlAndPrint(url) {
  try {
    let response = await fetch(url);
    if (!response.ok) {
      throw Error(`Error: ${response.status} ${response.statusText}`);
    }
    obj = await response.json();
    console.log(`${obj.reference} | ${obj.text}`);
  }
  catch (err) {
    console.log(`Err: ${err.message}`);
  }
}
```

Depending on how much you need to do at each step, this can result in easier-to-read code.

### Exiting Node

You can, of course close the terminal window but if you like using commands, type:

```js
process.exit()
```
