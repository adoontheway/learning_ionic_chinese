## 导航
接下来出场的是导航相关的组件。导航组件有许许多都的指令和许许多多的服务。  
第一个出场的指令是*ion-nav-view*。  
在*第三章 Ionic CSS组件和导航*中，我们已经学习过Ionic状态路由器并了解了他是如何工作的。我们也看过了Ionic中的*ion-nav-view*是如何向AngularJS里面的*ui-view*一样工作的。  
当app启动的时候，*$stateProvider*将会查找默认的状态，然后尝试加载*ion-nav-view*里面对应的模板。  
### ion-view
接下来出场的是*ion-view*指令。*ion-view*是*ion-nav-view*的额砸。这个指令用来作为添加页头信息和其他内容的容器。当应用状态改变的时候，会在父容器*ion-nav-view*内展示对应的视图。  
在Ionic中，为改善执行效率，视图都会被缓存起来。当视图离开*ion-nav-view*之后，他的字元素都将从DOM中移除，他的scope也将和*$watch*循环断开连接。
当之前缓存的视图进入*ion-nav-view*的时候，他的scope将会重新连接，已有的元素都将重新激活。  
新建一个空白模板项目来学习：
```
ionic start -a "Example 18" -i app.example.eighteen example18 blank
```
通过*cd*目录，进入到*example18*文件夹，然后运行：
```
ionic serve
```
接下来，我们将给这个app添加一个路由器，这个路由器有2个状态。打开*www/js/app.js*在*run*方法后添加以下方法：
```
.config(function($stateProvider, $urlRouterProvider) {
    $stateProvider
    .state('page1', {
        url: '/page1',
        templateUrl: 'page1.html'
    })
    .state('page2', {
        url: '/page2',
        templateUrl: 'page2.html'
    });
    $urlRouterProvider.otherwise('/page1');
})
```
我们分别给*page1*和*page2*创建了两个状态。接下来，修改*www/index.html*的body部分：
```
<body ng-app="starter">
    <ion-nav-bar></ion-nav-bar>
    <ion-nav-view></ion-nav-view>
    <!-- Templates -->
    <script type="text/ng-template" id="page1.html">
        <ion-view view-title="Title">
            <ion-content>
            <h3>Page 1</h3>
            <button class="button button-dark" uisref="page2">Navigate to Page 2</button>
            </ion-content>
        </ion-view>
    </script>
    <script type="text/ng-template" id="page2.html">
        <ion-view view-title="Title">
            <ion-content>
            <h3>Page 2</h3>
            <button class="button button-dark" uisref="page1"> Navigate to Page 1</button>
            </ion-content>
        </ion-view>
    </script>
</body>
```
我们创建了一个供*ion-view*内容注入的*ion-nav-view*指令。我们的视图是通过*script*标签语法创建的。这样一来，AngularJS就不用发起AJAX请求来加载模板来。
但是这么做对于开发者和维护者来说都是很不友好的。  
*ion-view*指令有一个子指令*ion-content*。每次导入新模板的时候，他都会进行缓存。你可以通过更改*ion-view*的属性来控制视图状态的外观，同理也可以控制缓存行为。  
例如，在以上代码中，我们给*ion-view*标签添加一个*view-title*属性。这个值是用作页面标签，同时在没有*ion-nav-bar*的情况下，也可以用作导航条标题。  
如果想要禁用模板缓存，在*ion-view*上面设置*cache-view*属性为*false*就可以了。同时也可以设置*hide-nav-bar*来控制是否显示*nav-bar*。  
加上上面这些属性之后，*ion-view*看起来差不多是这样子的：
```
<ion-view view-title="Title" cache-view="false" hide-navbar="false" hide-back-button="true" can-swipe-back="false">
```
为避免在导航条里面显示返回按钮，我们也可以控制返回按钮的显示。在后续学习*ion-nav-bar*的时候会学到这一点。  
运行以下命令，可以验证目前所有对*example18*的更改：
```
ionic serve
```

