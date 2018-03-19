# electron打包流程

一个Electron的大小写转换的实用功能举例怎么打包在各个平台,发布包
Electron网上的教程,都写得隐晦难懂,我真的很费解他们是不是故意的,教程只有能让什么都不懂的小白都能看懂,这才是教程

下面,是Electron从建立项目到打包出来一个windows应用程序的完整举例,由于例子太小,我就不往github上面传了

(PS:git是一个代码工具,可以方便的回退版本,分支等等,github是一个网站,用来保存你的代码,一般你上传之后,都是所有人都可以查看的,如果你不想让别人看的代码,要不你不穿上去,要么,购买私人代码空间)

首先,我们声明同步一下系统环境,

电脑win8,自带.net,(PS:其实你可以直接理解为需要一台正常运行的电脑啦)

软件Sublime Text 3  (PS:你用什么代码编译器都可以,比如 VS code,notepad++等等,你用ide我也不反对,相信,看这篇教程的,都或多或少用过webstrom,这个也可以)

软件NodeJS软件,我的版本是6.11.1,(PS:看个人喜好啦,不过最好是6.0版本以上的,低了会有问题,我已被低版本NodeJS坑过)

没了

考虑到很多人的网络并不好,我们可以先设置npm为淘宝的cnpm,这样子,用node下载Electron和其他的依赖包都很快.

切换淘宝镜像的命令,

打开一个DOS窗口,输入

·npm install -g cnpm --registry=https://registry.npm.taobao.org·

创建一个文件夹,我创建的是electronAppDemo

然后在electronAppDemo这个文件夹下面创建三个文件,

1.文件一    package.json

 {
 "name": "demoApp",
 "version": "0.1.0",
 "main": "main.js"
}
2.文件二 main.js

 const {app, BrowserWindow} = require('electron')
const path = require('path')
const url = require('url')
// 保持一个对于 window 对象的全局引用，如果你不这样做，
// 当 JavaScript 对象被垃圾回收， window 会被自动地关闭
let win

function createWindow () {
// 创建浏览器窗口。
win = new BrowserWindow({width: 900, height: 800})

// 加载应用的 index.html。
win.loadURL(url.format({
pathname: path.join(__dirname, 'index.html'),
protocol: 'file:',
slashes: true
}))

// 打开开发者工具。
// win.webContents.openDevTools()

// 当 window 被关闭，这个事件会被触发。
win.on('closed', () => {
// 取消引用 window 对象，如果你的应用支持多窗口的话，
// 通常会把多个 window 对象存放在一个数组里面，
// 与此同时，你应该删除相应的元素。
win = null
})
}

// Electron 会在初始化后并准备
// 创建浏览器窗口时，调用这个函数。
// 部分 API 在 ready 事件触发后才能使用。
app.on('ready', createWindow)

// 当全部窗口关闭时退出。
app.on('window-all-closed', () => {
// 在 macOS 上，除非用户用 Cmd + Q 确定地退出，
// 否则绝大部分应用及其菜单栏会保持激活。
if (process.platform !== 'darwin') {
app.quit()
}
})

app.on('activate', () => {
// 在这文件，你可以续写应用剩下主进程代码。
// 也可以拆分成几个文件，然后用 require 导入。
if (win === null) {
createWindow()
}
})

// 在这文件，你可以续写应用剩下主进程代码。
// 也可以拆分成几个文件，然后用 require 导入。

3.文件三 index.html 

 <!DOCTYPE html>
<html lang="en">
<head>
 <meta charset="UTF-8">
 <title>我是窗口显示的标题</title>
</head>
<body>
 <div>

<p>我会被按钮点击隐藏的P</p>
 <button>点击隐藏上面</button>

</div>
 <script>
 var p = document.getElementsByTagName("p")[0];
 var btn = document.getElementsByTagName("button")[0];
 btn.onclick = toggleP;
 var nextP = document.createElement("p");
 btn.parentNode.appendChild(nextP);
 var pText = p.innerText;
 function toggleP () {
 var that = this;

if("" !== p.innerText){
 p.innerHTML = "";
 
 nextP.innerHTML = "小提示:再次点击,P就会出现哦"; 
 
 }else {
 p.innerHTML = pText;
 nextP.innerHTML = ""; 
 }
 
 }
 </script>
</body>
</html>

具体的操作,请访问 
`http://www.52xunyi.com/index.php/zzyblog/134.html` 
获取
