Translation for Getting Started with nw.js

翻译自Getting Started with nw.js

本章节包涵了一些指导信息，以帮助您开始nw.js编程。假定你有nw.js的二进制文件(这样的文件都可以在 “[下载](https://github.com/nwjs/nw.js#downloads)”READEME的部分，如果你想建立自己的二进制文件请参阅[Building nw.js])
nw.js基于[Chromium](http://www.chromium.org) and [io.js](http://iojs.org/)。它可以让你直接从DOM调用Node.js的代码及模块，使您可以使用web技术来开发应用程序。此外，你可以很轻松的打包web应用到本地应用程序
## 基础
首先我们介绍nw.js，我们先从最简单的程序开始。
**示例 1. Hello World**

![Capture3](https://f.cloud.github.com/assets/2891424/279516/5fba0cca-912b-11e2-983d-c2e8a66c3706.PNG)

创建 `index.html`:

```html
<html>
<head>
<title>Hello World!</title>
</head>
<body>
<h1>Hello World!</h1>
</body>
</html>
```

创建 `package.json`:

```json
{
  "name": "nw-demo",
  "main": "index.html"
}
```

压缩 `index.html` 和 `package.json` 到zip压缩文件，并修改文件名为 `app.nw`:

    app.nw
    |-- package.json
    `-- index.html

下载你所使用的平台的预构建的二进制文件并用它打开 `app.nw` 文件:

```bash
$ ./nw app.nw
```

注意: 在 Windows, 你可以拖拽 `app.nw` 到 `nw.exe` 来打开它。

**示例 2. Native UI API**

![Capture4](https://f.cloud.github.com/assets/2891424/279875/e8572dd0-913d-11e2-8a82-ea021ca07ce6.PNG)

nw.js 有原生 UI 控制 API。 你可以用这些来控制窗口、菜单等等

下面的示例演示如何使用菜单的API。

```html
<html>
<head>
  <title> Menu </title>
</head>
<body>
<script>
// 载入原生UI库
var gui = require('nw.gui');

// 创建空菜单
var menu = new gui.Menu();

// 添加菜单项，label为菜单项的显示名
menu.append(new gui.MenuItem({ label: 'Item A' }));
menu.append(new gui.MenuItem({ label: 'Item B' }));
menu.append(new gui.MenuItem({ type: 'separator' }));
menu.append(new gui.MenuItem({ label: 'Item C' }));

// 移除菜单项
menu.removeAt(1);

// 遍历菜单项
for (var i = 0; i < menu.items.length; ++i) {
  console.log(menu.items[i]);
}

// 添加菜单项并绑定菜单点击后的回调函数
menu.append(new gui.MenuItem({
label: 'Click Me',
click: function() {
  // 创建HTML元素
  var element = document.createElement('div');
  element.appendChild(document.createTextNode('Clicked OK'));
  document.body.appendChild(element);
}
}));


// 弹出上下文菜单
document.body.addEventListener('contextmenu', function(ev) { 
  ev.preventDefault();
  // 在你点击后弹出
  menu.popup(ev.x, ev.y);
  return false;
}, false);

// 获取当前窗口
var win = gui.Window.get();

// 创建一个窗口的菜单栏
var menubar = new gui.Menu({ type: 'menubar' });

// 创建一个菜单项
var sub1 = new gui.Menu();


sub1.append(new gui.MenuItem({
label: 'Test1',
click: function() {
  var element = document.createElement('div');
  element.appendChild(document.createTextNode('Test 1'));
  document.body.appendChild(element);
}
}));

// 添加子菜单
menubar.append(new gui.MenuItem({ label: 'Sub1', submenu: sub1}));

// 设置菜单窗口的菜单
win.menu = menubar;

// 添加一个点击事件到已有菜单
menu.items[0].click = function() { 
    console.log("CLICK"); 
};

</script>  
</body>
</html>
```

**示例 3. Using node.js**

您可以直接在DOM调用的Node.js和模块。因此，它实现了无限的可能性，写的应用程序与nw.js.

```html
<html>
<body>
<script>
// 使用node.js获取系统平台
var os = require('os')
document.write('Our computer is: ', os.platform())
</script>
</body>
</html>
```


## 运行与打包应用

现在，我们可以写简单的nw.js应用程序。下一步是了解如何运行并将其打包。 

**运行应用程序**

多平台运行的两种常见方式

* 从一个文件夹。启动路径指定该文件夹。
* 从.nw文件（重命名.ZIP文件）。启动路径指定文件。

例如:

````bash
nw path_to_app_dir
nw path_to_app.nw
````

## 故障排除

如果有任何问题，请参阅 [[Troubleshooting]] 。

回到 [Wiki](https://github.com/nwjs/nw.js/wiki) 以查看更多