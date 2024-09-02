HTML - Hypertext Markup Language. Provides structure for web pages. Not used for style and dynamic states. 

Consists of:
- tag name
- (optional) series of attribute/value pairs
- (optional) inner html

```html
<tag1 attr1="value1" attr2="value2">
	<tag2>Text</tag2>
	<tag3>Other</tag3>
</tag1>
```


What is web browser - tool that takes in HTML, CSS, JavaScript and more, and renders dynamic web pages.

`<!DOCTYPE html>` is required by the specification on modern websites.
```html
<!doctype html>
<html>
	<head>
		<title>Title</title>
	</head>
	<body>
		Hi
		<! -- More meta -- >
	</body>
</html>
```

`<head></head>` - contains meta information of the page, does not render in the browser. Used for:
- give meta-information about the page
- load css stylesheets
- Load in javascript code
```html
<head>
	<title>My page</title>
	<meta charset="utf-8" />
	<link rel="icon" href="favicon.ico" />
	<link rel="stylesheet" type="text/css" href="styles.css" />
	<script type="text/javascript" src="script.js"></script>
</head>
```

`<body></body>` - everything inside `body` renders on the webpage. Only starts rendering after all meta information in `head` has been processed by the web browser.

Layout tags - helps separate page into separate structures. Many of these tags have inherent properties, and are just semantically meaningful ways of distinguishing or identifying particular key parts of the webpage

```html
<div></div> <! -- a generic 'box grouping' element -- >
<span></span> <! -- a generic 'grouping' element -- >
<p></p> <! -- a paragraph -- >
<h1></h1> <! -- 1st biggest header -- >
<h6></h6> <! -- 6th biggest header -- >
<ul><li></li><li></li></ul> <! -- unordered list of items -- >
<ol><li></li><li></li></ol> <! -- ordered list of items -- >
<table></table> <! -- a table of information -- >
<b></b> <! -- bold text (formatting tag) -- >
<i></i> <! -- italic (formatting tag) -- >
<u></u> <! -- underliend text (formatting tag) -- >
<! -- and more -- >
```

Links: `<a></a>`
anchor tag is used to linking to another resource from the current page, takes in URL that may be relative or absolute
Other properties include - `title` which is the hover-over tooltip text and `target="_blank"` - open link in a new tab
```html
<a href="/settings" title="Open settings page">Settings</a>
<a href="https://example.com" target="_blank">example</a>
<! -- below a rare use case -- >
<a href="/image.png" download><img src="download-icon.png" /> </a>
```

Images: `<img />`
known as 'void' tag, since it doesn't need to contain any inner HTML.
Can specify image height, width, and source of the image
Can specify the 'alternate' text in case the image doesn't load
`html| <img src="cat.jpg" alt="Image of the cat" width="300" height="600" />`

Forms: `<form></form>`
provide the structure to collect information from a user and then submit it.
Key parts: inputs & labels, textarea, select, button, submit

`<input />` - input from user
Properties:
- `type` - type of the field (text, radio)
- `name` - name of the attribute during submission
- `value` - the default value to the field that will be sent when submitting
- `placeholder` - background text to hint at what value to input
- `disabled` - boolean as to whether the field is disabled
```html
<input type="text" placeholder="Ram" />

<input type="radio" id="yes" name="answer" value="yes" checked/>
<input type="radio" id="no" name="answer" value="no" />

<input type="checkbox" id="no" name="answer" value="no" checked/>
<input type="checkbox" id="yes" name="answer" value="yes" />

<input type="hidden" name="messageId" value="message123" />
```

`<textarea></textarea>` extends response text inputs (from words/sentence to paragraphs)
```html
<textarea row="5" cols="40" placeholder="write here">
	Default value
</textarea>
```

`<select></select>` - dropdown box that contains options inside of it
```html
<select name="animal">
	<option value="" selected>Select your favourite Chocolate </option>
	<option value="Mars">Mars</option>
	<option value="Snickers">Snickers</option>
</select>
```

`<label></label>` - labels group text to an input so that when text is clicked, field is selected or focused
```html
<label for="dog">I am a dog</label>
<input type="checkbox" value="dog" id="dog" />
```

`<button></button>` an input type 'submit' is also known as button that will automatically submit the form
```html
<button>
	This is a button
</button>

<input type="submit" value="This is a submit button" />
```

Other html tags (not much used)
```html
<! -- iframe allow us to include a 'view' to another webpage in our own -- >
<iframe src="https://example.com" width="400" height="400"></iframe>

<! -- html5 - render playing audo and video -- >
<audio src="music.mp3" controls>
	Browser does not support audio <! -- alternate text if audio cannot load -- >
</audio>

<video src="movie.mp4" type="video/mp4" controls>
	Browser does not support video
</video>
```

## Image types

Images are files that are used to render a collection of pixels on a screen that provide a visual

Two key categories:
**Raster Images** - display an image where every pixel has a defined colour (e.g. RGBA) and position
when enlarged - original image is just stretched, leading to lower quality.
- GIF, BMP, JPG, PNG

**Vector Images** - store a series of geometric instructions (lines, shapes, colours) that are rendered on-the-fly by the browser. typically used for icons and animations.
- SVG


|                | BMP      | GIF              | JPG                    | PNG                      |
| -------------- | -------- | ---------------- | ---------------------- | ------------------------ |
| General format | Variable | 8 bit            | 24 bit                 | 16-24 bit                |
| Compression    | No       | Yes, lossless    | Yes, loosy             | yes, looseless           |
| Uses           | No       | Image animations | High resolution photos | most non-photo use cases |

Base64 Encoding

instead of loading an image as a resource via HTTP, can instead encode it directly into the page of API response

reduces number of HTTP connections required, but does increase the overall amount of data needing in the process, and will naturally slow down the request being made that contains that information
used for small images.

### SVG

scalable vector graphics
![[Pasted image 20240829211613.png]]

```html
<! -- line -- >
<svg viewBox="0 0 100 100" width="100" height="100">
	<line x1="80" y1="10" x2="20" y2="70" stroke="yellow" />
</svg>
-- stroke for line colour
-- stroke-width for line thickness
-- stroke-linecap to specify whether the endpoint is round, square etc.

<! -- circle -- >
<svg viewBox="0 0 100 100" width="100" height="100">
	<circle cx="50" cy="50" r="50"/>
</svg>
-- fill for filled-in colour
-- stroke for outline colour
-- stroke-width for line thickness

<! -- ellipse -- >
<svg viewBox="0 0 100 100" width="100" height="100">
	<ellipse cx="50" cy="50" rx="50" ry="25" />
</svg>
-- fill for filled-in colour
-- stroke for outline colour
-- stroke-wdith for line thickness

<! -- rect -- >
<svg viewBox="0 0 100 100" width="100" height="100">
	<rect x="0" y="0" width="50" height="50" rx="5" ry="5"/>
</svg>
-- fill for filled-in colour
-- stroke for outline colour
-- stroke-wdith for line thickness

<! -- SVG path -- >
<svg viewBox="0 0 100 100" width="100" height="100">
	<path d="M0 0 L100 100 L0 100 Z" />
</svg>
-- fille for filled-in colour
-- stroke for outline colour
-- stroke-width for line thickness
```

[SVG path reference](https://www.w3schools.com/graphics/svg_path.asp)
![[Pasted image 20240829212235.png]]


![[Pasted image 20240829212312.png]]


