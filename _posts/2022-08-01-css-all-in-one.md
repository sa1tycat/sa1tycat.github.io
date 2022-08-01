---
title: CSS一次通
author: saltycat
date: 2022-08-01 19:39:01 +0800
categories: [CTF]
tags: [CSS]
math: true
comments: true
mermaid: true
typora-root-url: ../../sa1tycat.github.ioyu
---



## Snytax and Selectors

### `<link>` Link Element

The `<link>` element is used to link HTML documents to external resources like CSS files. It commonly uses:

- `href` attribute to specify the URL to the external resource
- `rel` attribute to specify the relationship of the linked document to the current document
- `type` attribute to define the type of content being linked

```css
<!-- How to link an external stylesheet with href, rel, and type attributes -->
 
<link href="./path/to/stylesheet/style.css" rel="stylesheet" type="text/css">
```

### Purpose of CSS

CSS, or Cascading Style Sheets, is a language that is used in combination with HTML that customizes how HTML elements will appear. CSS can define styles and change the layout and design of a sheet.

### Write CSS in Separate Files

CSS code can be written in its own files to keep it separate from the HTML code. The extension for CSS files is **.css**. These can be linked to an HTML file using a `<link>` tag in the `<head>` section.

```css
<head>
  <link href="style.css" type="text/css" rel="stylesheet">
</head>
```

### Write CSS in HTML File

CSS code can be written in an HTML file by enclosing the code in `<style>` tags. Code surrounded by `<style>` tags will be interpreted as CSS syntax.

```css
<head>
  <style>
    h1 {
      color: blue;
    }
  </style>
</head>
```

### Inline Styles

CSS styles can be directly added to HTML elements by using the `style` attribute in the element’s opening tag. Each style declaration is ended with a semicolon. Styles added in this manner are known as *inline styles*.

```css
<h2 style="text-align: center;">Centered text</h2>
 
<p style="color: blue; font-size: 18px;">Blue, 18-point text</p>
```

### Separating HTML code from CSS code

It is common practice to separate content code in HTML files from styling code in CSS files. This can help make the code easier to maintain, by keeping the syntax for each file separate, and any changes to the content or styling can be made in their respective files.

### Class and ID Selectors

CSS classes can be reusable and applied to many elements. Class selectors are denoted with a period `.` followed by the class name. CSS ID selectors should be unique and used to style only a single element. ID selectors are denoted with a hash sign `#` followed by the id name.

```css
/* Selects all elements with class="column" */
.column {
}
 
/* Selects element with id="first-item" */
#first-item {
}
```

### Groups of CSS Selectors

Match multiple selectors to the same CSS rule, using a comma-separated list. In this example, the text for both `h1` and `h2` is set to red.

```css
h1, h2 {
  color: red;
}
```

### Selector Chaining

CSS *selectors* define the set of elements to which a CSS rule set applies. For instance, to select all `<p>` elements, the `p` selector can be used to create style rules.

### Chaining Selectors

CSS selectors can be chained so that rule sets apply only to elements that match all criteria. For instance, to select `<h3>` elements that also have the `section-heading` class, the selector `h3.section-heading` can be used.

```css
/* Select h3 elements with the section-heading class */
h3.section-heading {
  color: blue;
}
 
/* Select elements with the section-heading and button class */
.section-heading.button {
  cursor: pointer;
}
```

### CSS Type Selectors

CSS *type selectors* are used to match all elements of a given type or tag name. Unlike for HTML syntax, we do not include the angle brackets when using type selectors for tag names. When using type selectors, elements are matched regardless of their nesting level in the HTML.

```css
/* Selects all <p> tags */
p {
}
```

### CSS class selectors

The CSS class selector matches elements based on the contents of their `class` attribute. For selecting elements having `calendar-cell` as the value of the `class` attribute, a `.` needs to be prepended.

```css
.calendar-cell {
  color: #fff;
}
```

### HTML attributes with multiple values

Some HTML attributes can have multiple attribute values. Multiple attribute values are separated by a space between each attribute.

```css
<div class="value1 value2 value3"></div>
```

### Selector Specificity

Specificity is a ranking system that is used when there are multiple conflicting property values that point to the same element. When determining which rule to apply, the selector with the highest specificity wins out. The most specific selector type is the ID selector, followed by class selectors, followed by type selectors. In this example, only `color: blue` will be implemented as it has an ID selector whereas `color: red` has a type selector.

```css
h1#header {
  color: blue;
} /* implemented */
 
h1 {
  color: red;
} /* Not implemented */
```

### CSS ID selectors

The CSS ID selector matches elements based on the contents of their `id` attribute. The values of `id` attribute should be unique in the entire DOM. For selecting the element having `job-title` as the value of the `id` attribute, a `#` needs to be prepended.

```css
#job-title {
  font-weight: bold;
}
```

### CSS descendant selector

The CSS *descendant* selector combinator is used to match elements that are descended from another matched selector. They are denoted by a single space between each selector and the descended selector. All matching elements are selected regardless of the nesting level in the HTML.

```css
div p { }
 
section ol li { }
```



## Visual Rules

### CSS declarations

In CSS, a *declaration* is the key-value pair of a CSS property and its value. CSS declarations are used to set style properties and construct rules to apply to individual or groups of elements. The property name and value are separated by a colon, and the entire declaration must be terminated by a semi-colon.

