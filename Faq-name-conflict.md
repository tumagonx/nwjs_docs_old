If your project already has a function called `require()` it will conflict with Node's built-in `require()` which has been integrated into the DOM `window` object. In this case you can either disable Node support with the `nodejs` field in the [manifest](Manifest-format), or use this trick at the beginning of your code:

````html
<script type="text/javascript">
    window.requireNode = window.require;
    window.require = undefined; 
</script>
````