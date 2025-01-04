---
title: Accessible Develop Practices
tags:
  - moodle-academy
  - tutorial
cssclasses: []
author: Ram
---

## Web accessibility
web-based tools and technologies are designed and developed so that people can use them regardless of ability.

### Conformance to accessibility standards
some of the accessibility standards that Moodle is guided by includes:
	WCAG 2.1
	ATAG 2.0
	ARIA 1.1

### Accessibility tools
checkers:
https://wave.webaim.org/
https://www.deque.com/axe/

HTML validators
https://validator.w3.org/nu/

Colour contrast checkers
https://webaim.org/resources/contrastchecker/
https://colourcontrast.cc/

Screen readers
https://www.freedomscientific.com/products/software/jaws/
https://www.nvaccess.org/download/

### Design considerations
**Blind**
functionality must be available via keyboard
good structure and semantics
custom control must have correct label, role and value
users must receive immediate feedback after all actions

**Low vision**
page must retain functionality even when zoomed (level AA: 400% zoom)
text must pass contrast guidelines against background
visible focus and hover states (links, buttons, controls)
clear visual distinction between content and controls (links must have 3:1 colour contrast ratio against text)

**Colour bind**
information must be understandable without relying on colours
e.g. - calendar - event types are distinguished by icons as well

**Motor disabilities**
keyboard functionality avaialble
visible focus and hover states
warn users before time expires and provide option to extend session
provided large click targets

## Accessibility features in Moodle API

### ARIA
W3C specification that stands for 'Accessible Rich Internet Applications'. Set of roles and attributes that define ways to make web content and web applications more accessible to people with disabilities.

three main features (role, properties, states)
Role define what an element is or does on the page or app
Properties express characteristics or relationship to an object
States define the current conditions or data values associated with the element
```html
<!-- roles !-->
<a role="menuitem" class="nav-link active" href="http://localhost/moodle/" aria-current="true">Home </a>

<!-- properties !-->
<div role="button" aria-describedby="more-info">Get help</div>
<div id="more-info">Our online support. </div>

<!-- States !-->
<a role ="menuitem" class="nav-link active" href="http://localhost/moodle/" aria-current="true">Home</a>
```

note: use `tabindex = 0` to any element that needs a focus that doesn't normally receive keyboard focus.

