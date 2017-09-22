# 调试和开发工具
---

!!! note "SDK Flavor Only"
    开发工具仅在[SDK风格](Advanced /Build Flavors.md)中可以使用.建议使用SDK风格个开发和测试你的APP,并使用其他的风格来生产.
    

## 打开开发者工具

在windows和linux上可以通过快捷键F12打开开发者工具.在mac上则是ctrl+shift+i.

Alternatively, you may open DevTools programmatically using NW.js API [win.showDevTools()`](../References/Window.md##winshowdevtoolsiframe-headless-callback) for a window.

或者,你也可以使用NW.js的API在一个窗口.以编程方式打开开发者工具[win.showDevTools()](https://github.com/wsqy182/nw.js/blob/nw25/docs/References/Window.md##winshowdevtoolsiframe-headless-callback).

## Node.js 模块的调试

NW.js默认是运行在[独立的上下文模式](https://github.com/wsqy182/nw.js/blob/nw25/docs/For%20Users/Advanced/JavaScript%20Contexts%20in%20NW.js.md#separate-context-mode)上的.去调试NODE.js模块,你可以右键点击APP,选择""InspectBackgroundPage"(检视背景页).当走到NODE.js 的debugeer,背景页的开发工具会自动获取焦点且在该条语句(debugger处)停止运行.

如果你的APP运行在[Mixed Context Mode(混合上下文模式)](https://github.com/wsqy182/nw.js/blob/nw25/docs/For%20Users/Advanced/JavaScript%20Contexts%20in%20NW.js.md#mixed-context-mode),Node.js模块可以直接在窗口的开发者工具中进行调试是,参见[JavaScript Contexts in NW.js(NW.js中的JS上下文)](https://github.com/wsqy182/nw.js/blob/nw25/docs/For%20Users/Advanced/JavaScript%20Contexts%20in%20NW.js.md)去比较差异.

## 远程调试

你可以使用`--remote-debugging-port=port`命令行选项去指定开发者工具应该侦听的端口.例如:通过运行`nw --remote-debugging-port=9222`,你可以打开 http://localhost:9222/ 在你的浏览器流远程访问调试器.

## 使用devtools扩展

开发者工具扩展是完全支持的,包括对ReactJS的etc.要使用它,可以在开发者扩展中添加“chrome -extension:// *”的权限到mainfest.json中,当使用  `--load-extension=path/to/extension`命令启动nw后,你从chrome 的web store安装他们后,nw.js会从chrome浏览器的扩展文件夹中copy这些扩展程序的文件.

### 例子

https://s3-us-west-2.amazonaws.com/nwjs/sample/react-app.zip
https://s3-us-west-2.amazonaws.com/nwjs/sample/react-devtools.zip

解压他们,下载这个SDK构建且使用`nw.exe --load-extension=path/to/devtools path/to/app/folder`运行.


这款应用程序是一个使用'package.json'添加的简单的react app.这个开发者工具的文件是从chrome浏览器的扩展文件夹devtools文件来自Chrome浏览器的扩展文件夹,是从Chrome的Web Store安装的官方react开发者工具扩展从webStore安装的.在NW.js使用只要修改这个mainfest.file文件添加一个权限: "chrome-extension://*".



