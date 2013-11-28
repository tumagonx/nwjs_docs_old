_Following content requires node-webkit >= v0.3.3_

Like normal browsers, node-webkit enables you to load third party NPAPI plugins, and you can even ship them with your app. So apart from using node.js native modules, you can also use browser plugins to add native code for your app.

By default the plugin support is closed, to open it, you need to specify `plugin` to `true` in `webkit` control fields: -

```json
{
  "webkit": {
    "plugin": true
  }
}
```

Then node-webkit will find and load plugins from conventional paths, which should be the same with your other browsers.

To distribute a plugin along with node-webkit, you have two choices. First you can write a installer which automatically installs plugins, like how many softwares done with flash.

The second way is to ship plugins along with your app, if `/app` is the path to your app, you can create a `plugins` folder under it, e.g. `/app/plugins`, and then put your browser plugins in it and node-webkit will load them.

To see whether your plugins are loaded, you can use `navigator.plugins` in devtools's console.