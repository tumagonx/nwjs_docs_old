node-webkit adds Node.js support and enhancement in DOM for trusted code and content. For untrusted code and content, it should remain in a normal `frame` or `iframe`, which is the same as the one in browser. So there are 2 kinds of frames in node-webkit: `Node frame` and `normal frame`.

Which frames are Node frames and which are not?

1. Local file, or remote URL matches the `node-remote` field. (`nodejs` field should not be set to false in this case)
2. Frames opened with `window.open` are normal frames when these flags are set: `new-instance` = `true` and `nodejs` = `false`
3. iframes has the attribute `nwdisable` are normal frames.

What can Node frames do?

1. Node support: access to `require`, `global`, `process`, 'Buffer' and 'root' from Node.
2. Universal access to other frames: this can get around the cross-domain security check defined in DOM.