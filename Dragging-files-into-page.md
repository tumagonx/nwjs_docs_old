_Following content requires node-webkit >= v0.2.5_

node-webkit allows directly dragging files into page, developers can use it the same way as HTML5 specifies. The bonus is a `path` field is provided in `File` object so you can use `fs` module to directly operate files dragged into page.

An example is:

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      #holder { border: 10px dashed #ccc; width: 300px; height: 300px; margin: 20px auto;}
      #holder.hover { border: 10px dashed #333; }
    </style>
    <script>

      //Same as $(document).ready();
      function ready(fn) {
        if (document.readyState != 'loading'){
          fn();
        } else {
          document.addEventListener('DOMContentLoaded', fn);
        }
      }

      //When the page has loaded, run this code
      ready(function(){
        // prevent default behavior from changing page on dropped file
        window.ondragover = function(e) { e.preventDefault(); return false };
        // NOTE: ondrop events WILL NOT WORK if you do not "preventDefault" in the ondragover event!!
        window.ondrop = function(e) { e.preventDefault(); return false };

        const holder = document.getElementById('holder');
        holder.ondragover = function () { this.className = 'hover'; return false; };
        holder.ondragleave = function () { this.className = ''; return false; };
        holder.ondrop = function (e) {
          e.preventDefault();

          for (let i = 0; i < e.dataTransfer.files.length; ++i) {
            console.log(e.dataTransfer.files[i].path);
          }
          return false;
        };
      });
  </script>
  </head>
  <body>
    <div id="holder"></div>
  </body>
</html>
```

And the standard HTML5 way of reading files is also supported too:

```javascript
holder.ondrop = function (e) {
  e.preventDefault();

  const file = e.dataTransfer.files[0];
  const reader = new FileReader();

  reader.onload = function (event) {
    console.log(event.target);
  };
  console.log(file);
  reader.readAsDataURL(file);

  return false;
};
```

If you need to handle file or folder:

```javascript
holder.ondrop = function (e) {
  e.preventDefault();

  const item = e.dataTransfer.items[0];
  const entry = item.webkitGetAsEntry();
  if (entry.isFile) {
    const file = item.getAsFile();
    // Do magic with file
  } else if (entry.isDirectory) {
    // Do magic with directory
    const dir = item.getAsFile();
  }

  return false;
};
```

If you're using jQuery to bind your drag / drop events, keep in mind that the event object passed back to your handlers is a jQuery event object, not the original HTML5 event object. Therefore, when you need to??access the `dataTransfer` object, you??have to??use the `e.originalEvent.dataTransfer` field.