---
title: Web Components Walkthrough
---

Reusing code is beneficial, but complex custom HTML, styles, and scripts can make it challenging. [Web Components](https://developer.mozilla.org/en-US/docs/Web/API/Web_Components#concepts_and_usage) addresses this by offering three technologies to create versatile, encapsulated custom elements, enabling easy reuse without code conflicts.

A key feature of Web Components is the ability to create [custom elements](https://javascript.info/custom-elements), allowing developers to define new HTML elements with specific behaviors, extending the standard set available in the browser.

Encapsulation is crucial for custom elements to ensure they work seamlessly when reused across different web pages.[Shadow DOM](https://javascript.info/shadow-dom) enables this by attaching a hidden DOM tree to an element, protecting its internal implementation from the page's JavaScript and CSS interference.

Reusing markup structures repeatedly is simplified by the HTML [templates element](https://javascript.info/template-element). This element's contents are not rendered in the DOM but can be referenced and instantiated via JavaScript, making the process more efficient and organized.

## Create custom elements

1. Creating a JavaScript file

`myelement.js`

2. Creating autonomous elements

```js
// Create a class for the element
class MyElement extends HTMLElement {
  constructor() {
    // Always call super first in the constructor
    super();
  }
```
3. Register the element:

To make a `custom element` available in a page, call the `define()` method of `Window.customElements`.

The `define()` method takes the following arguments:

`name`
The name of the element. This must start with a lowercase letter, contain a hyphen, and satisfy certain other rules listed in the specification's definition of a valid name.

`constructor`
The custom element's constructor function.

`options`
Only included for customized built-in elements, this is an object containing a single property extends, which is a string naming the built-in element to extend.

```js
// let the browser know that <my-element> is served by our new class
customElements.define("my-element", MyElement);
```
 

