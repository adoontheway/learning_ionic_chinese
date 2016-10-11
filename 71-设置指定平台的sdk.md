# 设定特定平台SDK
在与设备功能交互之前，我们需要在本机设置设备的操作系统对应的SDK。Ionic只正式支持iOS,Android以及部分Windows phont平台的扩展。
但是，Ionic还是可以用在任何设备上。  
以下链接展示了如何在本机上设置移动SDK。
很可惜的是本章不会进行更多的设置。链接如下：
* Android : http://cordova.apache.org/docs/en/5.0.0/guide_platforms_android_index.md.html#Android%20Platform%20Guide
* iOS:http://cordova.apache.org/docs/en/5.0.0/guide_platforms_ios_index.md.html#iOS%20Platform%20Guide
* Windows Phone8 :  http://cordova.apache.org/docs/en/5.0.0/guide_platforms_wp8_index.md.html#Windows%20Phone%208%20Platform%20Guide
> 对于其他OS，参考： http://cordova.apache.org/docs/en/5.0.0/guide_platforms_index.md.html#Platform%20Guides ; 我使用的是Cordova 5.0.0的文档

本书中只对Android与iOS进行设置。其他操作系统也是类似的。  
在继续操作之前，我们需要确保设置完成，并且工作正常。
> 关于本章，你也可以通过以下Github目录来访问源代码，发起issue，与作者沟通:
https://github.com/learning-ionic/Chapter-7

## Android平台设置
确保安装好了Android的SDK以及Android tools在你的环境变量path中。然后，在任何地方打开命令行/终端，运行：
```
android
```
这个命令将启动Android SDK管理器。先确保你安装了最新版本的Android，或者某个特定的版本。  
接下来，运行如下命令：
```
android avd
```
这个命令将启动Android Virtual Device（安卓虚拟设备）管理器。确保至少设置了一个AVD。如果没有的话，点击上面的**Create**按钮创建一个，如下：  
![create avd](imgs/chapter-7-0.png 'create avd')
  
## iOS平台设置
先确保安装好了Xcode和他所需的工具，同时也要确保全局安装了*ios-sim*和*ios-deploy*：
```
npm install -g ios-sim

npm install -g ios-deploy
```
> iOS设置只能在Apple机器上进行。Windows开发人员不能从Windows上面部署iOS app，因为Xcode只能在iOS上使用。

## 测试设置
我们看一下如何测试Android和iOS的设置。

### 测试Android
为测试设置是否成功，我们新建一个Ionic应用，然后使用Android和iOS模拟器进行模拟：
我们将新建一个标签页应用：
```
ionic start -a "Example 27" -i app.example.twentyseven example27 tabs
```
使用*cd*口令进入*example27*文件夹内，运行：
```
ionic serve
```
这样，应用就运行起来了。我们就可以通过浏览器访问此应用了。  
为了能在Android模拟器中模拟此应用，首先我们需要给项目添加Android平台支持，然后再模拟。  
添加Android支持，使用以下命令：
```
ionic platform add android
```
命令运行成功之后，运行此命令：
```
ionic emulate android
```
经过短暂的等待之后，我们可以看到模拟器启动，app部署其中，并且在其中运行：  
![emulate android](imgs/chapter-7-1.png 'amulate android')
  
如果你之前用过Android模拟器的话，那么你就应该体会到他有多慢。如果没用过的话，那么我告诉你他真的是好慢。  
另一个可选的Android模拟器是Genymotion（(https://www.genymotion.com）。Ionic也很好的与Genymotion整合了。  
Genymotion有两个版本，一个免费版和一个商业版。免费版功能较少，仅支持个人使用。
> 可以从此处下载一个Genymotion的副本：https://www.genymotion.com/#!/store

一旦安装好了Genymotion，就可以创建一个你想要的Android SDK的虚拟设备了。以下是我的配置：  
![emulate android](imgs/chapter-7-2.png 'amulate android')
  
接下来我们就可以启动模拟器让他在后台运行了。  
现在，我们的Genymotion模拟器运行起来了，我们就需要告诉Ionic使用Genymotion而不是Android默认的模拟器了，使用如下口令：
```
ionic run android
```
替换：
```
ionic emulate android
```
这个命令将会把app部署到Genymotion模拟器，并且与Android模拟器不同的是，你可以立刻看到效果：  
![emulate android](imgs/chapter-7-3.png 'amulate android')
  
> 一定要确保Genymotion在后台运行。

如果Genymotion对你来说偏贵，那么你也可以简单的连接你的Android移动电话到你的笔记本电脑，然后运行：
```
ionic run android
```
这样，app将会被部署到甚至设备上去。
> 设置Android USB调试，请参考： http://developer.android.com/tools/device.html
> 之前截屏里的Genymotion是一个私人版的，因为我没有买授权。
开发期间我都是使用iOS模拟器连接我的Android手机的。一旦完成开发，我从在线测试服务订购设备使用，然后在上面进行测试。
> 如果你在连接Android手机与电脑的时候出现问题，请先检查一下时候可以在命令行/终端里面运行*adb*命令，且命令可以列出你的设备。
更多关于Android Debug Bridge（ADB）的信息，请参考：http://developer.android.com/tools/help/adb.html

以上是测试Android app的不同方式。

### 测试iOS
要测试iOS，首先要添加iOS支持，然后进行模拟。  
运行：
```
ionic platform add ios
```
然后：
```
ionic emulate ios
```
然后你可以看到默认模拟器运行，最后app出现了：  
![emulate ios](imgs/chapter-7-4.png 'amulate ios')
  
可以使用以下命令部署到Apple设备：
```
ionic run ios
```
深入之前要确保你可以模拟app。