### Ionic视图事件
Ionic视图有很多生命周期方法，你可以在这些方法里添加你的自定义行为。我们将给*example18*添加一个*run*方法，代码如下。
在*run*方法中，我们给*$ionicview*添加了事件监听器。打开*www/js/app.js*添加以下代码：
```
.run(function($ionicPlatform, $rootScope) {
    // View Life Cycle
    $rootScope.$on('$ionicView.loaded', function(event, view) {
        console.log('Loaded..', view.stateName);
    });
    $rootScope.$on('$ionicView.beforeEnter', function(event, view)
    {
        console.log('Before Enter..', view.stateName);
    });
    $rootScope.$on('$ionicView.afterEnter', function(event, view)
    {
        console.log('After Enter..', view.stateName);
    });
    $rootScope.$on('$ionicView.enter', function(event, view) {
        console.log('Entered..', view.stateName);
    });
    $rootScope.$on('$ionicView.leave', function(event, view) {
        console.log('Left..', view.stateName);
    });
    $rootScope.$on('$ionicView.beforeLeave', function(event, view)
    {
        console.log('Before Leave..', view.stateName);
    });
    $rootScope.$on('$ionicView.afterLeave', function(event, view)
    {
        console.log('After Leave..', view.stateName);
    });
    $rootScope.$on('$ionicView.unloaded', function(event, view) {
        console.log('View unloaded..', view.stateName);
    });
})
```
> 你可以给一个单独的模块添加多个*run*方法。

这样，在我们从page1导航到page2的时候，你将会看到下面这样的结果：  
![run](imgs/chapter-5-5.png 'run')
  
你可以追踪这些事件的触发顺序，然后根据需求给他们绑定你的自定义行为。  
> *$ionicView.loaded*和*$ionicView.unloaded*只会在控制器的*$rootScope*里生效，*$scope*里面不会有任何效果。其他的方法可以。

## ion-nav-bar
当我们制作多页面应用的时候,*ion-nav-bar*将会是你的好基友。这个指令负责在你的状态改变的时候更新页面标题栏。在*example18*中，我们添加了一个简化版的*ion-nav-bar*。  
现在，我们来看一下稍微强化版的。在*www/index.html*中，使用以下代码替换*ion-nav-bar*：
```
<ion-nav-bar class="bar-assertive">
<ion-nav-buttons side="left">
<button class="button button-energized" ngclick="leftyClick()">
Left Button
</button>
</ion-nav-buttons>
<ion-nav-back-button class="button-clear">
<i class="ion-arrow-left-c"></i> Back
</ion-nav-back-button>
<ion-nav-buttons side="right">
<button class="button button-energized" ngclick="rightyClick()">
Right Button
</button>
</ion-nav-buttons>
</ion-nav-bar>
```
在这段代码中，你可以看到*ion-nav-bar*使用*ion-nav-buttons*或者*ion-nav-back-button*作为子指令。  
*ion-nav-buttons*用于将按钮展示在页头导航条。更新后的页面效果如下：  
![navigation](imgs/chapter-5-6.png 'navigation')
  
你可以定义*leftyClick()*和*rightyClick()*功能以供按钮点击的时候调用。也可以添加一个Header Controller并定义这些方法，然后将Header Controller添加给*ion-nav-bar*;
或者也可以在root scope里面定义*lefyClick()*和*rightyClick()*，虽然这个做法不怎么理想化。在实际环境中，根据情况右上角一般显示的是**Logout**按钮，**Option**按钮或者**Add**按钮。  
*ion-nav-bar*也是*ion=nav-back-button*的宿主。这个指令负责在页面之间导航的时候自动显示返回按钮：  
![run](imgs/chapter-5-7.png 'run')
  
看到了吗，返回按钮自动出现了，把左边的按钮从原先的位置挤走了。  
> *ion-nav-bar*指令只有在模板内容被包装在*ion-view*标签里的时候才会正常工作。

那么，这样我们就又回到了*ion-view*标签的属性来。你可以设置*ion-view*的*hide-nav-bar*属性为*false*；这样将会为当前页面显示导航。或者你可以设置*back-button*为*false*以隐藏页面上的返回按钮。  
修改后的page2的*ion-view*如下：
```
<ion-view view-title="Page 2" hide-nav-bar="false" hide-backbutton="true">
```
此时，当你从page1导航只page2的时候，你会发现返回按钮不见了：  
![run](imgs/chapter-5-8.png 'run')
  
