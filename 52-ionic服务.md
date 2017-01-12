> 用到的名词：
* hook 钩子
* action 动作

# Ionic平台服务

第一个我们将要使用的服务是Ionic Platform服务(*$ionicPlatform*)。这个服务提供设备级别的的钩子，以使你更好的控制你的应用的行为。  
我们从最基本的*ready*方法开始。这个方法会在设备准备好的时候，或者设备已经准备好的时候立即触发。  
我们将新建一个项目来学习Ionic Platform服务。新建一个文件夹名为*chapter5*。在文件夹内打开命令行/终端，运行：
```
ionic start -a "Example 16" -i app.example.sixteen example16 blank
```
app创建好之后，打开*www/hs/app.js*找到以下部分：
```
.run(function($ionicPlatform) {
    $ionicPlatform.ready(function() {
    // Hide the accessory bar by default (remove this to show the  accessory bar above the keyboard
    // for form inputs)
    if(window.cordova && window.cordova.plugins.Keyboard) {
        cordova.plugins.Keyboard.hideKeyboardAccessoryBar(true);
    }
    if(window.StatusBar) {
        StatusBar.styleDefault();
    }
    });
})
```
> 所有与Cordova有关的代码都需要写在*$ionicPlatform.ready*方法里面，因为这里是app生命链中插件初始化和预备使用的点。

可以看到*$ionicPlatform*服务作为依赖注入到了*run*方法。强烈建议在其他的AngularJS组件（如控制器，指令，这些都是你打算与Cordova交互的地方）中使用*$ionicPlatform.ready*方法。  
在之前的*run*方法中，注意我们通过以下设置隐藏了键盘访问条：
```
cordova.plugins.Keyboard.hideKeyboardAccessoryBar(true);
```
你可以重写这个设置为*false*。同时，注意表达式前面的*if*条件。在使用Cordova的变量之前进行检查永远是不会错的。  
*$ionicPlatform*指令有一个侦查硬件返回按钮事件的简便的方法。一些（Android）设备有一个硬件返回按钮，如果你想监听返回按钮的按下时间，
那么你就要对*$ionicPlatform*服务的*onHardwareBackButton*方法进行关联了：
```
var hardwareBackButtonHandler = function() {
    console.log('Hardware back button pressed');
    // do more interesting things here
}
$ionicPlatform.onHardwareBackButton(hardwareBackButtonHandl
er);
```
这个时间需要在*$ionicPlatform.ready*方法中注册，最好是在AngularJS的*run*方法里面。
*hardwareBackButtonHandler*回调函数将会在每次用户按下设备的返回按钮进行调用。  
这个处理的的一个简单的逻辑功能是询问用户是否真的要退出app，以确保他们不会因为错误操作而退出app。  
虽然有时候这很烦人。因此，你可以给用户提供一个设置，用来指定当他退出的时候是否需要显示这个提示。在此之上，你可以延迟注册此事件或者你可以退订此事件。  
上述逻辑的完整代码如下：
```
.run(function($ionicPlatform) {
    $ionicPlatform.ready(function() {
        var alertOnBackPress = localStorage.getItem('alertOnBackPress');
        var hardwareBackButtonHandler = function() {
            console.log('Hardware back button pressed');
            // do more interesting things here
        }
        function manageBackPressEvent(alertOnBackPress) {
            if (alertOnBackPress) {
                $ionicPlatform.onHardwareBackButton(hardwareBackButtonHandler);
            } else {
                $ionicPlatform.offHardwareBackButton(hardwareBackButtonHandler);
            }
        }
        manageBackPressEvent(alertOnBackPress);
        // later in the code/controller when you let
        // the user update the setting
        function updateSettings(alertOnBackPressModified) {
            localStorage.setItem('alertOnBackPress', alertOnBackPressModified);
            manageBackPressEvent(alertOnBackPressModified)
        }
    });
})
// when the app boots up
```
在上面的代码片段里，我们在*localStorage*里面查找*alertOnBackPress*的值。接下来，我们创建了一个名为*hardwareBackButtonHandler*的函数，这个函数将在返回按钮按钮的时候触发。
最后一个名为*manageBackPressEvent()*的工具方法接收一个布尔值，这个布尔值只是是否注册*HardwareBackButton*的回调或者移除注册。  
有了这些设置，当app启动的时候，我们使用从*localStorage*读取的值调用了*manageBackPressEvent*方法。如果在*localStorage*中这个值存在并且为*true*的时候，我们就注册这个事件；
反之则不注册。稍后，我们可以给用户提供一个设置控制器以对这个设置进行更改。当用户改变了*alertOnBackPress*的状态时，我们调用了*updateSettings*方法并传入了用户是否需要被提示。
*updateSettings*更新了*localStorage*里面的设置并且调用了*manageBackPressEvent*方法。  
这是一个很强大的范例，展示了当AngularJS与Cordova组合起来，提供API供你轻松的管理你的应用。
> 这个范例第一眼看起来也许会有点复杂，但是大部分你需要消化的服务都比较类似。你需要根据情况与优先级对事件进行注册和移除注册。所以我觉得在这里放出这个范例是再合适不过了，
这个想法在你完成本章学习之后会更加确定。

