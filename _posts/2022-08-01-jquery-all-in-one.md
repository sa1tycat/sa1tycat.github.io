---
title: jQuery一次通
author: saltycat
date: 2022-08-01 19:45:56 +0800
categories: [CTF]
tags: [jQuery]
math: true
comments: true
mermaid: true
typora-root-url: ../../sa1tycat.github.ioyu
---



## Introduction

### jQuery streamlines dynamic behavior

*jQuery* is a JavaScript library that streamlines the creation of dynamic behavior with predefined methods for selecting and manipulating DOM elements. It offers a simplified approach to implementing responsiveness and requires fewer lines of code to assign behaviors to DOM elements than traditional JavaScript methods.

```javascript
//Selecting DOM elements and adding an event listener in JS
const menu = document.getElementById('menu');
const closeMenuButton = document.getElementById('close-menu-button');
closeMenuButton.addEventListener('click', () => {
    menu.style.display = 'none';
});
 
//Selecting DOM elements and adding an event listener in jQuery
$('#close-menu-button').on('click', () =>{
  $('#menu').hide();  
});
```

### jQuery document ready

JavaScript code runs as soon as its file is loaded into the browser. If this happens before the DOM (Document Object Model) is fully loaded, unexpected issues or unpredictable behaviors can result.

jQuery’s `.ready()` method waits until the DOM is fully rendered, at which point it invokes the specified callback function.

```javascript
$(document).ready(function() {
  // This code only runs after the DOM is loaded.
  alert('DOM fully loaded!');
});
```

### jquery object variables start with

*jQuery objects* are typically stored in variables where the variable name begins with a `$` symbol. This naming convention makes it easier to identify which variables are jQuery objects as opposed to other JavaScript objects or values.

```javascript
//A variable representing a jQuery object
const $myButton = $('#my-button');
```

### jQuery CDN Import

*jQuery* is typically imported from a CDN (Content Delivery Network) and added at the bottom of an HTML document in a `<script>` tag before the closing `</body>` tag.

The jQuery `<script>` tag must be listed before linking to any other JavaScript file that uses the jQuery library.

```html
<html>
  <head></head>
  <body>
    <!-- HTML code -->
    
    <!--The jQuery library-->
    <script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
 
    <!--Site-specific JavaScript code using jQuery-->
    <script src="script.js"></script>
  </body>
</html>
```



## Event Handlers

### jquery on event listeners

jQuery `.on()` event listeners support all common browser *event types* such as mouse events, keyboard events, scroll events and more.

```javascript
// A mouse event 'click'
$('#menu-button').on('click', () => {
  $('#menu').show();
});
 
// A keyboard event 'keyup'
$('#textbox').on('keyup', () => {
  $('#menu').show();
});
 
// A scroll event 'scroll'
$('#menu-button').on('scroll', () => {
  $('#menu').show();
});
```

### jquery event object

In a jQuery event listener, an *event object* is passed into the event handler callback when an event occurs. The *event object* has several important properties such as `type` (the event type) and `currentTarget` (the individual DOM element upon which the event occurred).

```javascript
// Hides the '#menu' element when it has been clicked.
$('#menu').on('click', event => {
  // $(event.currentTarget) refers to the '#menu' element that was clicked.
  $(event.currentTarget).hide();
});
```

### jquery event.currentTarget attribute

In a jQuery event listener callback, the `event.currentTarget` attribute only affects the individual element upon which the event was triggered, even if the listener is registered to a group of elements sharing a class or tag name.

```javascript
// Assuming there are several elements with the 
// class 'blue-button', 'event.currentTarget' will
// refer to the individual element that was clicked.
$('.blue-button').on('click', event => {
  $(event.currentTarget).hide();
});
```

### jquery on method chaining

*jQuery* `.on()` event listener methods can be chained together to add multiple events to the same element.

```javascript
// Two .on() methods for 'mouseenter' and 
// 'mouseleave' events chained onto the 
// '#menu-button' element.  
$('#menu-button').on('mouseenter', () => {
  $('#menu').show();
}).on('mouseleave', () => {
  $('#menu').hide();
});
```

### jquery on method

The jQuery `.on()` method adds *event listeners* to jQuery objects and takes two arguments: a string declaring the event to listen for (such as ‘click’), and the event handler callback function. The `.on()` method creates an event listener that detects when an event happens (for example: when a user clicks a button), and then calls the *event handler* callback function, which will execute any defined actions after the event has happened.

```javascript
//The .on() method adds a 'click' event to the '#login' element. When the '#login' element is clicked, the callback function will be executed, which reveals the '#login-form' to the user.
 
$('#login').on('click', () => {
  $('#login-form').show();  
});
```



## Traversing the DOM

### jQuery children

The jQuery `.children()` method returns all child elements of a selected parent element.

This method only applies to the direct children of the parent element, and not deeper descendents.

In the example code, `$('.parent').children()` would select all the `.item` elements.

```html
<div class="parent">
  <div class="item">Child 1</div>
  <div class="item">Child 2</div>
  <div class="item">Child 3</div>
</div>
```

### jQuery .parent

The jQuery `.parent()` method returns the parent element of a jQuery object.

```html
<ul>ul <!-- this is the parent of li's one, two, six and ul three -->
  <li class="one">li</li>
  <li class="two">li</li>
  <ul class="three"> <!-- this is the parent of li's four and five -->
    <li class="four">li</li>
    <li class="five">li</li>
  </ul>
  <li class="six">li</li>
</ul>
```

### jQuery .next

The jQuery `.next()` method targets the next element that shares the same parent element as the original element.

In the following HTML code, the element returned by `$('.two').next()` would be `<li class="three">Item three</li>`.

```html
<ul>
  <li class="one">Item one</li>
  <li class="two">Item two</li>
  <li class="three">Item three</li>
</ul>
```

### jQuery .find()

In jQuery, the `.find()` method will find and return all descendent elements that match the selector provided as an argument.

This code block shows a snippet of HTML that has a simple shopping list. Using jQuery, the list items inside the shopping list can be selected. The `listItems` variable will be a jQuery object that contains the two list items from the shopping list.

```javascript
/*
In HTML:
<ul id='shopping-list'>
    <li class='list-item'>Flour</li>
  <li class='list-item'>Sugar</li>
</ul>
*/
 
// jQuery:
const listItems = $('#shopping-list').find('.list-item');
```

### jQuery .siblings

The jQuery `.siblings()` method targets all of the sibling elements of a particular element.

`.siblings()` can be used to add a `selected` class to an element on click and remove it from all of its sibling elements, ensuring that only one element appears as “selected” at one time.

```javascript
$('.choice').on('click', event => {
  // Remove the 'selected' class from any siblings
  $(event.currentTarget).siblings().removeClass('selected');
  // Adds 'selected' class to that element only.
  $(event.currentTarget).addClass('selected');
});
```

### jQuery .closest

The jQuery `.closest()` method travels up through the DOM tree to find the first (and closest) ancestor element matching a selector string.