### ion-nav-buttons
Ionic对页头里面的按钮提供了细粒度的控制。如果你在*ion-view*中声明了*ion-nav-buttons*，在模板中，他们将会覆盖*ion-nav-bar*指令里面的。  
我们将*page2*模板更新如下：
```
<script type="text/ng-template" id="page2.html">
<ion-view view-title="Page 2" hide-nav-bar="false" hideback-button="true">
<ion-nav-buttons side="left">
<button class="button button-calm" ngclick="settingsClick()">
Settings
</button>
</ion-nav-buttons>
<ion-nav-buttons side="right">
<button class="button button-calm" ngclick="optionsClick()">
Options
</button>
</ion-nav-buttons>
<ion-content>
<h3>Page 2</h3>
<button class="button button-dark" uisref="page1">
Navigate to Page 1
</button>
</ion-content>
</ion-view>
</script>
```
记住，我们不是对*ion-nav-bar*里面的*ion-nav-buttons*进行更改：
```
<ion-nav-bar class="bar-assertive">
<ion-nav-buttons side="left">
<button class="button button-energized" ngclick="leftyClick()">
Left Button
</button>
</ion-nav-buttons>
<ion-nav-back-button class="button-clear">
<i class="ion-arrow-left-c"></i> Back
</ion-nav-back-button>
<ion-nav-buttons side="right">
<button class="button button-energized" ngclick="rightyClick()">
Right Button
</button>
</ion-nav-buttons>
</ion-nav-bar>
```
保存文件回到浏览器，page1效果如下：  
![run](imgs/chapter-5-9.png 'run')
  
page2在模板内显示了*ion-nav-button*：  
![run](imgs/chapter-5-10.png 'run')

## $ionicNavBarDelegate
可以在控制器内控制*ion-nav-bar*，*$ionicNavBarDelegate*服务也可以。  
为了更好的理解，我们给模板添加两个控制器：page1.html的*PageOneCtrl*和page2.html的*PageTwoCtrl*。  
更新后的模板如下：
```
<script type="text/ng-template" id="page1.html">
<ion-view ng-controller="PageOneCtrl">
<ion-content>
<h3>Page 1</h3>
<button class="button button-dark" uisref="page2">
Navigate to Page 2
</button>
</ion-content>
</ion-view>
</script>
<script type="text/ng-template" id="page2.html">
<ion-view ng-controller="PageTwoCtrl">
<ion-content>
<h3>Page 2</h3>
<button class="button button-dark" uisref="page1">
Navigate to Page 1
</button>
</ion-content>
</ion-view>
</script>
```
注意看我们移除了*ion-view*上面的所有属性和指令然后给他添加了*ng-controller*。  
我们需要去*www/js/app.js*里面更新这两个生命的控制器：
```
.controller('PageOneCtrl', function($scope, $ionicNavBarDelegate)
{
    $ionicNavBarDelegate.title('Page 1');
})
.controller('PageTwoCtrl', function($scope, $ionicNavBarDelegate)
{
    $ionicNavBarDelegate.title('Page 2');
    $ionicNavBarDelegate.showBackButton(false);
})
```
我们给*page1*和*page2*设置了标题,并且将*page2*的*showBackButton*设置为*false*。保存这些修改返回浏览器的时候，就可以看到你预想的结果了。  
其他可以通过*$ionicNavBarDelegate*控制的属性有：
* align：这个是用来指定标题，按钮的排列方向的:left, right, 以及center（默认）
* showBar：用来设置或者取得*ion-nav-bar*是否显示

### $ionicHistory
另一个非常重要的导航服务是*$ionicHistory*。*$ionicHistory*持续追踪所有视图并记录用户在视图之间的导航行为。  
*$ionicHistory*的优美之处在于他支持平行历史记录，在这点上，浏览器是按固定顺序存储的。当你的界面有多个标签，每个标签都有一套视图的时候，平行历史记录就会非常有用了。  
*$ionicHistory*可以用来捕获标签级别的历史记录；意思是，当用户选择标签2的时候，在标签2里面进行了一些页面导航，之后选择标签1，然后点击返回按钮；应用不会把用户带回标签2最后显示的页面，
而是导航到标签页的父容器的最后显示页，或者如果当前父容器页面的第一个页面的话就退出了应用。  
回到手边的范例，更新*PageTwoCtrl*为以下：
```
.controller('PageTwoCtrl', function($scope, $ionicNavBarDelegate, $ionicHistory) {
    $ionicNavBarDelegate.title('Page 2');
    $ionicNavBarDelegate.showBackButton(false);
    console.log($ionicHistory.viewHistory())
})
```
我们在*PageTwoCtrl*中注入*$ionicHistory*依赖然后在控制台记录视图切换历史记录。  
保存文件，返回浏览器，然后从page1导航到page2，我们可以看到如下展示：  
![run](imgs/chapter-5-11.png 'run')
  
