If you used to write server side scripts, and have little experience writing mobile HTML apps, then this guide is absolutely for you. And if you think you're an expert on node.js or mobile HTML apps, you should also read this guide carefully, too many 'experts' were restricted to their old experiences and failed to aware new possibilities.

## Express?

If you searched `write desktop apps in node.js` and found node-webkit, the first thing you may do is trying to figure out how to make `Express` or other node web frameworks work, and I've get questions like this for too many times.

The short answer is, **DO NOT** use `Express`.

### Router

People who want `Express` are indeed finding two things: `router` and `template engine`.

As for `router`, it is used for showing different pages for different urls, like `/user/zcbenz` and `/article/901127`. You don't need a `router` in node-webkit, because you already have a better one: the `file:` protocol. For example, if you have an app like this:

```
ap
├── index.html
└── views
    ├── user.html
    └── article.html
```

Then you you can navigate to a page by using `window.location = 'views/user.html#zcbenz'` and handle logics in `views/user.html` file.

However the recommended way is not opening a new window or changing the `window.location`, you should just replace part of your page with ajax, like `$('#main').load('views/user.html#zcbenz');`. In this way users won't have bad experience feeling refreshing the page, and the current javascript context is also kept.

### Template Engine

For template engine, node.js guys have always been comfortable with rendering at server side and then send result to clients. In node-webkit everything is at client side, you don't need to render the template and then feed it to node-webkit, you could just render everything after the page is loaded.

The basic idea is: use the template engine to generate contents on the fly, and then append dynamic contents into the DOM. like this:

```html
<script>
var jade = require('jade');
var gen_files_view = jade.compile([
    '- each file in files',
    '  .file(data-path="#{file.path}")',
    '    .icon',
    '      img(src="icons/#{file.type}.png")',
    '    .name #{file.name}',
].join('\n'));

$(document).ready(function() {
  // read sth to files
  // blabla...
  $('#content').html(gen_files_view({ files: files }));
});
</script>
```

Another example is how I refresh the address bar and folder view in [zcbenz/nw-file-explorer](https://github.com/zcbenz/nw-file-explorer), see [node_modules/folder_view.js](https://github.com/zcbenz/nw-file-explorer/blob/master/node_modules/folder_view.js) and [node_modules/address_bar.js](https://github.com/zcbenz/nw-file-explorer/blob/master/node_modules/address_bar.js).

### Why not the router way?

If you have experiences with both `express` and `appjs`, you are likely to be finding the same way of writing apps in node-webkit, e.g. using `router` to show pages. We don't support it because it's the wrong way.

Firstly, node-webkit is a runtime for HTML and Javascript apps, not for node.js apps, we don't want to require any node.js knowledge to write a complete app.

And node-webkit is also compatible with most other web runtimes, you can easily migrate an app written in tideSDK or other platforms into node-webkit, and vice verso. But if you use the `router` style in `express` and `appjs` for your app, it would be a hard time for you to move to other platforms.

## CoffeeScript, LESS and more

Guys who code in node.js would also want languages that compile into javascript and CSS, the most famous ones are `CoffeeScript` and `LESS`. Usually people would compile the code in server and then send results into clients' browsers, but indeed you can use them directly at browser side, by installing the compiler first and then evaluate the code on the fly.

For `CoffeScript`, you should first tag your coffeescript with the type `text/coffeescript`, and then include [coffee-script.js](http://github.com/jashkenas/coffee-script/raw/master/extras/coffee-script.js) after all coffeescript on the page (this is the compiler that will evaluate and compile all coffeescript in order). Here is an example:

```html
<html>
<head>
<script type=”text/javascript” src=”http://ajax.googleapis.com/ajax/libs/jquery/1.4.4/jquery.min.js”></script>
<script type=”text/coffeescript”>
 $ -> $('#header').css 'background-color', 'green'
</script>
<script type=”text/javascript” src=”http://github.com/jashkenas/coffee-script/raw/master/extras/coffee-script.js”></script>
</head>
<body>
<h1 id=”header” style=”color:white”>CoffeeScript is alive!</h1>
</body>
</html>
```

For `LESS`, it also has an online compiler that translates your `.less` file into `.css` file in browser, to use it, just add following lines in `<head>` tag:

```html
<link rel="stylesheet/less" href="/link/to/your.less" type="text/css" /> 
<script src="http://lesscss.googlecode.com/files/less-1.3.0.min.js"></script>
```

And of course you can always choose to compile the code before packing your app, and only deliver the complied files to end users.

## Page navigation and multiple windows

If you have written server side scripts, you would be familiar with navigating user to different pages when they click a link, and save users' session information in `cookies` or in server.

In node-webkit, you can basically do the same thing by using `window.location`, you can install it in the `onclick` event of a link and node-webkti will navigate to a new page when user clicks it. But by doing this you would lose everything in `window` context, you can save everything in cookies like old web pages, or you can save things in the `global` variable, which resides in Node.js's context and will live through your app.

But a better way is just refreshing part of your pages, not navigating to a new one, you can search for `ajax` to get basic ideas.

The `global` variable is also useful for apps that have multiple windows. Every opened window will share a same `global`, so you can use it to transfer messages across different windows. And the normal HTML way of using the return value of `window.open` is also available, for more you can have a look at [[Play with window]].