### registerBackButtonAction
*$ionicPlatform*提供了另一个名为*registerBackButtonAction*的方法。当你在按下了返回按钮你想要控制你的应用的行为的另一个API。  
默认按下返回按钮只会执行一个任务。例如，当你有一个多页面应用，然后你从page1导航到了page2，这个时候你点击了返回按钮，你就会返回page1.
在另一个场景下，当用户从page1导航到page2，而page2加载的时候弹出了一个对话框，此时按下返回按钮，只会隐藏弹出框，而不会导航到page1.  
*registerBackButtonAction*方法提供了一个钩子去重写这个行为。  
*registerBackButtonAction*接收以下3个参数：
* callback：这个是事件触发的时候将会调用的方法
* priority：这个是事件监听器的优先级
* actionId（可选）：这个是分配给这个动作的ID
默认的优先级有以下几种：
* 预览视图 = 100
* 关闭侧边菜单 = 150
* 清理modal = 200
* 关闭动作表单= 300
* 清理弹出框 = 400
* 清理加载遮盖层 = 500
因此，当你想要重写返回按钮的默认行为的时候，你需要这样子去做：
```
var cancelRegisterBackButtonAction =
  $ionicPlatform.registerBackButtonAction(backButtonCustomHandler,201);
```
这个监听器将重写（抢夺优先权）所有优先级低于201的默认的监听器，也就是清理modal，关闭侧边菜单和预览视图，但是他不会重写优先级高于他的其他的监听器。  
*$ionicPlatform.registerBackButtonAction*执行的时候，返回一个函数。我们已经将这个函数指派给了*cancelRegisterBackButtonAction*变量。
执行*cancelRegisterBackButtonAction*移除*registerBackButtonAction*监听器的注册。  

### on方法
除了上面方法之外，*$ionicPlatform*有一个通用的*on*方法可以用来监听所有的Cordova事件：https://cordova.apache.org/docs/en/edge/cordova_events_events.md.html  
你可以为应用的*pause*（暂停）, *application resume*（重回）,*volumedownbutton*（音量加）,*volumupbutton*（音量减）等等设置钩子，然后根据情况执行自定义的方法。  
可以这样在*$ionicPlatform.ready*方法内设置这些监听器：
```
var cancelPause = $ionicPlatform.on('pause', function() {
    console.log('App is sent to background');
    // do stuff to save power
});
var cancelResume = $ionicPlatform.on('resume', function() {
    console.log('App is retrieved from background');
    // re-init the app
});
// Supported only in BlackBerry 10 & Android
var cancelVolumeUpButton = $ionicPlatform.on('volumeupbutton',function() {
    console.log('Volume up button pressed');
    // moving a slider up
});
var cancelVolumeDownButton = $ionicPlatform.on('volumedownbutton',function() {
    console.log('Volume down button pressed');
    // moving a slider down
});
```
*on*方法返回一个当移除事件监听需要的函数。  
现在你应该知道在处理移动OS事件和硬件按钮的时候如何更好的控制你的app。
## 页头与页脚
使用*ion-header-bar*和*ion-footer-bar*指令，可以给你的app添加一个固定的页头和页脚。
范例结构如下：
```
<ion-header-bar align-title="center" class="bar-assertive">
    <div class="buttons">
        <button class="button button-royal" ngclick="doSomething()">Left Button</button>
    </div>
    <h1 class="title">Fixed Header</h1>
    <div class="buttons">
        <button class="button button-royal">Right Button</button>
    </div>
</ion-header-bar>
<ion-content>
    <div class="padding">
        <h3>Content</h3>
        <p>Lorem ipsum dolor sit amet, consectetur adipisicing
        elit, sed do eiusmod
        tempor incididunt ut labore et dolore magna aliqua. Ut
        enim ad minim veniam,
        quis nostrud exercitation ullamco laboris nisi ut
        aliquip ex ea commodo
        consequat. Duis aute irure dolor in reprehenderit in
        voluptate velit esse
        cillum dolore eu fugiat nulla pariatur. Excepteur sint
        occaecat cupidatat non
        proident, sunt in culpa qui officia deserunt mollit
        anim id est laborum. </p>
    </div>
</ion-content>
<ion-footer-bar align-title="left" class="bar-energized">
    <div class="buttons">
        <button class="button button-dark">Left Button</button>
    </div>
    <h1 class="title">Fixed Footer</h1>
    <div class="buttons" ng-click="doSomething()">
        <button class="button button-dark">Right Button</button>
    </div>
</ion-footer-bar>
```
结果如下：  
![screenshot](imgs/chapter-5-0.png '')