*viewHistory*方法返回了一个对象，里面有*backView*（也就是之前视图），*currentView*，*Histtories*以及应用里面所有其他的视图的所有信息。  
可以通过这个对象得到用户是如何导航到当前页面的；在此情景之下，视图历史非常有用。  
你依然可以通过*$ionicHistory*服务的函数读取视图历史的独立属性，例如：
* currentView：返回应用当前页面
* currentHistoryId：返回当前视图父容器的ID
* currentTitle：取得或者设置当前视图的标题
* backView：返回当前视图在历史记录栈里面的上一个视图
* forwardView：然后历史栈中的下一个视图。前视图当中用户从page1导航至page2，然后返回page1的时候有效。此时page2就是*forwardView*。
* currentStateName：返回当前状态名
  
为快速测试以上属性，我们更新*PageOneCtrl*和*PageTwoCtrl*如下：
```
.controller('PageOneCtrl', function($scope, $ionicNavBarDelegate,$ionicHistory) {
    $ionicNavBarDelegate.title('Page 1');
    console.log('currentView', $ionicHistory.currentView());
    console.log('currentHistoryId', $ionicHistory.currentHistoryId());
    console.log('currentTitle', $ionicHistory.currentTitle());
    console.log('backView', $ionicHistory.backView());
    console.log('backTitle', $ionicHistory.backTitle());
    console.log('forwardView', $ionicHistory.forwardView());
    console.log('currentStateName', $ionicHistory.currentStateName());
})
.controller('PageTwoCtrl', function($scope, $ionicNavBarDelegate,$ionicHistory) {
    $ionicNavBarDelegate.title('Page 2');
    $ionicNavBarDelegate.showBackButton(false);
    console.log('viewHistory', $ionicHistory.viewHistory());
})
```
现在，当我们导航到page1的时候，可以记录的属性如下：  
![run](imgs/chapter-5-12.png 'run')
  
然后，当导航到**Page 2**的时候，可以看到早先看到的相同的值：  
![run](imgs/chapter-5-13.png 'run')
  
最后，当你再次导航回**Page 1**的时候，将会看到有*forwardView*了：  
![run](imgs/chapter-5-14.png 'run')
  
> 我已经为page1和page的*ion-view*指令设置了 *cache-view="false"*；因此，上面截屏中的*backView*是*null*。

*$ionicHistory*还有3个其他的方法：
* goBack：默认状态路由返回1个视图的历史距离。你可以传入一个负整数来制定返回多少个视图的历史距离。因为默认值是-1，所以返回的是一个视图的历史距离。如果执行*goBack(-2)*那么将返回两个视图的历史距离。
*goBack*不会超过历史栈，如果传入的值超过历史栈的话，会直接返回到第一个页面。
* clearHistory：清除除了当前页面之外的其他历史栈
* clearCache：清除所有缓存的视图

可以使用*$ionicHistory*的*nextViewOptions*方法来控制下一个视图如何展示。  
你可以控制以下选项：
* disableAnimate：禁用下一个视图的动画效果
* disableBack：将下一个视图的*backView*设为*null*
* historyRoot：将下一个视图设置为历史栈的根

将以上属性添加到我们的*PageOneCtrl*：
```
.controller('PageOneCtrl', function($scope, $ionicNavBarDelegate,$ionicHistory) {
    $ionicNavBarDelegate.title('Page 1');
    console.log('currentView', $ionicHistory.currentView());
    console.log('currentHistoryId', $ionicHistory.currentHistoryId());
    console.log('currentTitle', $ionicHistory.currentTitle());
    console.log('backView', $ionicHistory.backView());
    console.log('backTitle', $ionicHistory.backTitle());
    console.log('forwardView', $ionicHistory.forwardView());
    console.log('currentStateName', $ionicHistory.currentStateName());
    $ionicHistory.nextViewOptions({
        disableAnimate: true,
        disableBack: true,
        historyRoot: true
    });
})
```
此时，导航到page2，控制台的输出如下：  
![run](imgs/chapter-5-15.png 'run')
  
你会发现，当点击**Navigate to Page 2**的时候，动画效果和过度效果都被禁用了，*backView*是*null*，最后*views*属性只有一个视图：*page2*。  
你也可以使用这些选项来控制你的应用的历史状态的行为。  
