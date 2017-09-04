Hello, 

I'm posting this here since the node_modules debug have been deplaced to the background page devtool since 0.13. It has bugged me for long, but I finally have a quick and dirty (those are the best, admit it) to log what's happening in my modules directly from the main dev console. I just added this snipped at the top of my html document:

```html
<script>chrome.extension.getBackgroundPage().console.log = console.debug;</script>
```

What this does, is overwrite the background page logging by the "normal" console.debug call (I used debug so I can easily hide it with filters, but feel free to use console.log instead).

I have no idea if it's totally bad practice, but my life just got easier 👍 