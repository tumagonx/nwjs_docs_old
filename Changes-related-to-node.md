_This document is still under construction_

## console
Since node-webkit is not a console app, console.log is now redirected to Webkit's console. So you can see it in devtools.

## process
* `process.versions['node-webkit']` is set with node-webkit's version.
* `process.mainModule` is set for start page (`index.html`). When `node-main` is specified in the manifest, `process.mainModule` points to `node-main`.

## global
The following names are inserted to the global object in Node's context:
* `require` - this is the `require()` function within the main module.

## require
"In any files included via webkit (using Window.open, xhr, $.getScript, etc), require is relative to the root of the app. For any files included via node.js (using require), require is relative to the included file." -- from issue#603