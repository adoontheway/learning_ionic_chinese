## Cordova白名单（whitelist）插件
在开始学习ngCordova之前，我们将花点时间熟悉一个关键的Cordova插件 - 白名单插件 https://github.com/apache/cordova-plugin-whitelist

Cordova文档里面是这样描述白名单插件的：
> "Domain whitelisting is a security model that controls access to external domains
over which your application has no control. Cordova provides a confgurable
security policy to defne which external sites may be accessed."
> 领域白名单是一个安全模型，用来控制外部域名的访问，在此之上你的应用没有控制权。
Cordova提供了一个可配置的安全策略供配置哪些外部网站可以访问。

所以当你在使用其他资源内容，想要更多控制权的时候，你就会用得到白名单插件。你也许已经注意到了，咱们的Ionic项目已经添加了白名单插件。

*译者：听起来与ActionScript 3的SecurtiyDomain.allowDomain差不多*

如果Ionic项目或者Cordova项目没有添加此插件的，只要运行如下命令就可以添加了：
```
ionic plugin add cordova-plugin-whitelist
```
一旦插件添加完成，就可以去更新*config.xml*里面的导航白名单-也就是你的app允许在webview里面访问的连接。  
你可以这样添加让app的webview里面可以访问*example.com*：
```
<allow-navigation href="http://example.com/*" />
```
如果你想要允许webview访问任何网站的话：
```
<allow-navigation href="http://*/*" />
<allow-navigation href="https://*/*" />
<allow-navigation href="data:*" />
```
你也可以添加一个Intent白名单，在此处可以指定允许在设备上浏览的链接列表。例如，从我们的app里面打开SMS app：
```
<allow-intent href="sms:*" />
```
或者一个简单的网页：
```
<allow-intent href="https://*/*" />
```
也可以通过插件在设备上增加Content Security Policy（CSP） http://content-securitypolicy.com/ 。
你所要做的只是在*www/index.html*的*meta*标签里面添加如下内容：
```
<!-- Allow XHRs via https only -->
<meta http-equiv="Content-Security-Policy" content="default-src 'self' https:">
```
这是针对白名单插件的一个快速浏览，白名单插件适用于：
* Android 4.0.0及以上版本
* iOS 4.0.0及以上版本
> 记住，要添加此插件并且配置好才能访问外部链接。在*第六章 创建一个书店应用*里面，确保白名单插件正确设置；否则，当你将app部署到设备是的时候，app运行会有问题。

## ngCordova
之前的案例中，我们整合了一些插件，并且使用了他们的JavaScript API与他们进行交互。你可能发现了，所有插件都处于同一个全局命名空间。
与AngularJS的依赖注入哲学不同的是，Cordova插件处于用以全局命名空间之下可以从任何地方进行访问。当使用依赖注入理念搭建的应用在测试的时候，可能会带来麻烦。  
因此，Ionic团队封装了Cordova插件，由此你就可以将这些功能作为服务进行注入。在之前的案例中，我们注入一个*$cordovaDevice*，然后使用*$cordovaDevice.getModel*方法来替换*device.model*。  
ngCordova库不是为Ionic特定的；他也可以用在任何使用AngularJS制作的Cordova app中。  
本章编写的时候，ngCordova已有71个插件。  
现在，我们测试驱动一些ngCordova插件。  
## 设置ngCordova
使用ngCordova之前，我们需要下载并添加他为依赖。新建一个空白模板项目来测试一下：
```
ionic start -a "Example 29" -i app.example.twentynone example29 blank
```
接下来，添加ngCordova作为项目依赖。使用*cd*命令进入*example29*：
```
bower install ngCordova --save
```
为验证ngCordova在项目中是否添加正确，导航至*www/lib*文件夹，你将会看到一个名为ngCordova的文件夹；在*dist*文件夹内，你可以找到一个文件名为*ng-cordova.min.js*。  
接下来，添加此文件的引用，然后将ngCordova作为依赖注入项目。  
打开*www/index.html*，加上：
```
<!-- ngCordova -->
<script src="lib/ngCordova/dist/ng-cordova.min.js"></script>
```
> ngCordova脚本应该放在*ionic.bundle.js*之后，*cordova.js*之前。如果顺序错误，控制台会有错误信息输出。

接下来，我们需要在我们的模组中添加ngCordova依赖。打开*www/js/app.js*，更新Angular模组声明：
```
angular.module('starter', ['ionic', 'ngCordova'])
```
由于*cordova-plugin-device*已经预装好了，我们现在可以使用*$cordovaDevice*服务了。  
如下更新*www/js/app.js*中的*run*方法：
```
.run(function($ionicPlatform, $cordovaDevice) {
    $ionicPlatform.ready(function() {
        // Hide the accessory bar by default (remove this to show the accessory bar above the keyboard
        // for form inputs)
        if (window.cordova && window.cordova.plugins.Keyboard) {
            cordova.plugins.Keyboard.hideKeyboardAccessoryBar(true);
        }
        if (window.StatusBar) {
            StatusBar.styleDefault();
        }
        alert('Platform : ' + $cordovaDevice.getPlatform() + '\nModel: ' + $cordovaDevice.getModel());
    });
})
```
我们就可以看到警告弹出设备平台和模组信息了。
> 任何处理插件相关的代码必须放在*$ionicPlatform.ready*中

## Legend 说明
从今往后，当我说，“给Ionic app添加一个平台”的时候，意味着你应该运行：
```
ionic platform add android
```
也可以：
```
ionic platform add ios
```
当我说，“为Ionic app添加ngCordova支持”的时候，意味着你应该运行：
```
bower install ngCordova --save
```
接下来，跟早先一样，在*www/index.html*中加入*ng-cordova.min.js*。最后将ngCordova添加到AngularJS模组作为依赖。  
当我说，“模拟Ionic app”的时候，意味着你应该运行：
```
ionic emulate android
```
或者：
```
ionic emulate ios
```
最后，当我说“运行Ionic app”的时候，意味着你应该运行：
```
ionic run android
```
或者：
```
ionic run ios
```
  
现在，给*example29* app添加平台并进行模拟的时候。可以看到：  
![finally](imgs/chapter-7-10.png 'finally')
  
这是一个使用ngCordova的端对端的范例。下一部分，我们将通过ngCordova服务来使用一些Cordova插件。  
每个插件我都会创建一个新的项目，这样你后续就可以轻松的进行参考。如果你跟随我的脚步进行练习的话，最好别这样做；所有插件用在一个项目里就好了。  
