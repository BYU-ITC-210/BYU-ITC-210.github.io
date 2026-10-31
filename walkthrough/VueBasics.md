---
title: Vue Walkthrough
---

[Vue](https://vuejs.org) is a versatile [Front-end framework](https://en.wikipedia.org/wiki/Front-end_web_development) for creating website user interfaces. Vue allows you to start small and add new features as needed. It concentrates on improving the visual aspect of your website (the `view`) while managing manage complex, interactive web pages when used with other modern tools and libraries. 

## Resources

* [VUE guide](https://vuejs.org/guide/introduction.html)

## Checking Your Environment

Before starting this walkthrough, you must have already installed [node.js](https://nodejs.org/en/download) and [NPM (Node Package Manager)](https://www.geeksforgeeks.org/node-js/node-js-npm-node-package-manager/). Verify your installation with the following commands:

### Vue.js Setup

1. Make sure node.js is installed
  ```sh
  node --version
  ```
2. Make sure npm is installed
  ```sh
  npm --version
  ```

---

## Step 1: Create a New Vue Project

In your terminal, navigate to the parent folder where you want your project to live, then run:

```sh
npm create vue@latest
```

The first time you run this, npm may ask permission to install the `create-vue` package. Type `y` and press Enter.

You will then be asked a series of questions. The first is the project name:

```
Project name: › vue-walkthrough
```

Type a name (for example, `vue-walkthrough`) and press Enter. The remaining prompts ask which optional features you want, such as TypeScript, Vue Router, Pinia, Vitest, and ESLint. Use the arrow keys and spacebar to select options, and press Enter to continue.

> **Tip:** For your first project, you should leave every optional *feature* unselected (choose **No**). You can always add features later.

When it asks "Skip all example code and start with a blank Vue project?" select **No**. We will use the example code in this walkthrough.

When the tool finishes, it creates a new folder, named after your project, that contains a ready-to-use starter app.

---

## Step 2: Install the Dependencies

Move into the new project folder:

```sh
cd vue-walkthrough
```

Then install everything the project needs:

```bash
npm install
```

This reads the project's `package.json` file and downloads all required packages into a `node_modules` folder. It may take a minute or two.

---

## Step 3: Run the Project in Development Mode

Start the development server:

```bash
npm run dev
```

You should see output similar to this:

```
  VITE v7.x.x  ready in 300 ms

  ➜  Local:   http://localhost:5173/
```

Open the **Local** address (usually `http://localhost:5173/`) in your web browser. You should see the Vue welcome page.

The development server uses *hot reloading*: if you edit a file such as `src/App.vue` and save it, the browser updates automatically with no need to refresh.

To stop the server press `Ctrl + C`.

## Step 4: Review Your Source Files

The process, so far, created the following files:

|File                            | Purpose |
|--------------------------------|---------|
| public/favicon.ico             | A [favicon](https://www.w3schools.com/html/html_favicon.asp) for your project |
| src/assets/base.css            | Baseline CSS that defines all colors and resets styles to a consistent state |
| src/assets/logo.svg            | The Vue logo |
| src/assets/main.css            | Primary CSS for the project. Imports base.css |
| src/components/icons           | Icons that are used by TheWelcome.vue |
| src/components/HelloWorld.vue  | A Vue component that displays the left side of the home page |
| src/components/TheWelcome.vue  | A Vue component that displays the right side of the home page |
| src/components/WelcomeItem.vue | A Vue component that is used by the right side of the home page to format each part |
| src/App.vue                    | The main page Vue component that puts *HelloWorld* on the left and *TheWelcome* on the right |
| src/main.js                    | JavaScript that loads up the Vue framework |
| .gitignore                     | Tells GIT what not to put into the repo |
| index.html                     | The main page of the website. Loads up *src/main.js* which loads everything else |
| jsconfig.json                  | A data file that tells a code editor, such as VS Code, how to handle the project |
| package.json                   | The manifest for a Node.js project. It describes the project and lists the modules it needs to run. When you type `npm install` it reads this file and installs all of the corresponding modules. |
| package-lock.json              | A record of which packages and versions are actually loaded. When npm traces the dependency list, all loaded packages and their versions get listed here. |
| vite.config.js                 | Configuration file for Vite. Vite is a tool used by `npm run dev` to run your development server and `npm run build` to compile a distribution version of your Vue project. |
| README.md                      | A default README file for your project |

---

## Step 5: Create a Grocery List Component

So far you have only run the starter app. Now you'll build something of your own: a **component** that displays a grocery list.

A component is a reusable piece of the user interface. In Vue, components are usually written as *single-file components*, which are files ending in `.vue`. Each file can hold a `<script>` section (logic), a `<template>` section (HTML), and a `<style>` section (CSS).

Create a new file named `GroceryList.vue` inside the `src/components` folder, and paste in the following contents:

```vue
<script setup>
defineProps({
  items: {
    type: Array,
    required: true
  }
})
</script>

<template>
  <div class="grocery-list">
    <h2>Grocery List</h2>
    <ul>
      <li v-for="item in items" :key="item.name">
        {{ item.name }}: {{ item.quantity }}
      </li>
    </ul>
  </div>
</template>

<style scoped>
.grocery-list {
  max-width: 40em;
  margin: 2rem auto;
}
</style>
```

Here is what each part does:

* `defineProps` declares that this component accepts one input, called `items`, which must be an array. A *prop* is how a parent component passes data down to a child.
* `v-for="item in items"` repeats the `<li>` element once for every entry in the array.
* `:key="item.name"` gives Vue a unique identifier for each list entry so it can update the list efficiently. (This assumes each item name is unique.)
* {% raw %}`{{ item.name }}` and `{{ item.quantity }}`{% endraw %} insert each entry's values into the page.
* `scoped` on the `<style>` tag means the CSS applies only to this component.

Notice that the component contains no grocery data of its own. It simply displays whatever list it is given, which is what makes it reusable.

---

## Step 6: Add a Grocery List to App.vue

Now, create the data. `App.vue` (in the `src` folder) is the root component of your application, so it's a natural place to keep it. In a real application you might `fetch` the data from a web API. 

Open `src/App.vue`. The starter file contains a `<script setup>` section near the top. Replace it with the following:

```vue
<script setup>
import { ref } from 'vue'

const groceries = ref([
  { name: 'Apples', quantity: 6 },
  { name: 'Bread', quantity: 1 },
  { name: 'Milk', quantity: 2 },
  { name: 'Eggs', quantity: 12 },
  { name: 'Bananas', quantity: 5 }
])
</script>
```

This is an array of JSON-style objects. Each entry has two properties:

- **`name`**: the name of the item (a string)
- **`quantity`**: how many to buy (a number)

The array is wrapped in **`ref()`**, which makes it *reactive*. That means if the list changes later (for example, an item is added), Vue automatically updates anything on the page that displays it.

---

## Step 7: Bind the Component to the List

Add the following at the beginning of `<script setup>` section of `src/App.vue`:

```vue
import GroceryList from './components/GroceryList.vue'
```

Replace the `<template>` section of `src/App.vue` with the following:

```vue
<template>
  <GroceryList v-bind:items="groceries" />
</template>
```

There are two new things here:

- **`import GroceryList from './components/GroceryList.vue'`** makes the component available in this file. Because the file uses `<script setup>`, no extra registration is needed.
- **`v-bind:items="groceries"`** *binds* the `groceries` array to the component's `items` prop. This tells Vue to treat the value as a JavaScript expression (the `groceries` variable) rather than plain text.

Save the file (or let it auto-save). If your development server is still running from Step 3, the browser updates automatically. If you stopped it, start it again with `npm run dev`. You should now see a "Grocery List" heading with your five items and their quantities.

**Try it:** change a quantity or add a new `{ name: ..., quantity: ... }` entry to the `groceries` array in `App.vue`, save, and watch the page update without a refresh.

---

This walkthrough is just sufficient to get you started with a Vue project. Lab 5 will include getting data from a web service, sending updated data back to the service, and a form to add new data.