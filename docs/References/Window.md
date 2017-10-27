# window {: .窗口}
---

[TOC]

"窗口"是DOM最顶层的"窗口"对象的包装.它扩展了操作,可以接收各种窗口事件。

每个"窗口"都是event发射器类的一个实例,您可以使用“Window.on(…)”来响应原生窗口的事件。


!!! 警告: "行为改变"
    从0.13.0"窗口"开始有一些的变化.请参见[从0.12到0.13的迁移说明](https://github.com/wsqy182/nw.js/blob/nw25/docs/For%20Users/Migration/From%200.12%20to%200.13.md)

## Synopsis(剧情简介)

```javascript
// Get the current window 获取当前窗口
var win = nw.Window.get();

// Listen to the minimize event 监听最小化的事件
win.on('minimize', function() {
  console.log('Window is minimized');
});

// Minimize the window 最小化窗口
win.minimize();

// Unlisten the minimize event 移除最小化事件
win.removeAllListeners('minimize');

// Create a new window and get it 创建一个新的窗口并保存指针
nw.Window.open('https://github.com', {}, function(new_win) {
  // And listen to new window's focus event
  new_win.on('focus', function() {
    console.log('New window is focused');
  });

});
```

## Window.get([window_object]) 

* `window_object` `{DOM Window}` _Optional_ 指定的dom窗口
* Returns `{Window}` 返回原生窗口对象

如果未指定参数 `window_object`,将会返回当前窗口的`Window`对象,否则返回`window_object`的`Window`对象.

!!! 笔记: 如果`window_object`是`iframe`类型,这个方法仍然会返回最顶层窗口的`Window`对象.


```javascript
// Get the current window 获取当前窗口
var win = nw.Window.get();

// Get iframe's window 获取iframe的窗口
var iframeWin = nw.Window.get(iframe.contentWindow);

//This will return true 这里仍然会返回真
console.log(iframeWin === win);

// Create a new window and get it 创建一个新的窗扣且保存.
nw.Window.open('https://github.com/nwjs/nw.js', {}, function(new_win) {
  // do something with the newly created window
  // 创建完一个新的窗口后将执行此处代码
});
```

## Window.open(url, [options], [callback]) 窗口.打开(url,[可选项],[回调])

!!! 警告 "行为改变"
    函数的行为从0.13.0开始变化。请参见[从0.12到0.13](https://github.com/wsqy182/nw.js/blob/nw25/docs/For%20Users/Migration/From%200.12%20to%200.13.md)

* `url` `{String}` URL将在打开的窗口中加载
* `options` `{Object}` _可选_ 参见 [窗口子属性](https://github.com/wsqy182/nw.js/blob/nw25/docs/References/Manifest%20Format.md#window-subfields)在清单的格式,另外,在选项中也可以使用以下额外的字段.
    - `new_instance` `{Boolean}` _可选_ 是否在单独的渲染过程中打开一个新窗口。
    - `inject_js_start` `{String}` _可选_ 在载入文件之前要注入的脚本.参见 [Manifest format](https://github.com/wsqy182/nw.js/blob/nw25/docs/References/Manifest%20Format.md#inject_js_start)
    - `inject_js_end` `{String}` _可选_ 在未加载文档之前注入脚本. 参见 [Manifest format](https://github.com/wsqy182/nw.js/blob/nw25/docs/References/Manifest%20Format.md#inject_js_end)
    - `id` `{String}` _可选_  用来识别窗口的`id`.这将用于记住窗口的大小和位置.并在稍后打开带有相同id的窗口时恢复到相同的位置.[也可以参考下Chrome应用文档](https://developer.chrome.com/apps/app_window#type-CreateWindowOptions)
* `callback(win)` `{Function}` _可选_ 回调将会在打开原生窗口后调用并传入当前窗口对象.

打开一个新窗口，并在其中加载`url`.

!!! 笔记
    您应该等待窗口的`加载`事件,在与它的任何组件交互之前,

!!! 笔记 "Focus"
    打开的窗口不是默认获取焦点的.如果你想让它成为默认获取焦点,你可以在选项中`焦点`设置`true`.

## win.window

获取本机窗口的对应的DOM窗口对象.

## win.x
## win.y

获取或者设置窗口到屏幕左边/顶边的距离.

## win.width
## win.height

获取或者设置窗口的尺寸.

## win.title

获取或者设置窗口的标题.

## win.menu


获取或者设置窗口的菜单栏.设置一个菜单需要类型为`menubar`的对象.当`win.menu`设置为`null`,在Windows和Linux上，menubar被完全删除了,而menubar在Mac上被清除了.

查看[自定义菜单栏](https://github.com/wsqy182/nw.js/blob/nw25/docs/For%20Users/Advanced/Customize%20Menubar.md)去使用菜单栏.查看[菜单](https://github.com/wsqy182/nw.js/blob/nw25/docs/References/Menu.md)和[菜单项](https://github.com/wsqy182/nw.js/blob/nw25/docs/References/MenuItem.md)提供详细的api.

## win.isFullscreen
是否处于全屏模式.

## win.isTransparent 
是否开启透明度

## win.isKioskMode
是否Kiosk模式.

## win.zoomLevel
获取或设置页面缩放.0为正常大小;放大的正值;放小的负值.

## win.cookies.*

这包括操作cookie的多个函数.API的定义方式和[Chrome Extensions'](http://developer.chrome.com/extensions/cookies.html)是一样的.
Nw.js支持`get`,`getAll`,`remove`,和`set`方法;`onChanged`事件(在这个事件上支持`addListener`和`removeListener`函数).

而除此之外的Chrome扩展API中任何与CookieStore相关的东西都不受支持,因为在NW.js apps 中只有一个全局的CookieStore.

## win.moveTo(x, y)

* `x` `{Integer}` 到屏边左边的距离
* `y` `{Integer}` 到屏幕顶边的距离

移动一个窗口到屏幕的指定的坐标.

## win.moveBy(x, y)

* `x` `{Integer}` 横向移动距离
* `y` `{Integer}` 纵向移动距离

相对于它当前的坐标移动窗口.

## win.resizeTo(width, height)

* `width` `{Integer}` 宽度
* `height` `{Integer}` 高度

重新定义窗口的宽和高

## win.resizeBy(width, height)

* `width` `{Integer}` 偏移的宽度
* `height` `{Integer}` 偏移的高度

按指定的数量调整窗口的大小.

## win.focus()

获取焦点

## win.blur()

转移焦点.通常它会把焦点转移到你的应用程序的其他窗口，因为在一些平台上没有焦点的概念.

## win.show([is_show])

* `is_show` `{Boolean}` _可选_ 指定窗口是否应该显示或隐藏.它默认设置为`true`.

显示未显示的窗口.

`show(false)` 和 `hide()`是同样的效果.

!!! 笔记 "Focus"
    `show` 在某些平台上不会自动获取焦点,所以你需要调用`focus`方法来获取焦点.

## win.hide()

隐藏的窗口.用户将无法找到隐藏的窗口。

## win.close([force])

* `force` `{Boolean}` 关闭窗口时是否忽略`关闭`事件.

关闭当前窗口.如果`force`是指定且为true,则`close`事件会被忽略

Usually you would like to listen to the `close` event and do some shutdown work and then do a `close(true)` to really close the window.

通常情况下,你想监听`close`事件,并且做一些结束工作,然后再调用`close(true)`去真的要关必窗口.

```javascript
win.on('close', function() {
  this.hide(); // Pretend to be closed already
  console.log("We're closing...");
  this.close(true); // then close it forcely
});

win.close();
```

## win.reload()

重新载入当前窗口

## win.reloadDev()

通过从头开始一个新的渲染程序来重新加载当前页面.(强制重载.)

## win.reloadIgnoringCache()

类似于 `reload()`,但是不会更新缓存.(又名"shift-reload").
Like `reload()`, but don't use caches (aka "shift-reload").

## win.maximize()

在GTK和Windows上最大化窗口，并在Mac OS X上缩放窗口.

## win.unmaximize()

不最大化窗口,即`maximize()`的反向.

!!! warning "Deprecated"
    该特性自0.13.0开始被弃用. 现在由[`restore` event](#event-restore)代替.请参见[从0.12到0.13](https://github.com/wsqy182/nw.js/blob/nw25/docs/For%20Users/Migration/From%200.12%20to%200.13.md)



## win.minimize()

最小化窗口在Windows上的任务栏,图标化窗口在GTK上,并在Mac上缩小窗口。

## win.restore() 还原

将窗口最小化后恢复到先前状态的窗口,即`maximize()`的反向.它并没有被命名为`unminimize`,因为一般使用`restore`


## win.enterFullscreen()

窗口全屏

!!! 笔记
    这个功能与HTML5 FullScreen API不同,它可以实现页面部分全屏. `Window.enterFullscreen()`只会遮住整个窗口.
    This function is different with HTML5 FullScreen API, which can make part of the page fullscreen, `Window.enterFullscreen()` will only fullscreen the whole window.

## win.leaveFullscreen()

离开全屏

## win.toggleFullscreen()

触发全屏模式.

## win.enterKioskMode()

进入Kiosk模式.在Kiosk模式下,应用程序将是全屏,并试图阻止用户离开app,所以你应该记住在app中提供一种方式,以离开Kiosk模式.这种模式主要用于公开显示.

## win.leaveKioskMode()

退出Kiosk模式.

## win.toggleKioskMode()

触发Kiosk模式

## win.setTransparent(transparent)

!!! 警告 "Deprecated"
    该特性自0.13.0开始被弃用. See [Migration Notes from 0.12 to 0.13](../For Users/Migration/From 0.12 to 0.13.md).

* `transparent` `{Boolean}` 是否设置窗口为透明

打开/关闭透明支持. 查看更多信息 [Transparent Window](https://github.com/wsqy182/nw.js/blob/nw25/docs/For%20Users/Advanced/Transparent%20Window.md).

## win.showDevTools([iframe], [callback])

显示开发者工具

!!! note
    这个API只能在SDK构建版本下使用.
    This API is only available on SDK build flavor.

!!! warning "Behavior Changed"
   函数的行为从0.13.0开始变化. Please see [Migration Notes from 0.12 to 0.13](../For Users/Migration/From 0.12 to 0.13.md).

* `iframe` `{String} or {HTMLIFrameElement}` _可选_ the id or the element of the `<iframe>` to be jailed on. By default, the DevTools is shown for entire window.
* `callback(dev_win)` `{Function}` callback with the native window of the DevTools window.

打开devtools来检查窗口.

The optional `iframe` as `String` should be the value of `id` attribute of any `<iframe>` element in the window. It jails the DevTools to inspect the `<iframe>` only. If it is an empty string, this feature has no effect.

可选项`iframe`作为`String`应该是在窗口中任何一个`<iframe >`元素的`id`属性的值.它只让DevTools检查`<iframe >`.如果是空字符串，则此功能没有效果.

可选项`iframe`作为`HTMLIFrameElement` 应该是一个iframe的对象. `它与`id`参数的作用相同.

这个函数通过回调返回一个`窗口`对象.除了事件，您可以使用`窗口`的任何属性和方法.

参见[webview参考](webview Tag.md).了解如何在webview中打开web view.

## win.closeDevTools()

!!! 笔记
    这个API尽在SDK构建版本中支持.

关闭开发者窗口.

## win.getPrinters(callback)

枚举系统中的打印机。回调函数将接收一个JSON对象数组,用于打印机信息.JSON对象的设备名称可以作为`nw.windows.print()`中的参数使用。

## win.isDevToolsOpen()

!!! 笔记
    这个API尽在SDK构建版本中支持.

Query the status of devtools window.
是否打开开发者窗口.

也可以查看 [`win.showDevTools()`](#winshowdevtoolsiframe-callback).

## win.print(options)

Print the web contents in the window with or without the need for user's interaction. `options` is a JSON object with the following fields:

* `autoprint` `{Boolean}` whether to print without the need for user's interaction; optional, true by default
* `printer` `{String}` the device name of the printer returned by `nw.Window.getPrinters()`; No need to set this when printing to PDF
* `pdf_path` `{String}` the path of the output PDF when printing to PDF
* `headerFooterEnabled` `{Boolean}` whether to enable header and footer
* `landscape` `{Boolean}` whether to use landscape or portrait
* `mediaSize` `{JSON Object}` the paper size spec
* `shouldPrintBackgrounds` `{Boolean}` whether to print CSS backgrounds
* `marginsType` `{Integer}` 0 - Default; 1 - No margins; 2 - minimum; 3 - Custom, see `marginsCustom`.
* `marginsCustom` `{JSON Object}` the custom margin setting; units are points.
* `copies` `{Integer}` the number of copies to print.
* `headerString` `{String}` string to replace the URL in the header.
* `footerString` `{String}` string to replace the URL in the footer.

`marginsCustom` example: `"marginsCustom":{"marginBottom":54,"marginLeft":70,"marginRight":28,"marginTop":32}`  
`mediaSize` example: `'mediaSize':{'name': 'CUSTOM', 'width_microns': 279400, 'height_microns': 215900, 'custom_display_name':'Letter', 'is_default': true}`

*NOTE: If no options are being passed, `win.print({})` is what should be called.*


## win.setMaximumSize(width, height)

* `width` `{Integer}` 最大宽度
* `height` `{Integer}` 最大高度

设置窗口的最大大小.

## win.setMinimumSize(width, height)

* `width` `{Integer}` 最小宽度
* `height` `{Integer}` 最小高度

设置窗口的最小大小.

## win.setResizable(resizable)

* `resizable` `{Boolean}` 窗口是否可以调整大小

设置窗口是否可以调整大小

## win.setAlwaysOnTop(top)

* `top` `{Boolean}` 窗口是否总是在最顶层

设置窗口是否置顶

## win.setVisibleOnAllWorkspaces(visible) (Mac and Linux)

* `visible` `{Boolean}` 是否在所有工作空间都可以看到窗口

对于支持多个工作空间的平台(目前的Mac OS X和Linux)，这允许NW.js窗口在所有工作空间同时可见.

## win.canSetVisibleOnAllWorkspaces() (Mac and Linux)

返回一个布尔值，指示该平台(当前Mac OS X和Linux)是否支持Window API方法`setVisibleOnAllWorkspace(boolean)`.

## win.setPosition(position)

* `position` `{String}` 窗户的位置。有三个有效的位置:`null` or `center` or `mouse`

将窗口移动到指定位置.目前在所有平台上都只支持`center`,这将把窗口放在屏幕中间.

## win.setShowInTaskbar(show)

* `show` `{Boolean}` 是否在任务栏显示

控制是否显示窗口在任务栏或dock. See also `show_in_taskbar` in [Manifest-format](Manifest Format.md#show_in_taskbar).

## win.requestAttention(attension)

* `attension` `{Boolean} or {Integer}` 如果布尔值,则表示请求或取消用户的注意.如果一个整数,它表示窗口闪烁的次数.

通过让窗口在任务栏中闪烁来请求用户的注意力.

!!! 注意 "Mac"
    在Mac上，value < 0将触发`NSInformationalRequest`，而> 0将触发 `NSCriticalRequest`.

## win.capturePage(callback [, config ])

* `callback` `{Function}` 完成捕获窗口时的回调
* `config` `{String} or {Object}` _可选_ 如果是一个字符串, 查看 `config.format` 
    - `format` `{String}` _可选_ 用于生成图像的图像格式.它支持两种格式:"png"和"jpeg"。如果忽略,则默认为"jpeg".
    - `datatype` `{String}` 它支持三种类型:"raw","buffer"和"datauri".如果忽略,则默认为"datauri",

捕获窗口的可见区域.

!!! 注意 "`raw` or `datauri`"
    `"raw"`只包含Base64编码的图像.但`"datauri"`包含mime类型的头,它可以直接分配给`Image`的`src`来加载图像.

Example usage:
```javascript
// png as base64string
win.capturePage(function(base64string){
 // do something with the base64string
}, { format : 'png', datatype : 'raw'} );

// png as node buffer
win.capturePage(function(buffer){
 // do something with the buffer
}, { format : 'png', datatype : 'buffer'} );
```

## win.setProgressBar(progress)

* `progress` `{Float}` valid values within [0, 1]. Setting to negative value (<0) removes the progress bar.

!!! note "Linux"
    Only Ubuntu is supported, and you'll need to specify the application `.desktop` file through `NW_DESKTOP` env. If `NW_DESKTOP` env variable is not found, it uses `nw.desktop` by default.

## win.setBadgeLabel(label)

在任务栏或dock中设置窗口图标上的徽章标签

!!! note "Linux"
    This API is only supported on Ubuntu and the label is restricted to a string number only. You'll also need to specify the `.desktop` file for your application (see the note on [`setProgressBar`](#winsetprogressbar))

## win.eval(frame, script)

* `frame` `{HTMLIFrameElement}` the frame to execute in. If `iframe` is `null`, it assumes in current window / frame.
* `script` `{String}` the source code of the script to be executed

在框架中执行一段JavaScript.

## win.evalNWBin(frame, path)

* `frame` `{HTMLIFrameElement}` the frame to execute in. If `iframe` is `null`, it assumes in current window / frame.
* `path` `{String|ArrayBuffer|Buffer}` the path or `Buffer` or `ArrayBuffer` of the binary file generated by `nwjc`

Load and execute the compiled binary in the frame. See [Protect JavaScript Source Code](../For Users/Advanced/Protect JavaScript Source Code.md).

## win.removeAllListeners([eventName])

移除所有的监听器或者移除所有指定事件的监听器

## Event: close

The `close` event is a special event that will affect the result of the `Window.close()` function. If developer is listening to the `close` event of a window, the `Window.close()` emit the `close` event without closing the window.

`close` 事件是一个特殊的事件他受window.Cliose()方法的影响.如果开发人员监听了close事件,并且调用window.close()去关闭窗口时才会触发这个事件.

通常你会在“关闭”事件的回调中做一些关闭工作，然后调用“this。close(true)”“要真正关上窗户，这样就不会再被抓住了。”忘记在调用“this . close()”时添加“true”在回调中会导致无限循环。

通常情况下,你会在 `close`事件的回调中做一些关闭工作,然后调用 `this.close(true)`去真正的关闭关口.如果调用`this.close()`没有指定`true`,则这个事件会陷入一个死循环.

如果关机工作需要一段时间,用户可能会觉得这款应用程序正在慢慢退出,这是一种糟糕的体验,所以你可以把窗口隐藏在`关闭`的事件中,然后才能真正关闭它,以获得流畅的用户体验.

See example code of [`win.close(true)` above](#wincloseforce) for the usage of `close` event.

!!! note "Mac"
    On Mac, there is an argument passed to the callback indicating whether it's being closed by <kbd>&#8984;</kbd>+<kbd>Q</kbd>. It will be set to string `quit` if that's true, otherwise `undefined`.

## Event: closed

关闭相应的窗口后,将发出“关闭”事件.通常,在窗口关闭后.您将无法获得此事件,所有js对象将被释放.但是,如果您在另一个窗口中监听这个窗口的事件,它的对象将不会被释放,这很有用.

```javascript
// Open a new window.
nw.Window.open('popup.html', {}, function(win) {
// Release the 'win' object here after the new window is closed.
win.on('closed', function() {
  win = null;
});

// Listen to main window's close event
nw.Window.get().on('close', function() {
  // Hide the window to give user the feeling of closing immediately
  this.hide();

  // If the new window is still open then close it.
  if (win != null)
    win.close(true);

  // After closing the new window, close the main window.
  this.close(true);
});

});

```

## Event: loading

当窗口开始重新加载时发出,通常情况下,您无法捕获该事件,因为通常它是在您实际设置回调之前发出的.

您能捕获此事件的惟一情况是刷新窗口并在另一个窗口中侦听此事件.

## Event: loaded

当窗口被完全加载时,该事件的行为与`window.onload`相同,但不依赖于DOM。
Emitted when the window is fully loaded, this event behaves the same with `window.onload`, but doesn't rely on the DOM.

## Event: document-start(frame)

* `frame` `{HTMLIFrameElement}` is the iframe object, or `null` if the event is for the window. 

当文件对象在这个窗口中或子iframe时,在所有文件被加载后，在DOM被构建或任何脚本运行之前发出.
在使用nw.windows.open()创建的新窗口中,它不会被触发:该函数的回调将在该事件的同一点被触发.
Emitted when the document object in this window or a child iframe is available, after all files are loaded, but before DOM is constructed or any script is run.
It will not be fired on the new window being created with nw.Window.open(): the callback of that function will be fired at the same point of this event.

See `inject_js_start` in [Manifest-format](Manifest Format.md#inject_js_start).

## Event: document-end(frame)

* `frame` `{HTMLIFrameElement}` is the iframe object, or `null` if the event is for the window. 
 
Emitted when the document object in this window or a child iframe is unloaded, but before the `onunload` event is emitted.

See `inject_js_end` in [Manifest-format](Manifest Format.md#inject_js_end)

## Event: focus
当窗口获取焦点触发

## Event: blur
当窗口失去焦点触发.

## Event: minimize
当窗口最小化触发

## Event: restore
当窗口恢复触发

!!! warning "Behavior Changed"
    The behavior of the function is changed since 0.13.0. Please see [Migration Notes from 0.12 to 0.13](../For Users/Migration/From 0.12 to 0.13.md).

Emitted when window is restored from minimize, maximize and fullscreen state.

## Event: maximize
当窗口最大化触发

## Event: move(x, y)

在窗口移动后发出触发.这个回调用2个参数调用:`(x,y)`为新位置的左/上距离.

## Event: resize(width, height)
在窗口改变大笑话触发.这个回调有两个参数:`(width, height)`为新窗口的大小.

## Event: enter-fullscreen
在进入全屏模式后触发

## Event: leave-fullscreen
在离开全屏模式后触发

!!! warning "Deprecated"
    This feature is deprecated since 0.13.0. It's now replaced by [`restore` event](#event-restore). See [Migration Notes from 0.12 to 0.13](../For Users/Migration/From 0.12 to 0.13.md).
    
## Event: zoom
在窗口缩放等级被改变后触发

Emitted when window zooming changed. It has a parameter indicating the new zoom level. See [`win.zoom()` method](#winzoom) for the parameter's value definition.

## Event: capturepagedone

!!! warning "Deprecated"
    This feature is deprecated since 0.13.0. Use the callback with `win.capturePage()` instead. See [Migration Notes from 0.12 to 0.13](../For Users/Migration/From 0.12 to 0.13.md).

Emitted after the capturePage method is called and image data is ready. See `win.capturePage()` callback function for the parameter's value definition.

## Event: devtools-opened(url)

!!! warning "Deprecated"
    This feature is deprecated since 0.13.0. Use the `callback` passed to [`win.showDevtools`](#winshowdevtoolsiframe-callback) instead. See [Migration Notes from 0.12 to 0.13](../For Users/Migration/From 0.12 to 0.13.md).

See [`win.showDevTools()` method](#winshowdevtoolsiframe-callback) for more details.

## Event: devtools-closed

Emitted after Devtools is closed.

See [`win.closeDevTools()` method](#winclosedevtools) for more details.

## Event: new-win-policy (frame, url, policy)

* `frame` `{HTMLIFrameElement}` is the object of the child iframe where the request is from, or `null` if it's from the top window.
* `url` `{String}` is the address of the requested link
* `policy` `{Object}` is an object with the following methods:
    * `ignore()` : ignore the request, navigation won't happen.
    * `forceCurrent()` : force the link to be opened in the same frame
    * `forceDownload()` : force the link to be a downloadable, or open by external program
    * `forceNewWindow()` : force the link to be opened in a new window
    * `forceNewPopup()` : force the link to be opened in a new popup window
    * `setNewWindowManifest(m)` : control the options for the new popup window. The object `m` is in the same format as the [Window subfields](Manifest Format.md#window-subfields) in manifest format.

Emitted when a new window is requested from this window or a child iframe. You can call `policy.*` methods in the callback to change the default behavior of opening new windows.

For example, you can open the URL in system brower when user tries to open in a new window:
```javascript

nw.Window.get().on('new-win-policy', function(frame, url, policy) {
  // do not open the window
  policy.ignore();
  // and open it in external browser
  nw.Shell.openExternal(url);
});

```

## Event: navigation (frame, url, policy)

* `frame` `{HTMLIFrameElement}` is the object of the child iframe where the request is from, or `null` if it's from the top window.
* `url` `{String}` is the address of the requested link
* `policy` `{Object}` is an object with the following methods:
    * `ignore()` : ignore the request, navigation won't happen.

当导航到另一个页面时发出.
Emitted when navigating to another page. Similar to [`new-win-policy`](#event-new-win-policy-frame-url-policy), you can call `policy.ignore()` within the callback to ignore the navigation.


