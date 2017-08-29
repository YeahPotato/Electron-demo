# Electron-demo

###打造你第一个 Electron 应用


大体上，一个 Electron 应用的目录结构如下：
```javascript
your-app/
├── package.json
├── main.js
└── index.html
```
<abbr>package.json</abbr>的格式和Node 的完全一致，并且那个被 main 字段声明的脚本文件是你的应用的启动脚本，它运行在主进程上。你应用里的 package.json 看起来应该像：

```javascript
{
  "name"    : "your-app",
  "version" : "0.1.0",
  "main"    : "main.js"
}
```

注意：如果 main 字段没有在 package.json 声明，Electron会优先加载 index.js。

<abbr>main.js</abbr>应该用于创建窗口和处理系统时间，一个典型的例子如下：
```javascript
var app = require('app');  // 控制应用生命周期的模块。
var BrowserWindow = require('browser-window');  // 创建原生浏览器窗口的模块

// 保持一个对于 window 对象的全局引用，不然，当 JavaScript 被 GC，
// window 会被自动地关闭
var mainWindow = null;

// 当所有窗口被关闭了，退出。
app.on('window-all-closed', function() {
  // 在 OS X 上，通常用户在明确地按下 Cmd + Q 之前
  // 应用会保持活动状态
  if (process.platform != 'darwin') {
    app.quit();
  }
});

// 当 Electron 完成了初始化并且准备创建浏览器窗口的时候
// 这个方法就被调用
app.on('ready', function() {
  // 创建浏览器窗口。
  mainWindow = new BrowserWindow({width: 800, height: 600});

  // 加载应用的 index.html
  mainWindow.loadURL('file://' + __dirname + '/index.html');

  // 打开开发工具
  mainWindow.openDevTools();

  // 当 window 被关闭，这个事件会被发出
  mainWindow.on('closed', function() {
    // 取消引用 window 对象，如果你的应用支持多窗口的话，
    // 通常会把多个 window 对象存放在一个数组里面，
    // 但这次不是。
    mainWindow = null;
  });
});
```

最后，你想展示的 <abbr>index.html ：</abbr>
```javascript
<!DOCTYPE html>
<html>
  <head>
    <title>Hello World!</title>
  </head>
  <body>
    <h1>Hello World!</h1>
    We are using io.js <script>document.write(process.version)</script>
    and Electron <script>document.write(process.versions['electron'])</script>.
  </body>
</html>
```

###运行你的应用
一旦你创建了最初的 <abbr>main.js</abbr>， <abbr>index.html </abbr>和 <abbr>package.json</abbr> 这几个文件，你可能会想尝试在本地运行并测试，看看是不是和期望的那样正常运行。

###electron-prebuild
如果你已经用 npm 全局安装了 electron-prebuilt，你只需要按照如下方式直接运行你的应用：

`electron .`

如果你是局部安装，那运行：

`如果你是局部安装，那运行：``

###electron-packager 打包你的应用
完成功能代码后，我们需要将代码打成可运行的包。可以使用electron-packager来进行应用打包，electron-packager支持windows, Mac, linux等系统。具体介绍可以去[https://github.com/electron-u...](https://electron.atom.io/docs/tutorial/desktop-environment-integration/) 了解。
打包的步骤很简单，只需要两步：

全局安装Node.js模块electron-packager。
运行命令： `electron-packager <sourcedir> <appname> --platform=<platform> --arch=<arch> [optional flags...]`。 其中platform可以取darwin, linux, mas, win324个值；arch可以取ia32, x64两个值。 
需要注意，打包后，代码中的所有路径都必须使用绝对路径，通过${__dirname}获得app的根路径，然后拼接成绝对路径。

`electron-packager ./ MyElectronApp`
会在当前目录下生成桌面应用
