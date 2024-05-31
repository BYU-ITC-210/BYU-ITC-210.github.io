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

2. Creating autonomous elements: A custom element is implemented as a class which extends HTMLElement (in the case of autonomous elements).

```js


// myelement.js

class MyElement extends HTMLElement {
  constructor() {
    super();

    // Create a shadow root
    this.attachShadow({ mode: 'open' });

    // Create wrapper element
    const wrapper = document.createElement('div');
    wrapper.setAttribute('class', 'wrapper');

    // Create input element
    const input = document.createElement('input');
    input.type = 'text';
    input.placeholder = 'Enter text';

    // Create submit button
    const submitButton = document.createElement('button');
    submitButton.textContent = 'Submit';
    submitButton.setAttribute('class', 'submit-button');

    // Create clear button
    const clearButton = document.createElement('button');
    clearButton.textContent = 'Clear';
    clearButton.setAttribute('class', 'clear-button');

    // Create style element
    const style = document.createElement('style');
    style.textContent = `
      .submit-button {
        background-color: white;
      }
      .submit-button:hover {
        background-color: lightblue;
      }
      .clear-button {
        background-color: white;
      }
      .clear-button:hover {
        background-color: lightcoral;
      }
    `;

    // Append elements to the shadow root
    this.shadowRoot.append(style, wrapper);
    wrapper.append(input, submitButton, clearButton);

    // Attach event listener to the submit button
    submitButton.addEventListener('click', () => {
      submitButton.style.backgroundColor = 'blue';
      this.dispatchEvent(new CustomEvent('submit', {
        detail: input.value,
      }));
    });

    // Attach event listener to the clear button
    clearButton.addEventListener('click', () => {
      input.value = '';
      clearButton.style.backgroundColor = 'red';
    });
  }
}

```
## Custom element lifecycle callbacks: 
Once the `custom element` is registered, the browser will invoke specific methods of your class when the element is interacted with in certain ways.

Custom element lifecycle callbacks include:

* `connectedCallback()`: called each time the element is added to the document. The specification recommends that, as far as possible,               developers should implement custom element setup in this callback rather than the constructor.
*  `disconnectedCallback()`: called each time the element is removed from the document.
*  `adoptedCallback()`: called each time the element is moved to a new document.
*  `attributeChangedCallback()`: called when attributes are changed, added, removed, or replaced.

 **Here's a minimal custom element that logs these lifecycle events**:
  
```js
  class MyElement extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
    // element created
  }

  connectedCallback() {
    console.log('Element added to the page.');
  }

  disconnectedCallback() {
    console.log('Element removed from the page.');
  }

  attributeChangedCallback(name, oldValue, newValue) {
    console.log(`Attribute ${name} changed from ${oldValue} to ${newValue}`);
  }

  adoptedCallback() {
    console.log('Element moved to a new page.');
  }
}
```

## Register the element:
To make a `custom element` available in a page, call the `define()` method of `Window.customElements`.The `define()` method takes the following arguments:
* `name`
The name of the element. This must start with a lowercase letter, contain a hyphen,    and satisfy certain other rules listed in the specification's definition of a [valid    name](https://html.spec.whatwg.org/multipage/custom-elements.html#valid-custom-element-name).
* `constructor`
The custom element's constructor function.
* `options`
Only included for customized built-in elements, this is an object containing a         single property extends, which is a string naming the built-in element to extend.

  ```js
  // let the browser know that <my-element> is served by our new class
  customElements.define("my-element", MyElement);
  ```
## Using a custom element
```html

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Custom Element Demo</title>
  <script src="myelement.js" defer></script>
</head>
<body>
  <form id="custom-form">
    <my-element></my-element>
    <button type="submit">Send</button>
  </form>

  <script>
    document.getElementById('custom-form').addEventListener('submit', function(event) {
      event.preventDefault();
      const myElement = document.querySelector('my-element');
      myElement.addEventListener('submit', function(e) {
        alert('Submitted value: ' + e.detail);
      });
    });
  </script>
</body>
</html>


```
