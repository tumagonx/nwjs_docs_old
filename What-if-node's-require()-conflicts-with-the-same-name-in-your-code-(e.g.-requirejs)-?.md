If your project already have a function called `require()` it will conflicts with Node's `require()` integrated into DOM. In this case you can either disable Node support with `nodejs` field in the [manifest][Manifest-format], or use this trick at the beginning of your code:

````html
<script type="text/javascript">
    window.requireNode = window.require;
    window.require = undefined; 
</script>
````