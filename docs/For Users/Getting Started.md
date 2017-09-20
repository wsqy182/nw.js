# 开始使用NW.js
---

[TOC]

## NW.js 能做什么?

NW.js是基于[Chromium](http://www.chromium.org)和[Node.js](http://nodejs.org/).
它让你可以调用Node.js代码和模块从浏览器,也可以在你的应用程序中使用Web技术,而且你可以很容易地将一个Web应用程序打包到一个本地应用程序中。

## Get NW.js

你可以从官网http://nwjs.io上获取最新的安装文件.或者你也可以编译自己的NW.js程序 [Building NW.js](https://github.com/wsqy182/nw.js/blob/nw25/docs/For%20Developers/Building%20NW.js.md).

!!! tip建议您选择SDK构建风格来开发您的应用程序，这使您可以使用DevTools来调试您的应用程序。
查看 [构建风格](https://github.com/wsqy182/nw.js/blob/nw25/docs/For%20Users/Advanced/Build%20Flavors.md) 以区分不同的构建风格.

## 编写 NW.js 应用程序

### 例子 1 - Hello,World

这是个基本的例子用来显示如何去编写一个NW.js 的应用程序.

**Step 1.** 创建文件 `package.json`:

```json
{
  "name": "helloworld",
  "main": "index.html"
}
```

`package.json` 是你的应用程序的清单文件. 它是用[JSON格式](http://www.json.org/)编写的. main字段表示打开NW.js文件后第一个加载的html页面,比如这个例子中的'index.html'.
name字段是这个NW.js构建的APP的唯一名字.
查看[Manifest格式](https://github.com/wsqy182/nw.js/blob/nw25/docs/References/Manifest%20Format.md) 得知更多细节.

!!! tip 
"使用JS文件作为main字段的值".
你可以设置JS文件作为main字段的值,比如"main.js".
这个js文件当程序启动且没有窗口的时候加载.
通常情况下你可以在打开窗口前在做一些初始化的工作.
例子:

    ```javascript
    // initialize your app
    // and ...
    nw.Window.open('index.html', {}, function(win) {});
    ```

**Step 2.** 创建文件 `index.html`:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Hello World!</title>
  </head>
  <body>
    <h1>Hello World!</h1>
  </body>
</html>
```

这是一个普通的HTML文件,您可以使用最新浏览器支持的任何web技术。

**Step 3.** 运行你的APP

```bash
cd /path/to/your/app
/path/to/nw .
```

`/path/to/nw` 是NW.js的二进制文件. 在windows中, 对应的是 `nw.exe`; 在 Linux中, 他是nw.js `nw`; 在 Mac中, 对应的是 `nwjs.app/Contents/MacOS/nwjs`.

!!! tip
    在windows中, 你可以拖动 `包含package.json的文件夹` to `nw.exe` 去运行的你的App.

### 例子 2 - 使用 NW.js APIs


所有的Nw.js的API都在nw这个全局对象中,并且你可以在任何一个JS文件中使用它.[API 引用](https://github.com/wsqy182/nw.js/blob/nw25/docs/index.md#references)中可以查看所有支持的API的清单.

这个例子告诉在你的NW.jsAPP中如何去创建一个原生的上下文菜单.你可以使用下面的内容创建`index.html`.

```html
<!DOCTYPE html>
<html>
<head>
  <title>Context Menu</title>
</head>
<body style="width: 100%; height: 100%;">

<p>'Right click' to show context menu.</p>

<script>
// 创建一个空的上下文菜单
var menu = new nw.Menu();

//添加菜单项
menu.append(new nw.MenuItem({
  label: 'Item A',
  click: function(){
    alert('You have clicked at "Item A"');
  }
}));
menu.append(new nw.MenuItem({ label: 'Item B' }));
menu.append(new nw.MenuItem({ type: 'separator' }));
menu.append(new nw.MenuItem({ label: 'Item C' }));
// 挂载"contextmenu"事件
document.body.addEventListener('contextmenu', function(ev) {
  // 防止显示默认的上下文菜单
  // Prevent showing default context menu
  ev.preventDefault();
  // 在你点击的地方弹出原生上下文菜单
  menu.popup(ev.x, ev.y);

  return false;
}, false);

</script>  
</body>
</html>
```

... 然后运行的APP:
```bash
cd /path/to/your/app
/path/to/nw .
```

!!! tip "require('nw.gui')"
使用 `require('nw.gui')`加载NW.js APIs的传统方式也是支持的.它返回相同的'nw'对象.

### 例子 3 - 使用 Node.js API

你可以在dom中直接调用node.js和相应模块.因此,它为使用NW.js编写应用程序提供了无限的可能性.

这个示例展示了了如何使用node.js的"OS"模块去查询操作系统.简单的使用下面的内容创建一个`index.html` 文件,然后用NW.js运行.


```html
<!DOCTYPE html>
<html>
<head>
  <title>My OS Platform</title>
</head>
<body>
<script>
// get the system platform using node.js
var os = require('os');
document.write('You are running on ', os.platform());
</script>
</body>
</html>
```

你还可以通过[npm](https://www.npmjs.com/)去安装NW.js的模块去使用.


!!! note "原生NODE模块"
原生NODE模块,当使用`npm install`去构建的时候,是与NW.js的API不兼容的.如果要使用他们,你不得不重构这些源码[`nw-gyp`](https://github.com/nwjs/nw-gyp).[使用原生Node模块](https://github.com/wsqy182/nw.js/blob/nw25/docs/For%20Users/Advanced/Use%20Native%20Node%20Modules.md)查看更多细节.

## 接下来呢?

查看 [调试与开发工具](https://github.com/wsqy182/nw.js/blob/nw25/docs/For%20Users/Debugging%20with%20DevTools.md) 去调试NW.js apps.

查看 [包和分类](https://github.com/wsqy182/nw.js/blob/nw25/docs/For%20Users/Package%20and%20Distribute.md) 打包和重新分配你的App

查看 [FAQ(常见文件)](https://github.com/wsqy182/nw.js/blob/nw25/docs/For%20Users/FAQ.md) 对于你可能遇到的问题.

查看 [迁移笔记](Migration/From 0.12 to 0.13.md), 如果你正在迁移你的App从NW.js 0.12或更老的版本.

## 获取帮助

这里有很多有用的信息 [NW.js wiki](https://github.com/nwjs/nw.js/wiki). 
维基也为所有人开放,你也可以在维基上发表你的知识.

你也可以在 [谷歌组邮件列表](https://groups.google.com/forum/#!forum/nwjs-general) 或者[Gitter](https://gitter.im/nwjs/nw.js)聊天室问问题.

请在[GitHub](https://github.com/nwjs/nw.js/issues)报告Bug或者提交意见/要求,让Nw.js更强大.
