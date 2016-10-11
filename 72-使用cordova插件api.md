> 名词参考：
* dot notation：点式标记 eg:org.apache.cordova.camera，
* hyphenated notation：连字标记 eg:org-apache-cordova-camera  

# 开始使用Cordova插件
参考Cordova文档：
> "A plugin is a package of injected code that allows the Cordova web view within
which the app renders to communicate with the native platform on which it runs.
Plugins provide access to device and platform functionality that is ordinarily
unavailable to web-based apps. All the main Cordova API features are implemented
as plugins, and many others are available that enable features such as bar code
scanners, NFC communication, or to tailor calendar interfaces."
> 插件是一个注入包，他允许渲染app的web view与他运行的本地平台进行通讯。
插件提供了设备和平台功能的访问接口，这些是网页应用访问不了的。
所有的Cordova主体API都实现为插件，也有一些其他可用的接口，例如二维码扫描，NFC通讯以及tailor calendar。

换句话说，Cordova插件是你的设备功能的窗口。Cordova/Phonegap团队以及为基本上所有的设备功能创建了可用的插件。
也有社区贡献的插件，他们自定义封装了很多设备特定的功能。
> 可以在此处查找已有的插件：http://plugins.cordova.io/

在本章教程期间，我们将探索一些插件。

> 注意Cordova插件正在迁移到NPM。此书发布的时候；所有的Cordova将会完成到NPM的迁移。更多信息，参考：https://cordova.apache.org/announcements/2015/04/21/plugins-releaseand-move-to-npm.html

为调整之前的变更，作为一个开发人员你这边不需要做任何变更，除了使用Cordova CLI（版本大于等于5.0.0）来添加插件。
这样将会从合适的注册点下载对应的插件。
> Ionic团队已经合并了一个pull请求：将dot notation改为hyphenated notation。参考：https://github.com/driftyco/ionic-cli/pull/409

  
由于我们的关注点是Ionic开发，我们将使用Ionic CLI来添加插件。他本身就是调用Cordova CLI的。

## Ionic插件API
在使用插件的过程中，主要有4个用到的命令。

### 添加插件
这个CLI命令用来给项目添加一个新的插件，例如：
```
ionic plugin add org.apache.cordova.camera
```
也可以这样：
```
ionic plugin add cordova-plugin-camera
```
### 移除插件
这个CLI命令用来从项目移除一个插件的，例如：
```
ionic plugin rm org.apache.cordova.camera
```
也可以这样：
```
ionic plugin rm cordova-plugin-camera
```
### 列出添加的插件
此CLI命令是用来列出所有项目里的插件的，例如：
```
ionic plugin ls
```
### 查找插件
此CLI命令是用在命令行中搜索插件的，例如：
```
ionic plugin search scanner barcode
```
  
为测试以上命令，新建一个项目然后逐个测试一下：
```
ionic start -a "Example 28" -i app.example.twentyeight example28 blank
```
> 创建空白模板的时候，我们会下载和设置一下几个插件：
* cordova-plugin-device
* cordova-plugin-console
* cordova-plugin-whitelist
* cordova-plugin-splashscreen
* com.ionic.keyboard

使用*cd*命令进入*example28*文件夹内运行如下命令以备测试：
```
ionic serve
```
我们先来搜索电池状态插件，然后将他添加到项目。关闭服务然后运行：
```
ionic plugin search battery status
```
编写本章的时候，看到的是如下的结果：  
![search plugin](imgs/chapter-7-5.png 'search plugin')
  
运行此命令，有可能只找得到hyphenated版本，也许都可以找得到。  
根据搜索到的插件名字，将它添加到项目中。  
所以，在我们案例中，我将运行如下命令以将电池状态的插件添加到项目：
```
ionic plugin add org.apache.cordova.battery-status
```
这样电池状态插件（https://github.com/apache/cordovaplugin-battery-status）将会添加到当前项目。  
此时，再次运行上面的命令时，将会看到：  
![search plugin](imgs/chapter-7-6.png 'search plugin')
  
CLI打印出来一个警告告诉你插件已被重命名，下载的插件可能不是最新的。  
那么，我们来使用最新版吧。但是，在我们使用连字版本之前，我们需要移除已经添加插件，如下：
```
ionic plugin rm org.apache.cordova.battery-status
```
添加连字命名方式的插件：
```
cordova plugin add cordova-plugin-battery-status
```
想要查看我们安装的所有插件的话，运行如下命令：
```
ionic plugin ls
```
然后你会看到以下：  
![list plugins](imgs/chapter-7-7.png 'list plugins')
  
之前说过，*com.ionic.keyboard*是用的连字标记法，其他的都是用的点式标记法。
当你运行这个命令的时候，应该只能看到连字命名标记。  
在继续测试电池状态插件之前，我们需要在代码中用别的方式来使用。打开*www/js/app.js*。在*run*方法里，快到*ionicPlatform.ready*回调的地方，添加以下代码：
```
alert(device.model);
window.addEventListener("batterystatus", onBatteryStatus,false);
function onBatteryStatus(info) {
    // Handle the online event
    alert("Level: " + info.level + " isPlugged: " + info.isPlugged);
}
```
我们添加了一个警告窗口展示设备模型（需要用到*cordova-plugin-deveice*插件），然后添加了一个电池状态变更的事件监听（需要用到*cordova-plugin-battery-status*）。  
此时，运行：
```
ionic serve
```
你将看到并没有警告窗口弹出来，然后，当你打开开发者工具的时候，会发现报错了：*device is not defined*  
这是因为我们不能直接在浏览器中运行这些插件；他们需要一些环境去执行，例如Android，iOS，或者是他本身（移动系统本身）的浏览器。  
是的，浏览器是一个单独的平台，像Android，iOS一样，他用来运行*cordova.js*。*cordova.js*是连接JavScript和设备特定语言之间缝隙的桥梁。
根据你使用的插件的类型的不同，你可以选择在浏览器中对部分插件进行模拟。  
我们用这个来试试设备和电池功能。添加一个浏览器平台：
```
ionic platform add browser
```
现在在浏览器平台运行此app，如下：
```
ionic run browser
```
此时将启动一个新的当前默认浏览器的实例，在其中运行此app。现在，就可以看到*device.model*的警告框了。
此时，打开开发者工具，可以看到：  
![browser platform](imgs/chapter-7-8.png 'browser platform')
  
此时，如果在浏览器控制台中运行*navigator.battery*的时候，会发现*battery*对象里面都是*null*属性。  
为更好的测试测试此app（和插件），我们需要添加一个Android平台或者iOS平台：
```
ionic platform add android
```
或者：
```
ionic platform add ios
```
然后执行以下任意一个命令：
* ionic emulate android
* ionic emulate ios
* ionic run android
* ionic run ios

然后你就可以看到正确的显示信息了：  
![browser platform](imgs/chapter-7-9.png 'browser platform')

现在，你已经知道怎样给Ionic项目添加Cordova插件以及如何对他们进行测试了。
下一步分钟，我们将学习ngCordova和其他一些插件。
> 以上截屏来自我的个人版Genymotion。此图仅供展示之用。