```css
/* 
CSS declaration format:
property-name: value;
*/
 
/* CSS declarations */
text-align: center;
color: purple;
width: 100px;
```

### Font Size

The `font-size` CSS property is used to set text sizes. Font size values can be many different units or types such as pixels.

```css
font-size: 30px;
```

### Background Color

The `background-color` CSS property controls the background color of elements.

```css
background-color: blue;
```

### `!important` Rule

The CSS `!important` rule is used on declarations to override any other declarations for a property and ignore selector specificity. `!important` rules will ensure that a specific declaration always applies to the matched elements. However, generally it is good to avoid using `!important` as bad practice.

```css
#column-one {
  width: 200px !important;
}
 
.post-title {
  color: blue !important;
}
```

### Opacity

The `opacity` CSS property can be used to control the transparency of an element. The value of this property ranges from `0` (transparent) to `1` (opaque).

```css
opacity: 0.5;
```

### Font Weight

The `font-weight` CSS property can be used to set the weight (boldness) of text. The provided value can be a keyword such as `bold` or `normal`.

```css
font-weight: bold;
```

### Text Align

The `text-align` CSS property can be used to set the text alignment of inline contents. This property can be set to these values: `left`, `right`, or `center`.

```css
text-align: right;
```

### CSS Rule Sets

A CSS rule set contains one or more selectors and one or more declarations. The selector(s), which in this example is `h1`, points to an HTML element. The declaration(s), which in this example are `color: blue` and `text-align: center` style the element with a property and value. The rule set is the main building block of a CSS sheet.

```css
h1 {
  color: blue;
  text-align: center;
}
```

### Setting foreground text color in CSS

Using the `color` property, foreground text color of an element can be set in CSS. The value can be a valid color name supported in CSS like `green` or `blue`. Also, 3 digit or 6 digit color code like `#22f` or `#2a2aff` can be used to set the color.

```css
p {
  color : #2a2aff ;
}
 
span {
  color : green ;
}
```

### Resource URLs

In CSS, the `url()` function is used to wrap resource URLs. These can be applied to several properties such as the `background-image`.

```css
background-image: url("../resources/image.png");
```

### Background Image

The `background-image` CSS property sets the background image of an element. An image URL should be provided in the syntax `url("moon.jpg")` as the value of the property.

```css
background-image: url("nyan-cat.gif");
```

### Font Family

The `font-family` CSS property is used to specify the typeface in a rule set. Fonts must be available to the browser to display correctly, either on the computer or linked as a web font. If a font value is not available, browsers will display their default font. When using a multi-word font name, it is best practice to wrap them in quotes.

```css
h2 {
  font-family: Verdana;
}
 
#page-title {
  font-family: "Courier New";
}
```

### Color Name Keywords

Color name keywords can be used to set color property values for elements in CSS.

```css
h1 {
  color: aqua;
}
 
li {
  color: khaki;
}
```



## The Box Model

### CSS Margin Collapse

CSS *margin collapse* occurs when the top and bottom margins of blocks are combined into a single margin equal to the largest individual block margin.

Margin collapse only occurs with vertical margins, not for horizontal margins.

```css
/* The vertical margins will collapse to 30 pixels
instead of adding to 50 pixels. */
.block-one {
  margin: 20px;
}
 
.block-two {
  margin: 30px;
}
```

### CSS `auto` keyword

The value `auto` can be used with the property `margin` to horizontally center an element within its container. The `margin` property will take the width of the element and will split the rest of the space equally between the left and right margins.

```css
div {
  margin: auto;
}
```

### Dealing with `overflow`

If content is too large for its container, the CSS `overflow` property will determine how the browser handles the problem.

By default, it will be set to `visible` and the content will take up extra space. It can also be set to `hidden`, or to `scroll`, which will make the overflowing content accessible via scroll bars within the original container.

```css
small-block {
  overflow: scroll;
}
```

### Height and Width Maximums/Minimums

The CSS `min-width` and `min-height` properties can be used to set a minimum width and minimum height of an element’s box. CSS `max-width` and `max-height` properties can be used to set maximum widths and heights for element boxes.

```css
/* Any element with class "column" will be at most 200 pixels wide, despite the width property value of 500 pixels. */
 
.column {
  max-width: 200px;
  width: 500px;
}
```

### The `visibility` Property

The CSS `visibility` property is used to render `hidden` objects invisible to the user, without removing them from the page. This ensures that the page structure and organization remain unchanged.

```css
.invisible-elements {
  visibility: hidden;
}
```

### The property `box-sizing` of CSS *box model*

The CSS *box model* is a box that wraps around an HTML element and controls the design and layout. The property `box-sizing` controls which aspect of the box is determined by the `height` and `width` properties. The default value of this property is `content-box`, which renders the actual size of the element including the content box; but not the paddings and borders. The value `border-box`, on the other hand, renders the actual size of an element including the content box, paddings, and borders.

```css
.container {
  box-sizing: border-box;
}
```

### CSS `box-sizing: border-box`

The value `border-box` of the `box-sizing` property for an element corresponds directly to the element’s total rendered size, including padding and border with the `height` and `width` properties.

The default value of the `border-box` property is `content-box`. The value `border-box` is recommended when it is necessary to resize the `padding` and `border` but not just the content. For instance, the value `border-box` calculates an element’s `height` as follows: `height = content height + padding + border`.

```css
#box-example {
  box-sizing: border-box;
}
```
