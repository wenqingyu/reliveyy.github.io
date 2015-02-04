---
layout: post
title: CSS position
category: Notes
tags: CSS
---


### Static
The default position is static for any html element if no specified explicitly. A static positioned element is always positioned according to the normal flow of the page. Static positioned elements are not affected by the top, bottom, left and right properties.

### Fixed
An element with a fixed position is positioned relative to the browser window, and will not move even if the window is scrolled.

Fixed positioned elements are removed from the normal flow. And they can overlap other elements.

### Relative
A relative positioned element is positioned relative to its normal position.
The content of relatively positioned elements can be moved and overlap other elements, but the reserved space for the element is still preserved in the normal flow.

Relatively positioned elements are often used as container blocks for absolutely positioned elements.

### Absolute
An absolute position element is positioned relative to the first parent element that has a position other than static. If no such element is found, the containing block is <html>

Absolutely positioned elements are removed from the normal flow. And they can overlap other elements.

When the element is absolutely positioned, you can use `clip` CSS property to clip it. The clip property dose not work if "overflow:visible".

```css
img{
	position: absolute;
	clip: rect(70px, 300px, 220px, 100px);
	/* rect(<top>, <right>, <bottom>, <left>) */
	/* clip: auto|shape|initial|inherit;      */
	/* rect is the only value of shape        */
}
```

```js
object.style.clip = "rect(70px, 300px, 220px, 100px)"
```

{% include explain-css-clip.html %}

### Stack order of an overlapping element
`z-index: an integer;`

If two positioned elements overlap without a z-index specified, the element positioned last in the HTML code will be shown on top.