---
title: "Homework 4: HTML Forms, GET and POST"
---

*40 points possible. 4 points per question.*

## Objectives

* Understand how to use bare HTML forms (without JavaScript)
* Learn the difference between HTTP GET and POST verbs
* Introduce the internals of HyperText Transport Protocol (HTTP)

## Resources

* Computer and web browser
* Text editor suitable for HTML (typically Microsoft Visual Studio Code)
* Article: [Anatomy of an HTTP Request](https://betterprogramming.pub/the-anatomy-of-an-http-request-728a469ecba9)

## How to Complete and Submit the Homework
*Please follow these instructions*

1. Download the [answersheet](HW4-GET-and-POST-Answersheet.txt){: download="HW4-GET-and-POST-Answersheet.txt"} and load it into a text editor such as VS Code.
2. Perform each of the following steps. Write or paste your answers into the answer sheet.
3. Submit your answer sheet to LearningSuite for grading.

## Steps: Do the Following

### 1. Create an HTML Page with a simple form

* Open VS code (or your favorite text editor) and create a new HTML document called `form.html`.
* Give the page a title, some introductory text, and a form with multiple input elements.
* Include a submit button in the form.

Here's a simple example.

```html
<html>
<head>
  <title>Bridge</title>
</head>
<body>
  <h1>Bridge</h1>
  <div>
    Stop! Who would cross the Bridge of Death must
    answer me these questions three, ere the other
    side he see.
  </div>
  <form>
    <label for="name">What is your name:</label>
    <input type="text" id="name"/><br/>
    <label for="quest">What is your quest:</label>
    <input type="text" id="quest"/><br/>
    <label for="color">What is your favorite color:</label>
    <input type="text" id="color"/><br/>
    <input type="submit"/>
  </form>
</body>
</html>
```

* Save the page to a folder on your local drive.

### 2. Set the form action

The URL, `https://echo.dicax.org`, is a handy tool that echoes everything in any HTTP request it receives directly back to the caller.

* In your page, set the `action` attribute on the `<form>` element to `https://echo.dicax.org`.

```html
  <form action="https://echo.dicax.org">
```

* Save the changes

### 3. Enter something into the form and view the results

* Open the page in a web browser. You don't need to load it onto a server. Just launch the .html file from your file explorer/finder.
* Enter answers into your form.
* Click `submit`.

#### Anatomy of an HTTP Request

Look at the request (that was echoed to your browser):

* The first line of an HTTP request starts with the verb, followed by the path and query string of the URL, followed by the HTTP version (typically "HTTP/1.1).
* Next are the headers, one header per line. Each header has a name followed by a colon followed by the header value.
* Next is a blank line.
* After the blank line are the body contents. GET requests do not have a body, most other requests (such as PUT and POST) do.

#### Answer Questions
Answer the following questions on the answer sheet:

<p>
<div class="question">Which HTTP verb (GET, POST, PUT, etc.) was used? (Hint: Look at the first line.)</div>
<div class="question">Where are your form responses passed to the server?</div>
<div class="question">Suppose you weren't using a service that echoes the request back. Would you be able to see your form responses anywhere in the browser? (Hint: Look in your browser address bar.)</div>
</p>

### 3. Change the Form method to "post"

* In your page, set the `method` attribute on the `<form>` element to `post`.


```html
  <form action="https://echo.dicax.org" method="post">
```

* Save your changes.
* Refresh the page in your browser.
* Enter answers.
* Click `submit`

#### Answer Questions
Answer the following questions on the answer sheet:

<p>
<div class="question">Which HTTP verb was used this time?</div>
<div class="question">Where are your form responses passed to the server this time?</div>
<div class="question">Suppose you weren't using a service that echoes the request back. Would you be able to see your form responses anywhere in the browser?</div>
<div class="question">What is the data format of the form submission? (Hint: see the `Content-Type` header.)</div>
</p>

### 4. Change the Form encoding to "multipart/form-data"

* In your page, set the `enctype` attribute on the `<form>` element to `multipart/form-data`.

```html
  <form action="https://echo.dicax.org" method="post"
    enctype="multipart/form-data">
```

> The `multipart/form-data` encoding is appropriate when your form includes an `<input type="file">` (file upload) input control.

* Save your changes.
* Refresh the page in your browser.
* Enter answers into your form.
* Click `submit`

#### Answer Questions
Answer the following questions on the answer sheet:

<p>
<div class="question">What is the data format of the form submission this time?</div>
<div class="question">Is the data different or just its encoding?</div>
</p>