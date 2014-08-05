## Basic solution

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

Sadly, the previous solution doesn't work recursively. If you want a recursive
alternative, first you need to install "gaze" or "chokidar", and
then change the script tag content:

### gaze

`npm install gaze`

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

### chokidar

`npm install chokidar`

```
  <script>
    var chokidar = require('chokidar');
    var watcher  = chokidar.watch('.', {ignored: /[\/\\]\./});
    watcher.on('all', function(event, path) {
      if (window.location)
        window.location.reload();
    });
  </script>
```

### gulp

`npm install gulp`

```
<script>
  var gulp = require('gulp');
  gulp.task('reload', function () {
    if (location) location.reload();
  });

  gulp.watch('**/*', ['reload']);
</script>
```