When you are working on a prototype it's faster to reload the nodewebkit window
automatically on file changes.

To do this, you can add this script tag to the end of your main file:

```
<script>
  var path = './';
  var fs = require('fs');
  
  fs.watch(path, [], function() {
    if (location)
      location.reload(false);
  });
</script>
```