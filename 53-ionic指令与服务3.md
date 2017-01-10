## 标签与侧边菜单
为更好的理解导航，我们将探索一下*tabs*指令与*side menu*指令。  
新建一个*tabs*模板项目，后面学习一下与标签相关的指令：
```
ionic start -a "Example 19" -i app.example.nineteen example19 tabs
```
使用*cd*命令进入*example19*文件夹，运行如下命令：
```
ionic serve
```
此时，标签页应用就运行起来了。  
当你打开*www/index.html*的时候，你会发现这个模板是通过一个内置*ion-nav-back-button*的*ion-nav-bar*来管理页头的。  
接着，打开*www/js/app.js*，找到应用的状态配置：
```
.state('tab.dash', {
    url: '/dash',
    views: {
        'tab-dash': {
            templateUrl: 'templates/tab-dash.html',
            controller: 'DashCtrl'
        }
    }
})
```
注意看*views*对象中有一个名为*tab-dash*的对象。这个会在使用*tabs*指令的时候用到。在选中标签页的时候，这个名字用来加载一个指定的名为*tab-dash*的视图到*ion-nav-view*指令中。  
打开*www/templates/tabs.html*，你将会发现标签页组件的html标记代码：
```
<ion-tabs class="tabs-icon-top tabs-color-active-positive">
    <!-- Dashboard Tab -->
    <ion-tab title="Status" icon-off="ion-ios-pulse" icon-on="ionios-pulse-strong" href="#/tab/dash">
        <ion-nav-view name="tab-dash"></ion-nav-view>
    </ion-tab>
    <!-- Chats Tab -->
    <ion-tab title="Chats" icon-off="ion-ios-chatboxes-outline"   icon-on="ion-ios-chatboxes" href="#/tab/chats">
        <ion-nav-view name="tab-chats"></ion-nav-view>
    </ion-tab>
    <!-- Account Tab -->
    <ion-tab title="Account" icon-off="ion-ios-gear-outline" icon-on="ion-ios-gear" href="#/tab/account">
        <ion-nav-view name="tab-account"></ion-nav-view>
    </ion-tab>
</ion-tabs>
```
由于标签状态是作为一个抽象路由的存在，*tabs.html*将会在其他子标签加载之前完成加载。  
*ion-tab*指令是嵌入在*ion-tabs*指令里面的，每个*ion-tab*指令都有一个*ion-nav-view*嵌入其中。当选中一个标签的时候，与*ion-nav-view*上的*name*属性同名路由将会在对应的标签页中进行加载。  
非常整洁的架构！
> 更多关于*tabs*指令和他的服务的信息请参考：http://ionicframework.com/docs/nightly/api/directive/ionTabs/

