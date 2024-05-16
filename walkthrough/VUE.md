---
title: VUE & Front-End Frameworks Walkthrough
---

##Introducing Front-end Frameworks and Vue.js

[Vue](https://v2.vuejs.org/v2/guide/#) is a versatile framework for creating website user interfaces. Vue allows you to start small and add new features as needed. It concentrates on improving the visual aspect of your website (the `view`). Still, it can also manage complex, interactive web pages when used with other modern tools and libraries. 

[Front-end frameworks](https://en.wikipedia.org/wiki/Front-end_web_development) give pre-written code components to make interface building easier. It makes creating and maintaining well-organized online projects easy while working with HTML, CSS, and JavaScript.

## Resources

[Vue guide](https://v2.vuejs.org/v2/guide/#)

## Installation

### Vue.js Setup

1. Make sure node.js is installed
   ```sh
   node --version
   ```
2. Make sure [npm](https://kinsta.com/knowledgebase/what-is-npm/) is installed
   ```sh
   npm --version
   ```
3. Install the VUE framework using this command: 
   ```sh
   sudo npm install -g @vue/cli 
   ```
   After installing, run the following command to make sure Vue has been installed:

   ```sh
   vue --version
   ```
## Creating a Data Model as a JavaScript Object
In Vue.js, the data model is typically defined as a JavaScript object within the Vue instance. Here’s an example:

```JS
//This line creates a new Vue instance and assigns it to the variable app. 
var app = new Vue({
//This line tells Vue to bind this instance to the element in our HTML with the id of app. 
  el: '#app',
//This is the data object, where we define all the data for our app.
  data: {
    message: 'Hello Vue!'
  }
})
```
## Creating a View on the Data
In Vue.js, the view is the HTML that the user sees. Vue.js uses a template syntax that allows you to bind the rendered DOM to the underlying Vue instance’s data. Here’s how you can create a view for the above data model:

```HTML
<div id="app">
  {{ message }}
</div>
```
## [MVVM pattern](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93viewmodel)



