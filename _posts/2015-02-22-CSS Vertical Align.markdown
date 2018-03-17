---
layout:       post
title:        "CSS Vertical Align"
subtitle:     "CSS Vertical Align"
date:         2015-02-22 12:00:00
author:       "Gary"
header-img:   "img/post-bg-android.jpg"
header-mask:  0.3
catalog:      false
multilingual: false
comments:     true
tags:
    - CSS
---
#### Vertical Align
##### Basic Approach
```css
#box {
  height:      90px;
  line-height: 90px;
}
```

It only works for a single line of text though, because we set the line's height to the same height as the containing box element.

--------------------------------------
##### A more versatile approach
And here is another way to align text vertically. This solution will work for a single line and multiple lines of text, but it still requires a fixed height container:
```html
<div>
  <span>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</span>
</div>
```
The CSS just sizes the `<div>`, vertically center aligns the `<span>` by setting the `<div>`'s line-height equal to its height, and makes the `<span>` an inline-block with `vertical-align: middle`. Then it sets the line-height back to normal for the `<span>`, so its contents will flow naturally inside the block.
```css
div {
  width: 250px;
  height: 100px;
  line-height: 100px;
  text-align: center;
}

span {
  display: inline-block;
  vertical-align: middle;
  line-height: normal;      
}
```
--------------------------------------
##### Simulating table display
And here is another option, which may not work on older browsers that don't support `display: table` and `display: table-cell` (basically that means Internet Explorer 7). The HTML is the same as the second example:
```html
<div>
  <span>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</span>
</div>
```
Using CSS we 'simulate' table display behavior, since tables support vertical alignment:
```css
div {
  display: table;
  width: 250px;
  height: 100px;
  text-align: center;
}

span {
  display: table-cell;
  vertical-align: middle;
}
```

--------------------------------------
##### Using absolute positioning
This technique uses an absolutely positioned element setting top, bottom, left and right to 0. 
