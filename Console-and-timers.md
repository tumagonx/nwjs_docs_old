`node.js` and `Chromium` each has its own implementation of `setTimeout` and `console`, currently we use `Chromium`'s implementation everywhere, even in node's module, reasons:

1. Many web libraries make use of hacks on `window.setTimeout`, while node modules rarely do that.
2. `Chromium`'s `console` is very friendly with devtools and is alse capable of debugging.