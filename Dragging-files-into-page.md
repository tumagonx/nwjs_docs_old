_Following content requires node-webkit >= v0.2.5_

node-webkit allows directly dragging files into page, developers can use it the same way as HTML5 specifies. The bonus is a `path` field is provided in `File` object so you can use `fs` module to directly operate files dragged into page.

An example is:

```html
<html>
<body>
<style>
#holder { border: 10px dashed #ccc; width: 300px; height: 300px; margin: 20px auto;}
#holder.hover { border: 10px dashed #333; }
</style>
<div id="holder"></div>
<script>
var holder = document.getElementById('holder');

holder.ondragover = function () { this.className = 'hover'; return false; };
holder.ondragend = function () { this.className = ''; return false; };
holder.ondrop = function (e) {
  e.preventDefault();

  for (var i = 0; i < e.dataTransfer.files.length; ++i) {
    console.log(e.dataTransfer.files[i].path);
  }
  return false;
};
</script>
</body>
</html>
```

And the standard HTML5 way of reading files is also supported too:

```javascript
holder.ondrop = function (e) {
  e.preventDefault();

  var file = e.dataTransfer.files[0],
      reader = new FileReader();
  reader.onload = function (event) {
    console.log(event.target);
  };
  console.log(file);
  reader.readAsDataURL(file);

  return false;
};
```