## 内容 Content
接下来将要学习的是内容相关的指令。从*ion-content*指令开始。

### ion-content
*ion-content*用作一个内容显示区域。这条指令有很多选择可以供你更好的控制内容显示区域。你可以对*ion-content*使用Ionic自定义的滚动视图或者使用浏览器默认的覆盖层滚动。  
这本书写作的时候，*ion-content*指令包含的所有属性如下：
```
<ion-content
    delegate-handle=""
    direction=""
    locking=""
    padding=""
    scroll=""
    overflow-scroll=""
    scrollbar-x=""
    scrollbar-y=""
    start-x=""
    start-y=""
    on-scroll=""
    on-scroll-complete=""
    has-bouncing=""
    scroll-event-interval="">
    <h1>Heading!</h1>
</ion-content>
```
*ion-content*关键的几个属性解释如下：
* scroll：决定是否使用滚动，默认：true
* overflow-scroll：使用浏览器的覆盖层滚动
* on-scroll：当显示内容滚动的时候调用的函数/表达式
* on-scroll-complete：当显示内容滚动完成之后执行的函数/表达式
* scroll-event-interval：在调用on-scroll之前的定时器，默认：10ms
* scrollbar-x：展示水平滚动条，默认：true
* scrollbar-y：展示垂直滚动条，默认：true
* locking：锁住滚动，默认：true
* direction：只是如何滚动，可用：x，y（默认），xy
* has-bouncing：允许你滚动超出显示内容的边界，默认：iOS上true， Android上false

### ion-scroll
也可以使用*ion-scroll*指令来控制显示内容的滚动。这个指令用于替换*ion-content*指令的。  
使用方式也是非常简单的：
```
<ion-view ng-controller="MyAppCtrl" cache-view="false">
    <ion-scroll zooming="true" direction="xy" style="width: 300px; height: 300px">
        <div style="width: 1000px; height: 1000px; backgroundcolor:teal"></div>
    </ion-scroll>
</ion-view>
```
> 注意一点：需要将scroll盒的高度设置为内部需要滚动的内容的高度。  如果想要使滚动区域居中，可以使用:ion-content

后面将会谈到*ion-view*的，不要着急。

### ion-refresher
另一个能够方便管理显示内容的指令是*ion-refresher*。这个是添加到滚动视图（*ion-content*或者*ion-scroll*）中用作下拉更新的指令。  
新建一个空白模板项目来测试：
```
ionic start -a "Example 17" -i app.example.seventeen example17 blank
```
使用*cd*命令，进入到*example17*文件夹，运行如下命令：
```
ionic serve
```
这个范例中，我们将实现一个下拉更新的功能。我们将创建一个工厂然后伪造HTTP请求并返回一个对象数组。数组对象是一个伪哈希，有两个用于显示的属性。  
这个工厂将会从*AppCtrl*调用，因为*AppCtrl*是视图的控制器。这个控制器会实现一个*doRefresh*方法，这个方法在用户进行下拉更新操作的时候调用。
这个方法将直接跟工厂对话，拿到随机数据，然后这些数据将会被添加到当前的显示列表。  
我们将在*www/js/app.js*的*config*方法后面定义如下数据工厂：
```
.factory('DataFactory', function($timeout, $q) {
    var API = {
        getData: function(count) {
            // Spoof a network call using promises
            var deferred = $q.defer();
            var data = [],
            _o = {};
            count = count || 3;
            for (var i = 0; i < count; i++) {
                _o = {
                    // http://stackoverflow.com/a/8084248/1015046
                    random: (Math.random() + 1).toString(36).substring(7),
                    time: (new Date()).toString().substring(15, 24)
                };
                data.push(_o);
            };
            $timeout(function() {
                // success response!
                deferred.resolve(data);
            }, 1000);
            return deferred.promise;
        }
    };
    return API;
})
```
> 以上范例可简单使用settiemout替代promise就可以了。但是，由于AngularJS严重依赖promise驱动，所以我们这个范例使用的是promise，我个人也认为应当这么做。

