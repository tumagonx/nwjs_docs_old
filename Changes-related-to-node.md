_This document is still under construction_

## console
Since node-webkit is not a console app, console.log is now redirected to Webkit's console. So you can see it in devtools.

## process
A couple of new fields is added to the global `process` object:
* `process.versions['node-webkit']` is set with node-webkit's version.
* `process.mainModule` is set for the start page (such as `index.html`) as specified in the manifest's [`main`](Manifest-format#main) field. However, when the [`node-main`](Manifest-format#node-main) field is also specified in the manifest, `process.mainModule` points to the file specified in the `node-main` field.

## global
The following names are inserted to the global object in Node's context:
* `require` - this is the `require()` function within the main module.

## require
Behaviour of relative paths in Node's `require()` method depends on how the parent file is used in the application (where “the parent file” is the file in which the `require()` method is called):
* If the parent file was also required by Node (using `require()`), then the child's relative path is treated as relative to its parent.
* If the parent file is included by WebKit (using any web technology: classic DOM `window.open()`, node-webkit's [`Window.open()`](Window#openurl-options), classic DOM [`XMLHttpRequest`](https://developer.mozilla.org/en/docs/DOM/XMLHttpRequest), jQuery's [`$.getScript()`](http://api.jquery.com/jQuery.getScript/), HTML `<script src="...">` element, etc.), then the child's relative path is treated as relative to the application's root directory.