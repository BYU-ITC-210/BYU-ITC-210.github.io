---
title: VUE & Front-End Frameworks Walkthrough
---

[Vue](https://v2.vuejs.org/v2/guide/#) is a versatile framework for creating website user interfaces. Vue allows you to start small and add new features as needed. It concentrates on improving the visual aspect of your website (the 'view'). Still, it can also manage complex, interactive web pages when used with other modern tools and libraries. 

[Front-end frameworks](https://en.wikipedia.org/wiki/Front-end_web_development) give pre-written code components to make interface building easier. It makes creating and maintaining well-organized online projects easy while working with HTML, CSS, and JavaScript.

## Resources

* [Vue.js guide](https://v2.vuejs.org/v2/guide/#)
* [Mozilla Express/Node Introduction](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/Introduction)


## Installation

Browse to the [Node.js installation page](https://nodejs.org/en/download/) and download the appropriate installer for your platform (Windows, Mac, Linux, Docker, etc.). Run the installer to get things set up. For our work, you will not need the ability to build C++ modules or Chocolatey.

## Experimenting with the Node console

Launch Node.js either from your start menu or by opening a console window and typing `node`.

By now, you've done some coding in JavaScript so you're familiar with the syntax. Try typing some JavaScript expressions at the console. Here are some examples:

```
2+3
console.log("Hello")
process.version
process.arch
process.platform
```

You may have noticed that `console.log()` prints a message and then the word "undefined". Node.js treats everything you type as an expression to be evaluated. In JavaScript, functions that don't return anything result in the `undefined` value. Sort of like `void` in **C** or **C++**.

Create and execute a simple function:

```
function Hello() {
  console.log('Hello, world!');
}
```

Run it by typing `Hello()`

## A "Hello World" Web Server

You can type the following code directly into the Node.js console. However, you have to be careful about typos. The alternative is to type it into a text editor, save it to a file called `HelloServer.js` and then, from a console, type `Node HelloServer.js`

```
var http = require("http");

function HelloServer(request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.write("Hello World!");
  response.end();
}

http.createServer(HelloServer).listen(8080);
```

Now, browse to `http://localhost:8080` and you'll see your simple text page.

## Exiting

When you're finished, you can exit the Node.js environment by typing:

```
process.exit();
```

If you have a web server running it will be shut down.