接下来，我们将创建一个侧边菜单模板app，然后学习其中的导航：
```
ionic start -a "Example 20" -i app.example.twenty example20 sidemenu
```
通过*cd*命令，进入*example20*文件夹运行如下命令：
```
ionic serve
```
此时，侧边菜单app启动完成。  
我们先从*www/index.html*开始。这个文件的body里面只有一个*ion-nav-view*。接下来，我们打开*www/js/app.js*。在这里，路由都按期望的定义好了。
但是注意观察search，browse和playlist的views的名字，他们都是一样的--*menuContent*：
```
.state('app.search', {
    url: "/search",
    views: {
        'menuContent': {
            templateUrl: "templates/search.html"
        }
    }
})
```
打开*www/templates/menu.html*，将会看到*ion-side-menu*指令。他有两个子元素，*ion-side-menu-content*和*ion-side-menu*。
*ion-side-menu-content*展示了*ion-nav-view*里面名为*menuContent*里的每个菜单条目。这就是为什么所有状态路由器里面的菜单条目名字一样的原因。  
*ion-side-menu*显示在页面的左边。你可以设置他的位置在右边，也可以设置为两边都有。  
注意观察*ion-nav-buttons*内部的按钮上的*menu-toggle*指令。这个指令用来切换侧边菜单的显示。  
如果想要两边都了菜单的话，*menu.html*看起来是如下效果：
```
<ion-side-menus enable-menu-with-back-views="false">
  <ion-side-menu-content>
    <ion-nav-bar class="bar-stable">
      <ion-nav-back-button>
      </ion-nav-back-button>
      <ion-nav-buttons side="left">
        <button class="button button-icon button-clear ionnavicon" menu-toggle="left">
    </button>
      </ion-nav-buttons>
      <ion-nav-buttons side="right">
        <button class="button button-icon button-clear ionnavicon" menu-toggle="right">
    </button>
      </ion-nav-buttons>
    </ion-nav-bar>
    <ion-nav-view name="menuContent"></ion-nav-view>
  </ion-side-menu-content>
  <ion-side-menu side="left">
    <ion-header-bar class="bar-stable">
      <h1 class="title">Left</h1>
    </ion-header-bar>
    <ion-content>
      <ion-list>
        <ion-item menu-close ng-click="login()">
          Login
        </ion-item>
        <ion-item menu-close href="#/app/search">
          Search
        </ion-item>
        <ion-item menu-close href="#/app/browse">
          Browse
        </ion-item>
        <ion-item menu-close href="#/app/playlists">
          Playlists
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-side-menu>
  <ion-side-menu side="right">
    <ion-header-bar class="bar-stable">
      <h1 class="title">Right</h1>
    </ion-header-bar>
    <ion-content>
      <ion-list>
        <ion-item menu-close ng-click="login()">
          Login
        </ion-item>
        <ion-item menu-close href="#/app/search">
          Search
        </ion-item>
        <ion-item menu-close href="#/app/browse">
          Browse
        </ion-item>
        <ion-item menu-close href="#/app/playlists">
          Playlists
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-side-menu>
</ion-side-menus>
```
> 参考此处查看更多关于*side menu*指令和他的服务信息： http://ionicframework.com/docs/nightly/api/directive/ionSideMenus

## Ionic加载
第一个学习的服务是*$ionicLoading*。这个服务在你想要从主页上阻断用户交互的时候，或者告诉用户后台正在进行一些处理的时候非常有用。  
新建一个空白模板项目并实现*$ionicLoading*来进行测试:
```
ionic start -a "Example 21" -i app.example.twentyone example21 blank
```
使用*cd*命令进入*example21*文件夹，运行：
```
ionic serve
```
然后这个项目将会运行在浏览器中。  
然后我们创建一个应用控制器，在其中定义显示和隐藏的方法。
打开*www/js/app.js*添加以下代码：
```
.controller('AppCtrl', function($scope, $ionicLoading, $timeout) {
    $scope.showLoadingOverlay = function() {
        $ionicLoading.show({
            template: 'Loading...'
        });
    };
    $scope.hideLoadingOverlay = function() {
     $ionicLoading.hide();
    };
    $scope.toggleOverlay = function() {
        $scope.showLoadingOverlay();
        // wait for 3 seconds and hide the overlay
        $timeout(function() {
            $scope.hideLoadingOverlay();
        }, 3000);
    };
})
```
我们有一个叫做*showLoadingOverlay*的方法，这个方法将调用*$ionicLoading.show()*，还有一个叫做*hideLoadingOverlay*的方法，这个方法将调用*$ionicLoading.hide()*。
同时我们也创建了一个名为*toggleOverlay()*的工具方法，这个方法将会调用*showLoadingOverlay*方法，3秒钟之后调用*hideLoadingOverlay*。  
我们将在*www/index.html*的body部分更新如下显示：
```
<body ng-app="starter" ng-controller="AppCtrl">
    <ion-header-bar class="bar-stable">
    <h1 class="title">$ionicLoading service</h1>
    </ion-header-bar>
    <ion-content class="padding">
    <button class="button button-dark" ng-click="toggleOverlay()">
    Toggle Overlay
    </button>
    </ion-content>
</body>
```
我们有一个按钮调用*toggleOverlay*。  
保存所有文件，回到浏览器，点击**Toggle Overlay**按钮，将会看到如图效果：  
![run](imgs/chapter-5-16.png 'run')

