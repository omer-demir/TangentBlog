---
title: "Memory Leaks in Javascript"
date: 2017-12-11T11:20:26+01:00
draft: true
slug: "memory-leak-js"
tags: ["javascript"]
categories: ["javascript"]
---


## Unaware usage of global variable

```javascript
function foo(arg) {
    bar = "this is a hidden global variable";
}
```

is equivalent to below; 

```javascript
function foo(arg) {
    window.bar = "this is an explicit global variable";
}

//or

function foo() {
    this.variable = "potential accidental global";
}
```

**In order to eliminate memory leak, add 'use strict'; at the beginning of your JavaScript files. This enables a stricter mode of parsing JavaScript that prevents accidental globals.**
**Other way to mark as variable as can be disposed assign to null when you dont use it**

## Forgotten timer and callbacks

```javascript
var someResource = getData();
setInterval(function() {
    var node = document.getElementById('Node');
    if(node) {
        // Do stuff with node and someResource.
        node.innerHTML = JSON.stringify(someResource));
    }
}, 1000);
```

The object represented by node may be removed in the future, making the whole block inside the interval handler unnecessary. However, the handler, as the interval is still active, cannot be collected (the interval needs to be stopped for that to happen). If the interval handler cannot be collected, its dependencies cannot be collected either. That means that someResource, which presumably stores sizable data, cannot be collected either.

Solution to problem

```javascript
element.addEventListener('click', onClick);
// Do stuff
element.removeEventListener('click', onClick);
element.parentNode.removeChild(element);
```

## DOM references in code

```javascript
var elements = {
    button: document.getElementById('button'),
    image: document.getElementById('image'),
    text: document.getElementById('text')
};

function doStuff() {
    image.src = 'http://some.url/image';
    button.click();
    console.log(text.innerHTML);
    // Much more logic
}

function removeButton() {
    // The button is a direct child of body.
    document.body.removeChild(document.getElementById('button'));

    // At this point, we still have a reference to #button in the global
    // elements dictionary. In other words, the button element is still in
    // memory and cannot be collected by the GC.
}
```

In practice this won't happen: the cell is a child node of that table and children keep references to their parents. In other words, the reference to the table cell from JavaScript code causes the whole table to stay in memory. Consider this carefully when keeping references to DOM elements.

## Closures

```javascript
var theThing = null;
var replaceThing = function () {
  var originalThing = theThing;
  var unused = function () {
    if (originalThing)
      console.log("hi");
  };
  theThing = {
    longStr: new Array(1000000).join('*'),
    someMethod: function () {
      console.log(someMessage);
    }
  };
};
setInterval(replaceThing, 1000);
```

This snippet does one thing: every time replaceThing is called, theThing gets a new object which contains a big array and a new closure (someMethod). At the same time, the variable unused holds a closure that has a reference to originalThing (theThing from the previous call to replaceThing). Already somewhat confusing, huh? The important thing is that once a scope is created for closures that are in the same parent scope, that scope is shared. In this case, the scope created for the closure someMethod is shared by unused. unused has a reference to originalThing. Even though unused is never used, someMethod can be used through theThing. And as someMethod shares the closure scope with unused, even though unused is never used, its reference to originalThing forces it to stay active (prevents its collection). When this snippet is run repeatedly a steady increase in memory usage can be observed. This does not get smaller when the GC runs. In essence, a linked list of closures is created (with its root in the form of the theThing variable), and each of these closures' scopes carries an indirect reference to the big array, resulting in a sizable leak.