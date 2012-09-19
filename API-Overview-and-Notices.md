_Native UI API requires node-webkit >= 0.3.0_

Here is our APIs for creating native UI controls in node-webkit. Generally, to load our Native UI library, you need first to use `require` function to load `nw.gui` module (our module name did not follow node's standard, so you wouldn't encounter module name clashes):

```javascript
var gui = require('nw.gui');
```

Then you create a GUI element the way you create a javascript object:

```javascript
// Standard way of creating elements
var element = new gui.ElementName(option);

// Example of creating a menu
var menu = new gui.Menu({ title: 'Menu Title' });
```

The properties like `title`, `label`, `icon` and `menu` are set/get via directly using object's attributes, like what you did in DOM, such as:

```javascript
menu.title = 'New Title';
console.log('Menu title is', menu.title);
```

And methods like `remove`, `append` and `insert`, are of course, methods of GUI objects:

```javascript
menu.append(new gui.MenuItem({ label: 'Im an item' }));
menu.removeAt(0);
```

Child elements can be got via index accessing like arrays:

```javascript
for (var i = 0; i < menu.length; ++i) {
  console.log('MenuItem', i, menu[i]);
}
```

And please don't directly change elements via reassigning like `menu[2] = new gui.MenuItem(...);`, it's absolutely wrong. To update an element, just change it like `menu[2].title = 'New Title'`, to replace an element, first `remove` it and then do an `insert`.

Another thing is we don't throw exceptions we you're doing something wrong in UI API, **we crash**. So be careful on using it. If you're reusing a deleted element, or passing wrong types, we will crash without warning you.

One thing you may not notice is after deleting objects, you should always assign `null` to a deleted UI object, in case you accidentally reuse it, an example is:

```javascript
var menu = new gui.Menu(...);
// blablabla...
// We are done with it
menu.remove();
menu = null; // This line is very important
```