覆盖层将会在*$ionicLoading*调用*hide*方法之前一直显示。  
你可以将上面的逻辑放入一个服务中，然后在应用中重复利用。服务代码如下：
```
.service('Loading', function($ionicLoading, $timeout) {
    this.show = function() {
        $ionicLoading.show({
            template: 'Loading...'
        });
    };
    this.hide = function() {
        $ionicLoading.hide();
    };
    this.toggle= function() {
        var self = this;
        self.show();
        // wait for 3 seconds and hide the overlay
        $timeout(function() {
            self.hide();
        }, 3000);
    };
})
```
现在，当你想在你的控制器或者指令中注入了*Loading*服务，你就可以使用*Loading.show()*,*Loading.hide()*和*Loading.toggle()*。  
如果你只是想展示一个微调图标而不是文本的话，可以直接调用*$ionicLoading.show*方法，不使用任何选项：
```
$scope.showLoadingOverlay = function() {
    $ionicLoading.show();
};
```
然后，你就可以看到下面这样的效果：  
![run](imgs/chapter-5-17.png 'run')

> 你可以后续再配置*show*方法。更新信息参考： http://ionicframework.com/docs/nightly/api/service/$ionicLoading/
可以使用*$ionicBackdrop*服务来展示一个背景。 http://ionicframework.com/docs/nightly/api/service/$ionicBackdrop
*$ionicModal*与加载服务差不多： http://ionicframework.com/docs/api/service/$ionicModal

## 动作表单服务 Action Sheet service
动作表单是一个向上滑动的展示了一系列选项的面板。通常，当你有一组列表条目或者格子条目的时候，他用来显示上下文选项。动作表单服务一般用在用户长按列表或者栅格条目的时候。  
为测试*$ionicActionSheet*服务，我们新建一个空白模板项目：
```
ionic start -a "Example 22" -i app.example.twentytwo example22 blank
```
使用*cd*口令，进入*example22*文件夹然后运行如下服务：
```
ionic serve
```
然后浏览器中就启动了这个项目了。  
打开*www/js/app.js*新建一个控制器，叫做*AppCtrl*：
```
.controller('AppCtrl', function($scope, $ionicActionSheet,$timeout) {
    $scope.showOptions = function() {
        var hideSheet = $ionicActionSheet.show({
            buttons: [{
                text: 'Open'
            }, {
                text: 'Get Link'
            }],
            destructiveText: 'Delete',
            titleText: 'Options'
        });
        // hide the sheet after three seconds
        $timeout(function() {
            hideSheet();
        }, 3000);
    };
})
```
*$ionicActionSheet.show*返回了一个方法，当这个方法执行的时候，关闭了动作表单。*show*方法接受一个对象作为参数，这个对象有以下几个属性：
* buttons：这个显示了一个选项或者按钮列表。
* destructiveText：高亮一个指定的选项作为一个危险操作
* titleText：设置动作表单的标题

然后更新*www/index.html*的body部分如下：
```
<body ng-app="starter" ng-controller="AppCtrl">
<ion-pane>
<ion-header-bar class="bar-stable">
<h1 class="title">Action Sheet Example</h1>
</ion-header-bar>
<ion-content class="padding">
<button class="button button-block button-dark" ng-click="showOptions()">
Show Options
</button>
</ion-content>
</ion-pane>
</body>
```
保存所有文件，回到浏览器，然后会看到一个**Show Options**按钮。点击将会看到如下效果：  
![run](imgs/chapter-5-18.png 'run')

这个动作表单将会在3秒后隐藏。
> 在*第八章 制作一个聊天app*的实际操作中，我们将会用到动作表单；其中我们会实现动作表单的按钮处理器。
更多关于动作表单的信息参考： http://ionicframework.com/docs/nightly/api/service/$ionicActionSheet/

