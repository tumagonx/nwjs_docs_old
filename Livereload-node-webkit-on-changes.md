## Basic solution

When you are working on a prototype it's faster to reload the nodewebkit window
automatically on file changes.

To do this, you can add this script tag to the end of your main file:

```html
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
alternative, first you need to install "gaze", "chokidar" or "gulp", and
then change the script tag content:

### gaze

`npm install gaze`

```html
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

```html
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

```html
<script>
  var gulp = require('gulp');
  gulp.task('reload', function () {
    if (location) location.reload();
  });

  gulp.watch('**/*', ['reload']);
</script>
```
Or if you don't want to reload the entire app when editing styles, you can attach a style task
in gulp only to css files.

```html
<script>
  var gulp = require('gulp');
  
  gulp.task('html', function () {
    if (location) location.reload();
  });

  gulp.task('css', function () {
    var styles = document.querySelectorAll('link[rel=stylesheet]');

    for (var i = 0; i < styles.length; i++) {
      // reload styles
      var restyled = styles[i].getAttribute('href') + '?v='+Math.random(0,10000);
      styles[i].setAttribute('href', restyled);
    };
  });

  gulp.watch(['**/*.css'], ['css']);
  gulp.watch(['**/*.html'], ['html']);
</script>
```