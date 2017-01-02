## 开发应用
首先，我们将新建和设置app。

## 新建和设置app
运行如下命令，新建一个标签页应用：
```
ionic start -a "Ionic Chat App" -i app.ionic.chat ionic-chat-app tabs
```
通过*cd*命令，进入*ionic-chat-app*文件夹运行：
```
ionic serve
```
然后就可以看到标签页范例的app了。  
继续深入之前，我们将通过Bower安装应用所需的依赖。在项目的根目录下，运行：
```
bower install ngCordova ng-cordova-oauth firebase angularfire lato --save
```
这些bower组件的使用主旨：
* ngCordova：ngCordova库
* ng-cordova-oauth：编写本书的时候，*ng-cordova-oauth*有个问题是与ngCordova捆绑的问题，所以我们另外安装和使用他。
我现在面对的这个问题在你学习本书的时候可能已经解决了。
* firebase：这是firebase的源代码
* angularfire：这是AngularFire的源代码
* lato：Lato字体（(https://www.google.com/fonts/specimen/Lato）

> 我没从Google加载Lato字体，因为我已经在本机安装了。这样就确保了在本机没有联网的情况下字体可以正常使用。
你也可以参考*localFont*来实现几秒钟内本地存储Web字体的缓存（https://github.com/jaicab/localFont），这样也可以在不用本机安装的情况下正常显示字体。

接下来，我们将给项目添加SCSS支持；运行如下：
```
ionic setup sass
```
现在，我们给下载下来的依赖库添加引用。在*index.html*中进行如下变更。  
首先，我们先看一下*ng-cordova*和*ng-cordova-oauth*。以下两个*script*标签是在Ionic bundle之后，*cordova.js*之前：
```
<script src="lib/ngCordova/dist/ng-cordova.js"></script>
<script src="lib/ng-cordova-oauth/dist/ng-cordovaoauth.js"></script>
```
我们也要在app添加添加指令来管理地图。我们稍后会添加这个指令，但是现在只要添加引用就可以了。  
在*services.js*文件的引用之后添加如下*script*标签：
```
<script src="js/directives.js"></script>
```
接下来，我们需要添加Google地图API的引用。在*head*标签的最后添加如下脚本：
```
<script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyDgE3k3per7mf0qjZLWwlbMXQL1OhH-x44&sensor=true"></script>
```
> 我将给你展示如何在使用Google认证设置里面获取你自己的Google API key（上面脚本中的script标签）

最后，我们将添加Lato字体。在*ionic.app.css*的引用之上，添加：
```
<link href="lib/lato/css/lato.min.css" rel="stylesheet">
```
> 时过境迁，上面参考的资源可能已经不是如今的路径了。如果资源出现“not found（404）”错误，在*lib*文件夹里面重新检查他的位置。

我们会将*body*标签上的模组名从*starter*改为*IonicChatApp*。
接着，将*nav bar*类型从*bar-stable*改为*bar-positive*。  
做完这些，我们就完成了*index.html*的设置。  
接下来，打开*www/js/app.js*。由于我们在index页面上对模组进行了重命名操作，我们在*app.js*也需要对他进行重命名。修改后的AngularJS模组声明如下：
```
angular.module('IonicChatApp', ['ionic', 'chatapp.controllers','chatapp.services', 'chatapp.directives', 'ngCordova','ngCordovaOauth', 'firebase'])
```
我们也重命名了控制器和服务的命名空间，在指令模组，*ngCordova*, *ngCordovaOayth*和*Firebase*中添加引用。
> 注意，我们将*ngCordovaOauth*模组作为依赖添加到了主模组。这是因为打包版（ng-cordova.js）在本章编写的时候有个issue。
如果你使用打包版的Cordova oAuth插件；你就不需要包含此依赖库和他的源代码了。
