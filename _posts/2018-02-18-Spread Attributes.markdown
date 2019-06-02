---
layout:       post
title:        "Spread Attributes"
subtitle:     "ECMAScript 2015 - Spread Attributes"
date:         2018-02-18 12:00:00
author:       "Kuo"
header-img:   "img/post-bg-js-version.jpg"
header-mask:  0.3
catalog:      false
multilingual: false
comments:     true
tags:
    - Javascript
    - ECMAScript 2015
---

... are called `Spread Attributes` which the name represents it allows an expression to be expanded.

```javascript
var parts = ['two', 'three'];
var numbers = ['one', ...parts, 'four', 'five'];  // ["one", "two", "three", "four", "five"]
```

Another example

```javascript
//just assume we have an object like this:
var person= {
    name: 'Alex',
    age: 35 
}
```

```html
<Modal {...person} title='Modal heading' animation={false} />

is equal to

<Modal name={person.name} age={person.age} title='Modal heading' animation={false} />
```