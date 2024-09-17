Cascading style sheets - used to describe the presentation of HTML document

### Importing CSS
to add styles to the document could be used of the following methods:

```html
< !-- import external file with styles -- >
<link rel="stylesheet" href="styles.css" />

< !-- inline styles to specific element -- >
<div style="color: red;">text</div>

< !-- add style directly to the document -- >
<style>
	.class {
		color: red;
	}
</style>
```

### Structure
css rule has 3 parts:
- selector - define the elements to which the rule applies
- property - one of the style property
- value - one of the possible value for the given property
```css
[select] {
	[property]: [value];
}
```

### Universal selector
`*` - selects all the elements on the page
```html
<h1>Hello World</h1>
<div>This is my <span>Article</span></div>

* {
	color: blue;
}
```

### Type selector
`tagName` - selects all the elements with the given node type
``` html
<span>one</span>
<span>two</span>
<span>three</span>

span {
	color: blue;
}
```


### Class selector
`.className` - selects all the elements with the given class 
Page could contain any number of elements with the same class name
```html
<p class="text">The first block of text</p>
<p class="text">The second block of text</p>

.text {
	margin-top: 10px;
}
```

### ID Selector
`#id` - selects the element with the given id, page should contain only one element for each id
```html
<div id="error">The password is incorred</div>

#error {
	color: red;
}
```

### Attribute selector
`selector[attr=value]` - selects all the elements with the given attribute pair - name/value

```html
<input type="radio" name="vote" value="one" />
<input type="radio" name="vote" value="two" />

input[type="radio"] {
	margin: 0 10px;
}
```

### Combinators
combinator establish a relationship between selectors
```html
<div class="A">
	<span class="B">one</span>
	<span class="C">two</span>
	<span class="B">three</span>
	<span class="B">four</span>
	<div>
		<span class="B">five</span>
	</div>
</div>
<span class="B">six</span>
```
```css
/* Descendant: all .B elements inside .A */
/* one, three, four, five */
.A .B { color: red; }

/* Child: all .B which are direct childs of .A */
/* one, three, four */
.A > .B { color: red; }

/* Adjcaent sibling: .B that follows immediately after .C */
/* three */
.C + .B { color: red; }

/* General subling: all subling .B that follows after .C */
/* three, four*/
.C ~ .B { color: red; }
```

### Pseudo-classes
`selector:pseudo-class` - selects the elements with a special state

```css
/* the button which is the first/last element in its container */
button:first-child { background-color: blue; }
button:last-child { background-color: blue; }

/* the button over which the user's pointer is hovering */
button:hover { background-color: blue; }

/* the input which received foucs */
input:focus { color: red; }

/* disabled input */
input:disabled { background-color: #ccc; }

/* Links that a user has already visisted */
a:visited { color: red; }
```

### Pseudo-elements
`selector::pseudo-element` - used to create cosmetic content for the element or allows to style a specific part of the element

```css
/* 
 * only the first letter in the .text elements
 * will have a red color
*/
.text::first-letter {
	color: red;
}

/*
 * after the .text elements will have "*"
 * with the given styles
*/
.text::after {
	content: '*';
	color: red;
}
```

### Cascade
In css rules applied to the element in the order which they are written in the document, means the value of the property for all other rules which are target this element
```css
.title {
	/* will be overidden by the next rule */
	font-size: 36px;
}

.title {
	/* will be used for .title */
	font-size: 40px;
}
```

### Inheritance
many properties are inheriting their values from the parents of the element, so you don't need to write a rule for every element and in opposite need to be careful and reset the styles from the parents if they are not expected for this particular element

```html
<article>
	My first <span>article</span>
</article>
```
```css
article {
	color: #444;
}

span {
	/**
	* will also have "color: #444" from its parent
	* even it's not defined here 
	*/
}
```

### Specificity
if an element has multiple rules with the same property, to decide what value should be used, beside cascade browser will look on the specificity of each selector and choose the highest one

specificity calculated by the number of each select type:
X-0-0-0: inline styles have more specificity than any selectors
0-N-0-0: number of id selectors
0-0-N-0: number of class selectors
0-0-0-N: number of type selectors
`<div id="id" class="class1 class2">text</div>`
```css
/* specificity: 0121 */
div#id.class1.class2 {
	/* will be used as has more specificity*/
	color: red;
}
/* specifity: 0110 */
#id.class1 {
	color: blue;
}
```

### !important
overrides any other declaration of the property applied to the element (even inline stypes)
```html
<div id="id" class="class1 class2" style="color: red;">text</div>
```
```css
.class1 {
	/* will override any other declarations */
	color: green!important;
}

div#id.class1.class2 {
	color: blue;
}
```


## Formatting

