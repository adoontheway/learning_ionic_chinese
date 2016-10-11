## 工具
本章最后的一个主题我们将用来探索Ionic提供的一些工具服务。首先要上的是*$ionicConfigProvider*。  
Ionic默认是根据他的运行环境插入到应用配置里面的。并且，某些属性将根据环境选择性的去使用，例如transition。些这本书的时候，
Ionic官方只支持Android和iOS。尽管如此，Ionic在其他平台上也是可以使用的。  
所有配置都是基于app的运行的环境的。如果运行环境既不是Android也不是iOS，那么将默认给他使用iOS配置。  
但是，我们还是可以通过*$ionicConfigProvider*服务来控制这些选择。可以通过如下方法来覆盖默认值：
```
.config(function ($ionicConfigProvider) {
    $ionicConfigProvider.views.transition('none');
    $ionicConfigProvider.views.maxCache(10);
    $ionicConfigProvider.form.checkbox('circle'); //square or circle
    $ionicConfigProvider.tabs.style('striped'); // striped or standard
    $ionicConfigProvider.templates.maxPrefetch(10);
    $ionicConfigProvider.navBar.alignTitle('right');
})
```
也可以通过以下方法为指定平台重写配置的默认值：
```
.config(function($ionicConfigProvider) {
    // Checkbox style. Android defaults to square and iOS defaults to circle.
    $ionicConfigProvider.platform.ios.form.checkbox('square');
    $ionicConfigProvider.platform.android.form.checkbox('circle');
})
```
> 可重写的属性请参考： http://ionicframework.com/docs/api/provider/$ionicConfigProvider/

Ionic通过*ionic.platform*提供了一系列的工具方法。可以使用这个对象提供的方法来检查环境信息：
```
.config(function() {
    console.log('ionic.Platform.isWebView()', ionic.Platform.isWebView());
    console.log('ionic.Platform.isIPad()',ionic.Platform.isIPad());
    console.log('ionic.Platform.isIOS()', ionic.Platform.isIOS());
    console.log('ionic.Platform.isAndroid()', ionic.Platform.isAndroid());
    console.log('ionic.Platform.isWindowsPhone()', ionic.Platform.isWindowsPhone());
})
```
> 其他*ionic.platform*方法请查看：http://ionicframework.com/docs/api/utility/ionic.Platform/

还有一些其他的方法帮你与DOM进行交互。这些方法在*ionic.DomUtil*对象里面可以找到。以下列举其中一些：
```
.controller('AppCtrl', function($scope) {
    var $element = angular.element(document.querySelector('#someElement'));
    console.log(ionic.DomUtil.getParentWithClass($element,'.card'));
    console.log(ionic.DomUtil.getParentOrSelfWithClass($element, '.card'));
    // requestAnimationFrame example
    function loop() {
        console.log('Animation Frame Requested');
        ionic.DomUtil.requestAnimationFrame(loop);
    }
    loop();
})
```
> 其他*ionic.DomUtil*方法请查看：http://ionicframework.com/docs/api/utility/ionic.DomUtil/

最后，我们来看一下Ionic的事件控制器（Event Controller）。他是由事件和手势的监听和移除监听的方法所组成的。
你也可以使用*ionic.EventController*的trigger方法来触发事件。  
以下部分展示了如何使用*ionic.EventController*来对事件和手势进行绑定，触发以及接触绑定的。  
再次声明，以下逻辑需要去指令里面实现，然后在想要的元素上应用此指令：
```
.controller('AppCtrl', ['$scope', function($scope) {
    // 绑定事件
    var $body = document.querySelector('body');
    var eventListener = function() {
        console.log('Body Tapped!');
        ionic.EventController.off('tap', eventListener, $body);
    };
    ionic.EventController.on('tap', eventListener, $body);
    ionic.EventController.trigger('tap', {
        target: $body
    });

    // 绑定手势
    var cancelSwipeUp;
    var gestureListener = function() {
        console.log('Body Swiped Up!');
        ionic.EventController.offGesture(cancelSwipeUp, 'swipeup', gestureListener);
    }
    cancelSwipeUp = ionic.EventController.onGesture('swipeup', gestureListener, $body);
    ionic.EventController.trigger('swipeup', {
     target: $body
    });
}])
```

## 总结
本章中，我们学习了大量的Ionic指令和服务以帮助我们轻松的创建应用。我们先从Ionic Platform服务入手，然后进入到页头和页脚指令。
接下来，我们贯穿了所有内容相关（content-related）的指令和导航相关（navigation-related）的指令和服务。接着，我们学习了一下覆盖层（overlay）。
然后，我们快速的了解了列表指令，手势，以及工具服务。  
完成这些之后，我们完成了对整个Ionic的熟悉过程。从下一章开始，我们将利用这些组件来制作简单和复杂的应用。  
在下一章里，我们将制作一个书店（Book Store）应用，用户可以在其中进行注册和登入操作。用户可以通过浏览书的目录将他们添加到购物车。
用户也可以在他们的档案里面检出书籍以及查看购物历史。
这个应用展示了如何整合Ionic与一个安全的REST风格的后端服务。