在上面的工厂中，我们用到了AngularJS的*$q*服务，这个服务帮助我们以异步的方式返回结果，类似HTTP请求。
我们创建的返回结果对象包含一个随机字母的字符串作为第一个属性，以日期子串作为第二属性。这个仅做显示用途，没别的意义。  
在*www/js/app.js*中，我们的控制器代码如下：
```
.controller('AppCtrl', function($scope, DataFactory) {
    $scope.items = [];
    $scope.doRefresh = function() {
        DataFactory.getData(3)
        .then(function(data) {
            // extend the $scope.items array with the response
            // array from getData();
            // http://stackoverflow.com/a/1374131/1015046
            Array.prototype.push.apply($scope.items, data);
        }).finally(function() {
            // Stop the ion-refresher from spinning
            $scope.$broadcast('scroll.refreshComplete');
        });
    }
    // load data on page load
    DataFactory.getData(3).then(function(data) {
        $scope.items = data;
    });
})
```
在上面的控制器代码中，我们给控制器注入了DataFactory作为依赖。我们可以在控制器初始化之后调用*getData*方法。*getData*方法将会返回3条记录，这3条记录我们将指派给列表显示。  
接下来，我们定义了用户下拉更新的时候需要的*doRefresh*方法。这个方法也使用了*DataFactory*的*getData*方法并返回了他的返回值。
然后我们将*getData*方法的返回值数组附加到*scope*变量上。  
注意，我们是把数据附加到已有的数组的后面的。如果在用户需要在顶部查看最新数据的时候，那么数据将要前置到已有数组。
最后，我们广播了*scroll.refreshComplete*事件，这样将会隐藏刷新按钮/微调（spinner）。  
现在可以更新*www/index.html*的body部分了：
```
<body ng-app="starter" ng-controller="AppCtrl">
    <ion-header-bar class="bar-stable">
        <h1 class="title">Pull To Refresh</h1>
    </ion-header-bar>
    <ion-content>
        <ion-refresher pulling-text="Pull to refresh..." onrefresh="doRefresh()">
        </ion-refresher>
        <ion-list>
            <ion-item collection-repeat="item in items">
                <h2>Random Key : {{item.random}}</h2>
                <p>Time : {{item.time}}</p>
            </ion-item>
        </ion-list>
    </ion-content>
</body>
```
注意看，我们将*ion-refresher*作为*ion-content*的直接子元素添加进来了。然后我们使用*collection-repeat*替换了*ng-repeat*。
> *collection-repeat*比*ng-repeat*在显示超大列表的时候效率更高。更多信息关于*collection-repeat*请参考： http://ionicframework.com/docs/api/directive/collectionRepeat/

保存所有文件，然后你可以在浏览器中看到如下画面：  
![pull to refresh](imgs/chapter-5-1.png 'pull to refresh')

使用鼠标下拉页面的时候，将会看到这样子的画面：  
![refresh](imgs/chapter-5-2.png 'refresh')

一旦promise完成之后，数据将会立即返回控制器然后附加到条目数组里。所以这些流程完成之后，将会触发 *scroll.refreshComplete*事件，隐藏加载图标/spinner。更新后的页面如下：  
![refresh](imgs/chapter-5-3.png 'refresh')

> 你也可以通过*ion-refresher*的pulling-text, pulling-iocn, spinner属性设置下拉刷新的文本，图标和spinner。其他可用选择，请参考： http://ionicframework.com/docs/api/directive/ionRefresher/