## Popover与Popup服务
Popover通常是显示在选中条目旁边的一个上下文视图。这个组件用来显示上下文信息或者显示某组件的更多信息。  
新建一个空白模板项目来进行学习：
```
ionic start -a "Example 23" -i app.example.twentythree example23 blank
```
使用*cd*口令进入*example23*，运行：
```
ionic serve
```
浏览器中将运行此项目。  
给项目添加一个新的控制器，名为*AppCtrl*。控制器代码是添加在*www/js/app.js*中的：
```
.controller('AppCtrl', function($scope, $ionicPopover) {
    // init the popover
    $ionicPopover.fromTemplateUrl('button-options.html', {
        scope: $scope
    }).then(function(popover) {
        $scope.popover = popover;
    });
    $scope.openPopover = function($event, type) {
        $scope.type = type;
        $scope.popover.show($event);
    };
    $scope.closePopover = function() {
        $scope.popover.hide();
        // if you are navigating away from the page once
        // an option is selected, make sure to call
        // $scope.popover.remove();
    };
});
```
我们使用的是*$ionicPopover*服务，同一个一个名为*buttons-options.html*的模板设置popover的。可以将当前控制器的scope作为scope传给popover。
控制器scope上有两个方法用来显示和隐藏popover。*openPopover*方法接受两个选项。一个是事件，另一个是我们当前点击的按钮的类型（同时的）。  
接下来，将*www/index.html*的body部分改为如下：
```
<body ng-app="starter" ng-controller="AppCtrl">
  <ion-header-bar class="bar-positive">
    <h1 class="title">Popover Service</h1>
  </ion-header-bar>
  <ion-content class="padding">
    <button class="button button-block button-dark" ngclick="openPopover($event, 'dark')">
    Dark Button
    </button>
    <button class="button button-block button-assertive" ng-click="openPopover($event, 'assertive')">
    Assertive Button
    </button>
    <button class="button button-block button-calm" ng-click="openPopover($event, 'calm')">
    Calm Button
    </button>
  </ion-content>
  <script id="button-options.html" type="text/ng-template">
    <ion-popover-view>
      <ion-header-bar>
        <h1 class="title">{{type}} options</h1>
      </ion-header-bar>
      <ion-content>
        <div class="list">
          <a href="#" class="item item-icon-left">
            <i class="icon ion-ionic"></i> Option One
          </a>
          <a href="#" class="item item-icon-left">
            <i class="icon ion-help-buoy"></i> Option Two
          </a>
          <a href="#" class="item item-icon-left">
            <i class="icon ion-hammer"></i> Option Three
          </a>
          <a href="#" class="item item-icon-left" ng-click="closePopover()">
            <i class="icon ion-close"></i> Close
          </a>
        </div>
      </ion-content>
    </ion-popover-view>
  </script>
</body>
```
在*ion-content*中，我们创建了3个按钮，每个的心情颜色都不一样（黑暗，武断与冷静）。当用户点击按钮的时候，显示按钮指定的popover。
在这个范例中，我们只是把心情传进去，然后将心情作为popover的页头。明显，你可以做更多的逻辑。  
注意，我们的模板内容都是包装在*ion-popover-view*里面的。他会负责对恰当的modal对位。
> 为了使popover工作正常，模板必须包装在*ion-popover-view*里面。
保存所有文件，返回浏览器，我们会看到3个按钮。点击其中一个按钮，popover的页头将会改变，但是选项却都是一样的:  
![run](imgs/chapter-5-19.png 'run')

然后，当我们点击页面上的任何地方或者关闭选项的时候，popover就会关闭。
> 如果在选中选项的时候想要导航到其他页面的话，一定要调用:**$scope.popover.remove();**
更多关于Popover的信息，参考： http://ionicframework.com/docs/api/controller/ionicPopover/