### Text
most common properties:
`color` - change the colour of the text
`font-size` - change the size of the text
`font-family` - specify the list of fonts that will be used to render the text
`font-weight` - sets the boldness of the text
`font-style` - sets whether the text renders italic/oblique or normal
`text-decoration` - adds decoratives lines on the text


## Layouts

Links:
https://www.w3.org/TR/CSS2/ - specification
https://caniuse.com - checks the browsers support of the css properties 
https://developer.mozilla.org/en-US/docs/Web/CSS/Reference - documentation from Mozilla corporation
https://csstriggers.com - checks what property triggers while rendering 
https://css-tricks.com - the portal with bunch of helpful tricks and articles
https://www.w3schools.com/css/css_display_visibility.asp - css layouts

### Box model
Each element represented as a rectangular box for the rendering
![[Pasted image 20240910124101.png]]
```html
<div class="box"><div class="content"></div></div>
<div class="box"><div class="content"></div></div>
```
```css
.box {
	width: 50px;
	height: 50px;
	padding: 10px;
	margin: 10px;
	border: 5px solid blue;
}
.content {
	height: 100%;
	background-color: red;
}
```

**Block elements** -  start with a new line and stretch to the full width of the container, could contain block and inline elements, some properties could be applied only to block elements (such as width, height, margin, padding...)

**Inline elements**  -  have the size of the content, could contain only text and other inline elements, don't break the flow
inline-block - creates a block element (could use all properties from the block element) which behaves in the flow as inline

**Display: none** - removes the element from the blow

**Visibility** - changes visibility of the element, but keeps it in the flow

**Float** - removes an element from the normal flow and puts it to the left/right of the container and allows to other inline elements wrap around it

**Static position** - positions an element based on the normal flow. Offset properties have no effect on it. Default value of the position property

**Relative position** positioned an element based on the normal flow, offset properties will apply on it based on its default position

**Absolute position** - removes an element from the normal flow, it will be positioned by the offset properties based on the closest parent

**Fixed position** - removes an element from the normal flow, offset properties will apply on it based on the viewport

**Overflow** - sets how an element will show its content when it overflows the edges

visible -> content will be rendered outside the edges (default value)
hidden -> hide the content outside the edges. Doesn't allow to scroll
auto -> hide the content outside the edges, but allows scrolling to the hidden content
scroll -> hide the content outside the edges, alwayss show scroll bars, even when content fits the element


## Flexbox
defines layout where children could be positioned in any direction and change their size based on the available space
```html
<div class="flex">
	<div class="child"></div>
	<div class="child"></div>
	<div class="child"></div>
</div>
```
```css
.flex {
	display: flex;
	justify-content: space-between;
	width: 200px;
	height: 50px;
	border: 5px solid blue;
}
.child {
	width: 30px;
	border: 5px solid green;
}
```
![[Pasted image 20240910132231.png|200]]
**Flex: flex-direction** - defines the direction in which flex items will be placed
![[Pasted image 20240910133931.png]]
**Flex: align-items** - defines how items will be positioned inside the flex element
![[Pasted image 20240910133955.png]]
**Flex: justify-content** - defines how space between items will be processed
![[Pasted image 20240910134026.png]]

## Mobile CSS

https://www.w3schools.com/css/css_rwd_mediaqueries.asp  - media queries

One of the ways to provide good experience to mobile users is to build separate version of the website which will be optimised for mobile devices.
Facebook does this with m.facebook.com

Pros:
-  well optimised for mobile
-  possible to build a different UX flow for mobile and desktop
-  Easier to debug
Cons:
-  Have to build 2 websites (time, make sure that changes were applied on both versions)
-  False detections
-  Handle SEO problem (how would the search engine know which one to pop up first in the browser)

### Responsive website
Changes how the website looks like based on the size of the screen

Pros:
-  Time
-  always have the same functionality whatever device is used
Cons:
-  Extra code that is used only for one version of the website
-  Harder to provide the best UX in all the cases

### Media queries
allows to create CSS rules which are applied to the document only when a device reaches specific criteria
```css
.article {
	padding: 5px 10px;
}

@media (min-width: 600px) {
	.article {
		padding: 10px 20px;
	}
}
```

Media types:
```css
// All the devices
@media all {...}
// Print mode
@media print {...}
// Screen devices
@media screen {...}
// Speech synthesizers
@media all {...}
```

### Viewport
represents the currently viewed area of the image, in CSS pixels, high resolution screens display multiple physical pixels per CSS pixels

On mobile viewport not always equal to the size of the device, by default. It is wider than the screen and renders zoomed out

```html
// Size viewport size to the actual screen size of the device
<meta name="viewport" content="width=dievice-width">

// controls the zoom of the page
<meta name="viewport" content="initial-scale=1, maximum-scale=2">
```
![[Pasted image 20240910140607.png]]