### ion-infinite-scroll
Ionic提供了另一个方便的名为*ion-infinite-scroll*的指令，和*ion-refresher*类似。当用户达到列表的末尾的时候，这个指令将会调用和上面例子里面*doRefresh*方法类似的方法来更新列表。  
*ion-refresher*与*ion-infinite-scroll*的不同点是当用户明确的需要加载一个列表的时候使用*ion-refresher*，而*ion-infinite-scroll*则是当用户滚动的时候自动更新列表。  
想要在之前的范例中使用*ion-infinite-scroll*，只需要将body部分更新为如下即可：
```
<body ng-app="starter" ng-controller="AppCtrl">
    <ion-header-bar class="bar-stable">
        <h1 class="title">Pull To Refresh</h1>
    </ion-header-bar>
    <ion-content>
        <ion-refresher pulling-text="Pull to refresh..." onrefresh="doRefresh()">
        </ion-refresher>
        <ion-list>
            <ion-item collection-repeat="item in items">
                <h2>Random Key : {{item.random}}</h2>
                <p>Time : {{item.time}}</p>
            </ion-item>
         </ion-list>
        <ion-infinite-scroll on-infinite="loadMore()"  distance="1%">
        </ion-infinite-scroll>
    </ion-content>
</body>
```
注意*ion-infinite-scroll*是加载列表后面的，他有一个属性名为*on-infinite*。当距离是1%的时候，将会评估这个属性。
同时我们也把*ion-refresher*留下来了。因为这是一个例子而已，我想要给你展示如何在一个例子中同时使用*ion-refresher*和 *ion-infinite-scroll*。  
在一个实时新闻馈送app中，你也许会这么做。当用户下拉更新的时候，你会要给用户展示最新的新闻，当用户滚动到下面的时候，你需要给用户展示旧新闻。  
更新了*loadMore*的新版本的*AppCtrl*如下：
```
.controller('AppCtrl', function($scope, DataFactory) {
    $scope.items = [];
    $scope.doRefresh = function() {
        DataFactory.getData(3)
        .then(function(data) {
            // extend the $scope.items array with the response
            // array from getData();
            // http://stackoverflow.com/a/1374131/1015046
            Array.prototype.push.apply($scope.items, data);
        }).finally(function() {
            // Stop the ion-refresher from spinning
            $scope.$broadcast('scroll.refreshComplete');
        });
    }
    $scope.loadMore = function() {
        DataFactory.getData(3)
        .then(function(data) {
            // extend the $scope.items array with the response
            // array from getData();
            // http://stackoverflow.com/a/1374131/1015046
            Array.prototype.push.apply($scope.items, data);
        }).finally(function() {
            // Stop the ion-refresher from spinning
            $scope.$broadcast('scroll.infiniteScrollComplete');
        });
    }
    // load data on page load
    DataFactory.getData(3).then(function(data) {
     $scope.items = data;
    });
})
```
我们给scope变量添加了*loadMore*方法。这个基本和*doRefresh*方法差不多，除了我们在这里广播的事件是*scroll.infiniteScrollComplete*。  
保存文件，回到浏览器，你将看到当一部分数据隐藏的时候*ion-infinite-scroll*会开始调用方法了。  
当你下拉刷新的时候，列表会再次更新。但是请记住，我们并不是前置列表，所以数据还是添加到末尾的。

### ionicScrollDelegate
Ionic提供了一个控制滚动视图的服务。名为*$ionicScrollDelegate*。*$ionicScrollDelegate*服务提供了一系列的API，用于滚动，缩放以及取得当前滚动位置等等。  
在之前的范例中，我们可以在页头里面添加一个按钮名为*Scroll To Top*。在用户下拉过列表之后，当用户点击这个，我们将通过*$ionicScrollDelegate*将他滚动到顶部。  
更新后的*ion-header-bar*是这样的：
```
<ion-header-bar class="bar-stable">
    <h1 class="title">Pull To Refresh</h1>
    <button class="button" ng-click="scrollToTop()">Scroll to Top</button>
</ion-header-bar>
```
然后在控制器中，我们在注入了*$ionicScrollDelegate*之后，添加了*scrollToTop*方法定义：
```
$scope.scrollToTop = function() {
    $ionicScrollDelegate.scrollTop();
}
```
现在，无论用户把列表拉到多下面，用户都可以通过按下这个按钮返回列表顶部，如下：  
![scrop to top](imgs/chapter-5-4.png '')

> 更多信息关于*$ionicScrollDelegate*请参考： http://ionicframework.com/docs/api/service/$ionicScrollDelegate
