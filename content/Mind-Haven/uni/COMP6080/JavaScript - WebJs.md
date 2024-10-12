JS two main uses:
	used to manipulate the DOM in a web browser
	used to write scripts with NodeJS

Code can be included inline or in an external link 

```html
<script type="text/javascript">
	const a = 1 + 2;
	console.log(a);
</script>
```
```html
<script type="text/javascript" src="mywork.js"></script>
```
```javascript
const a = 1 + 2;
console.log(a);
```

Linking javaScript externally:
	improvements performance when browser cache is utilised
	reduces the time it takes for initial html document to be received (smaller file)
inline:
	avoids network requests (potentially costly) for script if file is not yet loaded

### Where the code is included
```html
<html>
	<head>
		<!-- CAN INCLUDE IN HEADER -->
	</head>
		<!-- CAN INCLUDE IN TOP OF THE BODY -->
		<!-- MOST OF YOUR PAGE -->
		<!-- CAN INCLUDE IN BOTTOM OF THE BODY -->
</html>
```
Include in `<head>` or at the top of `<body>` if you need js to run prior to DOM elements doing initial render
Include at end of `<body>` if you *do not need* js to run prior to DOM elements doing initial render


## The DOM

DOM (Document Object Model) is an interface that allows js to interact with HTML through the browser

### Structure
```html
<!DOCTYPE HTML>
<html>
	<head>
		<title>The DOM</title>
	</head>
	<body>
		DOM is a <b>tree!</b>
	</body>
</html>
```
![[Pasted image 20241012175109.png]]

### DOM data types
*Document* - type of the document object, represents root of entire DOM
*Element* - node in the DOM tree, object of this type implements an interface that allows interacting with the document
*NodeList* - array of elements

### Reading the DOM
```javascript
// returns an html element with the given id
document.getElementById(id);

// returns a DOM HTMLCollection of all matches
document.getElementsByTagName(name);
document.getElementsByClassName(classname);

// returns the first element that matches the selector
document.querySelector(query);
```

[DOM Reference](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)

## Events
Event is a signal that a 'thing' has happened to a DOM event, this 'thing' can be the element *loading*, *click*, or a *key press*

```javascript
document.addEventListener('mousemove', (event) => {
	console.log(event.clientX);
	console.log(event.clientY);
});
```

### The event loop
js uses a run-to-completion mode, meaning it will not handle a new event until current event has completed.

## Forms
```html
<form name="user_form">
	First name:<br>
	<input type="text" name="firstname">
	Last name:<br>
	<input type="text" name="lastname">
</form>
```

### Reading forms in js
```javascript
document.forms.test // form with name="test"
document.forms["test"] // also form with name="test"
document.forms[0] // first form in the document
```

```html
<form>
	<input type="text" name="fname">
	<input type="radio" name="age" value="10">
	<input type="radio" name="age" value="20">
</form>
```
```javascript
const form = document.forms[0];

// element with the name "fname"
forms.element.fname;
// shorter notation
form.fname;

// since there are multiple elements with the name "age", this returns a collection
const ages = forms.elements.age;
```

### Backreferences
each form element stores a backreference to the form it came from
```html
<form>
	<input type="text" name="login">
</form>
```
```javascript
const form = document.forms[0];
const login = form.login;

console.log(login.form)
```


## Local storage
State is React and js is transient - meaning we will loose all data input after a page refresh, to retain the data, we can use a database, or stored client-side via *local storage*

`window.localStorage` is an API that allows us to read and write to a storage object in the document

**Use local storage**:
	when there is no server
	for data which is not crucial if its lost
**Don't use local storage:**
	 when security of data is important
	 for large amounts of data
	 for complex data
	 data needed on multiple devices

### Local storage API
```javascript
// add a data item given the key and value
localStorage.setItem(key, value);

// Retrieves an item from localstorage given a key
const value = localStorage.getItem(key);

// Remove an item with a given key from localstorage
localStorage.removeItem(key);

// removes all items from local storage
localStorage.clear();
```

