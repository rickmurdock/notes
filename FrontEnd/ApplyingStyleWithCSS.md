# Basic CSS Selectors  

In this lesson, we will focus on three essential selectors: type, ID, and class. In CSS, selectors allow for the targeted styling of CSS elements.

## Type Selector  

Type selectors allow us to select all of the instances of a particular tag. If for instance, I want to select every h1 tag in the document, I'd use the `h1` selector. Any styles applied to that type selector would affect *every `h1` on the page*. Type selectors are intended to make sweeping changes to elements. We can use them to set default styles for tags. Type selectors correspond directly to HTML tags. To select all `<p>` tags, the type selector is `p`, to select all `<div>` tags, the type selector is `div`, etc.

For example, the `h1` type selector selects all `h1` tags on the page and makes them the color blue.

```css
h1 {
  color: blue;
}
```

This type selector (`p`) removes all the padding from paragraphs.

```css
p {
  padding: 0;
}
```

This final example selects unordered lists and makes them block level elements. It also gives them some margin top.

```css
ul {
  display: block;
  margin-top: 1em;
}
```

Any [HTML tag](https://www.quackit.com/html/tags/) can be used as a selector in CSS.

## Class Selector  

Class selectors target elements based on the value of their `class` attribute. In HTML, tags that have a class, such as `<div class="home">`, can be targeted in CSS with statements such as:

```css
.home {
  color: red;
}
```

Sometimes it's not enough to target an element using type selection alone. Take the following set of elements as an example:

```html
<p>Regular text...</p>
<p>Regular text...</p>
<p>Regular text...</p>
<p>Special text...</p>
<p>Regular text...</p>
```

Let's say we have some standard styles we want to apply to the p tag here, but we want to add a special style to the Special text... paragraph. This is where CSS Class selection comes in super handy. Let's add a class to our HTML and see how we can style it.

```html
<p>Regular text...</p>
<p>Regular text...</p>
<p>Regular text...</p>
<p class="special">Special text...</p>
<p>Regular text...</p>
```

Now we can select just the specific `p` by using the CSS Class selection. To do that you'll use the `.` character followed by the name you provided as the value for the class attribute.

```css
.special {
  font-style: italic;
}
```

## ID Selector  

ID selectors allow you to target a unique element. Each element can only have one ID. Each page can only have one element with the ID. For example, in HTML, `<div>` tags which have IDs are unique to the page.

In CSS, these elements are denoted with a `#`, such as:

```css
#header {
  background-color: yellow;
}
```

In the HTML, the ID selector would appear as:

```html
<header id="header">
```

Remember that there is nothing that you can do with an ID that you can't do with a class. The main difference is that IDs are unique and there can only be one on the page. With this in mind, you'll likely want to use classes more often than IDs.

## Closing  

There are many types of selectors, the lesson today focused on basic selectors, type, ID, and Class. In CSS, selectors target HTML elements for styling. They are a part of a CSS rule that allows for specified application of styles to an HTML document.

## Additional Resources  

* [MDN](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS/Selectors)

---

# CSS Syntax & Declarations  

After you've selected an element you'll want to apply styles to it, and likely more than one. Let's take a look at how you apply a specific style to an element as well as what it looks like to apply multiple styles.

![css-rules.jpg](./images/css-rules.jpg)

## Declaration Blocks  

When you apply styles to CSS, these are called CSS declarations and can be grouped into CSS declaration blocks.

Here is what a CSS declaration block might look like:

```css
h1 {
  font-style: italic;
  font-size: 15px;
  color: #ebedef;
}
```

As you can see, we first use type selection to select our `h1` then create a declaration block with three separate CSS declarations in it.

### Syntax  

CSS declarations are composed of a property/value pair.

```css
/* Selector */
h1 {
  /* Property : Value */
  color: red;
}
```

The declaration block is always surround by curly braces (`{}`) and can contain one or more declarations separated by semicolons (`;`).

```css
.selector {
  /* declaration block start */
  display: inline-block;
  width: 50%;
  height: 50px;
  background-color: #ffffff;
} /* declaration block end */
```

Declaration blocks are always surrounded by curly braces.

## Closing  

In this lesson we looked at CSS selectors and declaration blocks. We covered how to make a selection and wrap the declarations inside of curly brackets, creating a declaration block.

## Additional Resources  

* [CSS Syntax](https://developer.mozilla.org/en-US/docs/Web/CSS/Syntax)

---

# How Precedence Affects CSS Declarations  

In this lesson we look at how CSS precedence effects declarations. Precedence refers to how CSS determines styling when two or more rules target the same element.

Since CSS stands for cascading style sheets, this means that it reads from top to bottom. The cascading effect allows you to write very simple CSS rules that apply to the most elements. Specific rules will take precedence over more general rules declared at the top.

It also includes issues of ancestry in styling. If there is a parent element that has a cascading style, all the child elements will inherit that style. If the child element is targeted with different styling, then the more specific styling will appear.

## Including CSS  

There are four ways to apply CSS to an HTML element:

* inline styles

* embedded styles

* external style sheets

* imported styles

### Inline styles  

CSS can be added to any HTML element by including the style attribute after the tag.

```html
<div class="home page" style="background-color: white;">
</div>
```

The above is an example of inline styling. Here we are styling the div element to have a background color of white.

### Embedded styles  

Embedded styling refers to using the style tag at the head of the HTML doc.

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    body {
      font-family: arial;
      background-color: green;
    }
    div {
      background-color: white;
    }
  </style>
</head>
<body>
  <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
  tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
  quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
  consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
  cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
  proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
</body>
</html>
```

In this case, the div still has a background color of white as specified within the style tags.

### External style sheets  

External sheets reference a .css file where all styles are declared. The external file is the most common way to add CSS styling to your doc.

These files are stored on the server and linked to from the HTML file.

```html
<link rel="stylesheet" type="text/css" href="style.css" media="screen"/>
```

### Imported styles  

The import rule allows you to attach new CSS from within the external CSS file. This is common when using libraries to contribute to the styling. It is also useful when you want to have many CSS files and want to have a main file that links to the HTML doc.

```html
@import newstyles.css;
```

## CSS Rules for Precedence  

Some basic rules govern how styles override one another. Often, when styling a page, you'll apply styles to elements via several tags. For instance, what if two different style blocks apply different colors to the same element? Does the first or the second style win? What if an external CSS file styles an element and then an inline style shows something different? Which one takes precedence?

The following list of rules govern the order of precedence for CSS styles:

* CSS rules that appear later in the code will override rules that appear earlier

* Style tags embedded in the HTML doc will override styles from an external file

* Inline styles will override embedded CSS

* A more specific selector takes precedence over a less specific one

## Closing  

In this lesson we looked at the precedence of CSS. There are four ways to apply styles to HTML elements: inline, embedded, external file, and import.

When using these methods of styling it is important to remember precedence. Although it is most common to use an external file for styling, you may also use other methods at the same time. Embedded CSS will override styles for the same element in the external file. Inline styles will override the embedded CSS.

Always a more specific selector will override a less specific one.

To further help you understand, read [this article](https://css-tricks.com/specifics-on-css-specificity/) by Chris Coyier to see some graphics on how precedence works.

## Additional Resources  

* [Four methods of adding CSS to HTML](http://matthewjamestaylor.com/blog/adding-css-to-html-with-link-embed-inline-and-import)

---

# CSS Style Inheritance  

Understanding style inheritance is important when styling with CSS. CSS stands for Cascading Style Sheets, where the intention is to allow styles to flow down from parent to child element. This points to a cascading effect, where its possible to style several elements at once. The main way this happens is through inheritance.

Inheritance in CSS allows you to force values to child elements from their parent. By understanding how this occurs, you can write simple and effective style sheets. Below we will go over some general rules for styling.

## Inheritance from a parent element  

The child element is the HTML element that sits inside of another, which would be its parent. An example of this is the `<p>` tag and `<body>`. Since the paragraph will sit inside of the body, the paragraph can inherit styling from body.

For example, setting the body to a font-family of sans-serif will force paragraphs to display in sans-serif. This removes the need to set the font-family on each paragraph. Instead, each paragraph will inherit from body.

Of course, not all properties are inherited by child elements. Things such as `background-color` and border properties are not passed on to child elements. The rules regarding inheritance work mostly through common sense. If in doubt, [CSS Reference](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference) contains links to many CSS properties. Each property page will state whether or not that property is inherited.

```html
<!-- index.html -->
<!DOCTYPE HTML>
<html>
  <head>
    <link rel="stylesheet" type="text/css" href="style.css">
  </head>
  <body>
    <p>I am the child! 'body' is my parent!</p>
  </body>
</html>
```

```
/* style.css */
body {
  color: blue;
  font-size: 24px;
}
```

## CSS sources  

There are several ways to add styling to your HTML doc. You can use internal styling, such as adding css to a `<style>` tag or inline styling on the element. You can also pull CSS in from external style sheets.

The source of CSS effects its influence in styling. Inline styling takes precedence over others. Therefore, styling applied inline with the element will override other styles. Styles in the `style` tag take precedence after inline styles. Then styles from external style sheets are applied.

An HTML page can use more than on style sheet, which can provide a way to organize your CSS. Styling from an external style sheet also allows for consistency among several pages. But, keep in mind that styles applied internally will take precedence over external styles.

```html
<!-- index.html -->
<!DOCTYPE HTML>
<html>
  <head>
    <link ref="stylesheet" type="text/css" href="style.css">
    <style></style>
  </head>
  <body>
    <h1 style="color: red;">I am a h1 element</h1>
    <p>This paragraph and it is styled by the body because of inheritance</p>
    <h2>This h2 element is styled from the css</h2>
  </body>


</html>
```

```css
/* style.css */
body {
  font-family: helvetica;
  color: black;
}
h2 {
  color: blue;
}
```

## Last Rule  

CSS is interpreted from top to bottom. The further down the page a style is applied, the higher its priority. This means that if there are two selectors that are identical, the one that appears last will take precedence. Styles at the bottom of your CSS doc will override those that appeared before it.

```html
<!-- index.html -->
<!DOCTYPE HTML>
<html>
  <head>
    <link ref="stylesheet" type="text/css" href="style.css">
  </head>
  <body>
    <h1>The last Rule</h1>
    <h2>What are the rules?</h2>
    <p>The paragraphs has been styled to be red</p><br>
    <p>but these are blue</p><br>
    <p>because the last paragraph styling was blue.</p>
  </body>
</html>
```

```css
/* style.css */
body {
  color: black;
}
h1 {
  font-family: helvetica;
  font-size: 24px;
}
p {
  color: red;
  font-family: sans-serif;
}
h2 {
  color: red;
}
p {
  color: blue;
}
``` 
 
# Specificity  

Specificity refers to how specific a selector is. If one selector is more specific than the others, the more specific rule takes precedence. Element selectors, like `<p>` and `<div>` have low specificity. This means that these selectors may not point to a specific part of the HTML doc since they can be repeated.

Next up are class selectors, which are more specific than element selectors. There can be several `<div>` elements, for instance, that belong to the same class.

Even more specific are ID selectors. ID are unique to the page, so an element with an ID cannot be repeated on that page.

The most specific of all is anything labeled `!important`.

```html
<!-- index.html -->
<!DOCTYPE HTML>
<html>
  <head>
    <link ref="stylesheet" type="text/css" href="style.css">
  </head>
  <body>
    <h1>Specificity</h1>
    <p>The default setting on this page has black letters and a white background.<br>
    The different colors show styling by specificity.</p>
    <div class="container">
      <p>The words written here are inside of a div with class container</p>
    </div>
    <p> These words are not </p>
    <div id="high">
      <p> These words are inside a div with id of "high" </p>
    </div>
  </body>
</html>
```

```css
/* style.css */
body {
  color: black;
  font-family: sans-serif;
}
.container {
  background-color: black;
  color: white;
}
#high {
  background-color: grey;
  color: pink;
}
```

## !Important  

This value is highly effective and therefore very dangerous. Always use caution before using `!important`. Adding `!important` after any CSS property value will ensure that this styling rule will rise to the top. Any styling with this label is considered higher than any other styling rules. Because `!important` is so powerful, it is advised to only be used when necessary.

## Closing  

In this lesson, we looked at CSS style inheritance. Inheritance is affected by several factors including its parent element's styling, specificity, the last rule, CSS sources and `!important`.

CSS allows for styling rules to cascade to child elements. Understanding these rules allows for effective and simplistic styling. Remember that these styling rules apply mainly to properties. Some properties will override other properties because of CSS styling rules. This does not apply to the rules themselves. When several rules match the same element, they are all applied to that element.

## References  

* [MDN Cascade and Inheritance](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS/Cascade_and_inheritance)

* [CSS Reference](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference)

---

# How selector specificity affects styling of HTML elements  

There are many different types of CSS selectors. These selectors allow targeting of HTML elements for styling. If two selectors apply to the same element, the one with higher specificity wins. When selectors have an equal specificity value, the latest rule is the one that counts.

Every selector has a place in the specificity hierarchy. Specificity is the way that browsers determine which CSS values will represent an element. It is based on matching rules composed of different CSS selectors.

Selectors will have different weights that affect how CSS rules apply. The embedded style sheet has a greater specificity than other rules. ID selectors have a higher specificity than attribute selectors. A class selector beats any number of element selectors. Understanding how these rules apply is crucial to rendering the CSS correctly.

## CSS selectors  

Some of the common selectors are 1 :

1. Universal Selector

  * Applies to all elements in the document
  
  * `*`, `{ }` Target all elements on the same page
  
2. Type Selector

  * Matches element names
  
  * `h1`, `h2` Targets the `<h1>` and `<h2>` elements
  
3. Class Selector

  * Matches an element whose class attribute has a value that matches the one specified after the `.`.

  * `.home { }` Targets any element whose class attribute has a value of home.
  
4. ID Selector

  * Matches an element whose ID attribute has a value that matches the one specified after the `#`.

  * `#jumbo { }` Targets the elements whose ID attribute has a value of jumbo.

5. Child Selector

  * Matches an element that is a direct child of another

  * `li > a { }` Targets any `<a>` elements that are children of an `<li>` element (but not any other `<a>` elements on the page).

6. Descendant Selector

  * Matches an element that is a descendant of another specified element (not only a direct child).

  * `p a { }` Targets any `<a>` elements that sit inside a `<p>` element, even if there are other elements nested between them.

7. Adjacent Sibling Selector

  * Matches an element that is the next sibling of another

  * `h1 + p { }` Targets the first `<p>` element after any `<h1>` element ( but not any other `<p>` elements).

8. General Sibling Selector

  * Matches an element that is a sibling of another. Although the element does not have to be the directly preceding element.

  * `h1~p { }` If you had two `<p>` elements that are siblings of an `<h1>` element, this rule would apply to both.

## Specificity Order  

* Inherited styles

* Element selector

* Class selector

* ID selector

* Inline styles

* `!important`

There is an order associated with selectors. The order below runs from least relevant to most relevant.

### Inherited styles  

CSS properties inherit styles from their parent elements. This means that the styling of a parent element will be passed on to its child elements. Not every property is inherited, but it can be forced by using the inherit value.

```css
p {
  color: pink;
}
p a:link {
  color: inherit;
}
/* usually links would default to blue but the inherit declaration will force the links to be pink, and inherit the color from the parent element */
```

### Element selectors  

The element selector is styling all elements that have the element name. Targeting the element will take precedence over inherited styles.

```css
p {
  font-family: sans-serif;
}
```

### Class selectors  

Class selectors target elements that are associated with the named class attribute. This selector will pass styles to all the elements within that class attribute.

```html
<!-- HTML -->
<span class="home">This is filler text for a span.</span>
```

```css
/* CSS */
span.home {
  background-color: purple;
}
```

### ID selector  

ID selectors are unique to the HTML doc. Unlike class selectors, there will only be one ID selector with that name. As a result, ID selectors will take precedence over class selectors.

```html
<!-- HTML -->
<div id="blog">Stuff for a blog goes here.</div>
```

```css
/* CSS */

#blog {
  margin: 0;
  background-image: picture.jpg;
}
```

### Inline Styles  

Inline styling applies directly to the HTML doc. Written inline with the HTML elements, these styles will take precedence over those from an external file.

```html
 <h3 style="color:red;margin-left:10px;">This is a sidebar</h3>
```

### !Important  

using the !important attribute will force this style to take precedence over all.

```html
<!-- index.html -->
<!-- HTML -->
<!DOCTYPE html>
<html>
<head>
  <title>CSS Important</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <p class="green">Things that will end up RED</p>
</body>
</html>
```

```
/* style.css */
/* CSS */
p {
  color: red !important;
}
.green {
  color: green;
}
```

## Closing  

In this lesson we covered CSS selectors. CSS selectors refer to HTML elements for styling. There are several types of selectors and there is a precedence for how styling is applied.

## Additional Resources  

[CSS Specificity and Inheritance](https://www.smashingmagazine.com/2010/04/css-specificity-and-inheritance/)

Lesson Footnotes

* 1: [HTML and CSS Design and Build Websites, Jon Duckett, John Wiley & Sons, 2011, 978-1-118-00818-8,p.238]
