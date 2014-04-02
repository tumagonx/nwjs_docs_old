When you are working on a prototype it's faster to reload the nodewebkit window
automatically on file changes.

To do this, you can add this script tag to the end of your main file:

```
<script>
  var path = './';
  var fs = require('fs');
  
  fs.watch(path, function() {
    if (location)
      location.reload();
  });
</script>
```

## Recursive solution

Sadly, the previous solution don't work recursively. If you want a recursive
alternative, first you need to install "gaze" (with "npm install gaze" command), and
then change the script tag content:

```
  <script>
   var Gaze = require('gaze').Gaze;
   var gaze = new Gaze('**/*');

   gaze.on('all', function(event, filepath) {
     if (location)
       location.reload();
   });
  </script>
```