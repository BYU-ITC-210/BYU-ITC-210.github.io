---
title: "Homework 2: CSS"
---
***

*40 points possible*

***

## Resources

* [Basic HTML Page](/HW2/home.html){: download="home.html"} (Your starting point)
* [Mozilla CSS Tutorial](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS){: target="_blank"}
* [W3Schools CSS Tutorial](https://www.w3schools.com/css/){: target="_blank"}

## Introduction
Cascading Style Sheets (CSS) are focused on the the look-and-feel of a website. CSS is a standard way of telling a web browser how to display HTML elements. While HTML provides the content and structure of a site, CSS provides the positioning of elements (where they show up on the page) and their look (e.g., size, color, and even some basic animation such as transitions). If you're not experienced with CSS, we highly recommend going through a tutorial online. Feel free to pick and choose which part of the tutorial to complete, based on your prior experience (e.g., if you haven't done much positioning or worked with classes and IDs then do the tutorial associated with them as they are critical).

For this assignment, your task is to change the look and feel of a [“Basic HTML Page”](/HW2/home.html){: download="home.html"}. There are 3 ways of using CSS: inline, embedded, and linking to an external file.

**External CSS** (best practice) is kept in an external file (ending with .css) that is linked to from the HTML page. This makes it very convenient to maintain since all styling can be found in one place. CSS files can be linked to from multiple pages, so if you make a change to it, all pages that use the external CSS file will update.

**Embedded CSS** is contained within a `<style>` tag that is nested inside the `<head>` element on an HTML page.

**Inline CSS** involves writing CSS styling attributes in the `style` attribute of an HTML tag. This means inline CSS only applies to the HTML element it is found in (and all nested tags). Use inline sparingly. It can make code hard to maintain because the CSS becomes scattered throughout the code.

Even though it is best practice to use an external file, in this assignment we want you to get practice using all three methods, since you’ll come across them all in the future.

## Procedures

To complete this assignment you will need to do the following:

### CSS Locations

1. Float the image to the right-hand side using *inline* CSS. **[2 points]**
2. Use embedded CSS within the HTML `<head>` of the page to fix the position of the page `<header>` so that it stays at the top of the browser view even if the user scrolls through the page. You may need to add more text to make the page scrollable; [lipsum.com](https://lipsum.com) is a good source of filler text. **[2 points]**
3. Link to an external CSS page that includes the rest of the CSS code (see points below) **[2 points for linking to the external file correctly]**

### External CSS

Use an external CSS file to implement at least the following 14 CSS properties (you may use more if you would like):

1. 2 different positioning elements other than those specified above (eg. absolute, relative, floating, fixed) **[4 points]**
2. 1 property that changes an HTML element based on an ID (note that you'll have to add the id to the HTML file) **[2 points]**
3. 1 property that changes an HTML element based on a class (note that you'll have to add the class to the HTML file) **[2 points]**
4. 1 CSS transition **[2 points]**
5. 1 different font **[2 points]**
6. Make image(s) bigger on hover **[2 points]**
7. Any other 7 CSS properties you’d like (please list them on your HTML page) **[14 points]**

### Validate your code

Use the [W3C HTML Validator](https://validator.w3.org/#validate_by_upload) and the [W3C CSS Validator](https://jigsaw.w3.org/css-validator/#validate_by_upload) to check for errors. Fix all errors **and** warnings for full credit. **[6 points]**

> **Note:** Feel free to add additional HTML tags and content to the original "Basic HTML Page" to fulfill the above requirements. Having more content to work with may make it easier.

## Submission Instructions
This homework should be submitted via LearningSuite. Copy all files into a .ZIP folder upload the .ZIP to the LearningSuite assignment.

The .zip folder should contain the following:
* The modified HTML file
* The external CSS file you created
* Screenshots of the HTML and CSS validations showing no errors.

> **Note:** If you add images or other media to your web page, be sure to include them in the zip file, so the page can be viewed with everything intact.