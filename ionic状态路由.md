> 用到的名词：
* router n. 路由器
* route v. 路由
* AngularUI Router AngularUI路由
* padding 填充
* named views 命名视图

## Ionic路由器
在应用比较小只有几个页面的情况下，维护状态和管理数据将会比较简单。但是当应用变得越来越复杂的时候，处理模板，模板数据路由相关数据等等，将会点的很难。
  
因为，为了使管理复杂多页面Ionic应用变得简单，我们使用Ionic路由器。Ionic路由器和AngularUI路由器一样。
更多信息请参考： https://github.com/angular-ui/ui-router
  
在AngularUI路由器文件里：  
> AngularUI Router is a routing framework for AngularJS, which allows you
to organize the parts of your interface into a state machine. Unlike the $route
service in the Angular ngRoute module, which is organized around URL routes,
UI-Router is organized around states, which may optionally have routes, as well
as other behavior, attached.
AngularUI路由器是AngularJS的路由框架，他允许将你的借口组织到一个状态机里面。
与AngularJS ngRouter里面的$routeservice以URL路由组织不同的是，UI-Router是以状态来组织路由的，这个有可能和其他其他附加行为一样有路由行为。

> 更多关于AngularUI 路由的信息，参考：https://github.com/angular-ui/ui-router/wiki

## 一个简单的双页app
Ionic源码里面捆绑了AngularUI。所以，在我们将Ionic添加为依赖之后，我们可以直接注入*$stateProvider*和*$urlRouterProvider*到我们的*config*方法中来，以此建立路由。
我们将通过一些范例学习路由。  
第一个范例中，我们将创建一个双页应用。一个导航按钮在两个页面之中进行导航。
这个范例的目的是了解路由器的语法和设置，这样我们可以将相同的逻辑应用到其他范例中。  
我们将创建一个空白模板项目，然后给他添加路由，这样他就变成了一个多页面应用。  
执行以下命令创建一个空白模板项目：
```
ionic start -a "Example 9" -i app.example.nine example9 blank
```
一旦app创建成功以后，打开*www/js/app.js*。我们将要创建一个名为*config*的方法然后为我们的app添加路由。
我们将在*www/js/app.js*的*run*方法后面添加以下*config*方法：
```
.config(function ($stateProvider, $urlRouterProvider) {
$stateProvider
    .state('view1', {
        url: '/view1',
        template: '<div class="padding"><h2>View 1</h2><button class="button button-positive" ui-sref="view2">To View 2</button></div>'
    })
    .state('view2', {
        url: '/view2',
        template: '<div class="padding"><h2>View 2</h2><button class="button button-assertive" ui-sref="view1">To View 1</button></div>'
    })
    $urlRouterProvider.otherwise('/view1');
})
```
就像你看到的一样，*$stateProvider*和*$urlRouterProvider*作为依赖注入到了*config*方法。
参考主页可知这些服务是和Ionic包一起发布出来的。  
接下来，我们使用*$stateProvider*来定义应用的状态。在这个案例中，状态和视图是一样的。
*$stateProver*上的*state*方法是用来声明路由的。方法的第一个参数是状态的可读名。第二个参数是由路由配置组成的一个对象。
作为路由配置的一部分，我们提佛那个了一个URL和URL触发的时候用做渲染的一个模板。  
在上面的配置中，我们创建了两个状态：一个叫做*view1*，这个将在导航到*http://localhost:8100/#/view1*的时候激活，
第二个叫做*view2*，这个将在导航到*http://localhost:8100/#/view2*的时候激活。  
当你观察URL的时候，会发现在view的名字的前面有一个#(hash 哈希)。这个哈希告诉浏览器不需要向服务器发起资源请求；取而代之的是，这些资源都是在客户端的，
JavaScript框架将会负责去渲染他们。  
简单来说，让URL哈希后面的任何东西改变的时候，会发出一个*hashchange*的事件。路由器有监听器，这些监听器会在事件发出的时候被激活。
这个监听器将负责根据哈希值（*view1* 或者 *view2*）和他的状态配置来管理UI。（简单讲，当哈希改变的时候，路由将为改变了的哈希调用对应的控制器和模板）  
注意，我们已经为视图编写了渲染用的模板。在下一个范例中我们将学习使用外部文件作为模板。
同时按钮有一个名为*ui-sref*的指令（http://angular-ui.github.io/uirouter/site/#/api/ui.router.state.directive:ui-sref）。
*ui-sref*指令用于将链接绑定到状态。如果状态有一个关联的URL，这条指令将自动生成和更新*href*。  
所以，在我们的场景中，当我们点击*view1*模板里面呈现的那个按钮的时候，app将导航到*view2*，反之亦然。  
最后，我们给*config*方法提供一个默认的URL作为结束：
```
$urlRouterProvider.otherwise('/view1');
```
在上面这一行代码中，我们指定了默认的URL，当当前URL不能匹配任何配置的状态URL的时候，用户将会被重定向到*view1*状态。/  
有了这些，我们就已经成功的设置了状态。但是对于设置来将，我们还有一个关键的部分。我们需要告知路由器页面的哪些部分需要使用状态的内容去更新。
这一步通过在我们的*index.html*中添加*ion-nav-view*就可以达到了。  
> 对于状态路由器来将，ion-nav-view和ui-view同样适用。ion-nav-view扩展自ui-view,然后添加了一些功能例如动画和历史。

在*index.html*中，将*ion-content*替换为以下代码：
```
<ion-nav-view class="has-header"></ion-nav-view>
```
*has-header*类在容器的顶部添加了一个padding。这样就确保了模板内容不会是从页头条的后面开始。  
完整的*index.html*的*body*代码如下：
```
<body ng-app="starter">
<ion-pane>
<ion-header-bar class="bar-stable">
<h1 class="title">Two Page Application</h1>
</ion-header-bar>
<ion-nav-view class="has-header"></ion-nav-view>
</ion-pane>
</body>
```
保存所有文件然后运行以下命令：
```
ionic serve
```
你将会看到下面截屏的效果:  
![view1](imgs/chapter-3-25.png 'view1')
  
当点击 **To View2**按钮的时候，他会将你带到**View2**，如下：  
![view2](imgs/chapter-3-26.png 'view2')
  
注意观察导航后的URL。  
接下来的范例中，我们将在单独的HTML文件中创建模板然后在咱们的路由器中进行配置。同时，我们也将为*config*对象引入新的属性，名为controller。  
我们将要创建的是一个双页的app；第一个页面上我们早先创建的登录表单，第二页是那个打分页。  
这个范例的目标是理解外部模板和给视图绑定控制器。老规矩，新建一个空白模板项目：
```
ionic start -a "Example 10" -i app.example.ten example10 blank
```
接下来，我们将要设置路由了。给*www/js/app.js*添加一个*config*方法，如下：
```
.config(function($stateProvider, $urlRouterProvider) {
$stateProvider
.state('login', {
url: '/login',
templateUrl: 'templates/login.html',
controller: 'LoginCtrl'
})
.state('app', {
url: '/app',
templateUrl: 'templates/app.html',
controller: 'AppCtrl'
})
$urlRouterProvider.otherwise('/login');
})
```
我们有两个状态:*login*和*app*。我们用来一个新的属性名为*templateUrl*替换之前的*template*。
*templateUrl*是模板文件的位置。模板文件可以是硬盘上的一个单独的文件，也可以是*index.html*的一部分，例如*script*标签。两种途径我们会都试验一下。  
我们也添加了一个名为*controller*的新属性。这个属性告诉路由器当导航到一个路由的时候需要调用哪一个控制器。如你所见，我们将要为两个控制器创建两个视图。  
为了使用基于*script*标签的模板开始，我们将创建两个空的控制器。在*www/js/app.js*文件的*run*方法后面，加上以下代码：
```
.controller('LoginCtrl', function ($scope) {
})
.controller('AppCtrl', function ($scope) {
})
```
由于我们已经在*route*配置中声明了控制器，AngularJS会在导航到视图的时候去查找相应的控制器。
有鉴于此，我们创建了两个假的控制器。功能代码稍后添加进去。  
接下来，在我们的*www/index.html*中，我们将使用以下代码替换其中的*ion-content*：
```
<ion-nav-view class="has-header"></ion-nav-view>
```
你可以在*body*标签的任何地方添加这个模板。*www/index.html*的*body*部分代码如下：
```
<body ng-app="starter">
<ion-pane>
<ion-header-bar class="bar-stable">
<h1 class="title">My Awesome App</h1>
</ion-header-bar>
<ion-nav-view class="has-header"></ion-nav-view>
</ion-pane>
<script type="text/ng-template" id="templates/login.html">
<h1>Login Template</h1>
<button class="button button-calm" ui-sref="app">To App</button>
</script>
<script type="text/ng-template" id="templates/app.html">
<h1>App Template</h1>
<button class="button button-royal" ui-sref="login">To
Login</button>
</script>
</body>
```
留意*script*标签上的*id*属性。他们跟*templateUrl*是一样的。这就是用来连接脚本*tag/ng-template*到路由*templateUrl*的钩子。  
保存所有文件，运行如下命令：
```
ionic serve
```
你将看到如下结果:  
![view1](imgs/chapter-3-27.png 'view1')
  
当点击**To App**按钮的时候，将会看到如下视图：  
![view1](imgs/chapter-3-28.png 'view1')
  
上面的范例示范了如何使用*script*标签在HTML文件里写模板的。  
在继续我们的真实案例之前，移除我们在*www/index.html*里面的内联模板。  
我们将在*www*文件夹里面创建一个名为*templates*的文件夹-不是项目根目录，是*www*文件夹，千万记住了。  
在*templates*文件夹里面，新建一个文件名为*login.html*。文件内容和我们在*example7*中用的是一模一样的。*login.html*文件看起来是酱紫的：
```
<div class="list">
<label class="item item-input">
<span class="input-label">Email</span>
<input type="email" ng-model="email">
</label>
<label class="item item-input">
<span class="input-label">Password</span>
<input type="password" ng-model="password"
ng-minlength="3">
</label>
<div class="padding">
<button ui-sref="app" ng-disabled="!email || !password"
class="button button-block button-positive">Sign In</button>
</div>
</div>
```
需要注意的是，我们给按钮添加了*ui-sref*。在按钮没有被激活之前，点击这个按钮是不会被重定向到app视图的。  
接下来，在*templates*文件夹下面新建一个名为*app.html*的文件。文件内容和我们在*example8*里面的是一样的，如下：
```
<div class="padding text-center">
<h3>Rate the App</h3>
<div>
<a href="javascript:" ng-repeat="r in ratingArr"
class="padding" style="text-decoration:none;">
<i class="icon {{r.icon}}"
ng-click="setRating(r.value)"></i>
</a>
</div>
<button ui-sref="login" class="button button-block
button-clam">Sign Out</button>
</div>
```
当你保存好了这些文件之后，回到页面，你就会发现login页面出现了。当你输入了一个有效的邮件地址和一个超过3个字符的密码的时候，sign-in按钮将被激活。
当你点击**Sign in**按钮的时候，他会把你带到app视图去。  
> 如果你没看到更新后的UI，这意味着你还没有删除*www/index.html*里面的基于脚本的模板。*scirpt*标签写入的模板的优先级高于硬盘里面的模板，
且硬盘里面的模板需要通过AJAX请求才能得到。关于更多的AngularJS模板缓存，请参考： https://docs.angularjs.org/api/ng/service/$templateCache

打开app视图的时候，会发现小星星部件了。这是因为我们的小星星都是基于一个名誉*ratingArr*的区域变量的。我们将会像在*example8*里面那样更新我们的*AppCtrl*:
```
.controller('AppCtrl', function($scope) {
$scope.ratingArr = [{
value: 1,
icon: 'ion-ios-star-outline'
}, {
value: 2,
icon: 'ion-ios-star-outline'
}, {
value: 3,
icon: 'ion-ios-star-outline'
}, {
value: 4,
icon: 'ion-ios-star-outline'
}, {
value: 5,
icon: 'ion-ios-star-outline'
}];
$scope.setRating = function(val) {
var rtgs = $scope.ratingArr;
for (var i = 0; i < rtgs.length; i++) {
if (i < val) {
rtgs[i].icon = 'ion-ios-star';
} else {
rtgs[i].icon = 'ion-ios-star-outline';
}
};
}
})
```
现在，当你返回查看视图的时候，会发现小星星又出现了，当你点击他们的时候，一切如你预期一样：  
![rating app view](imgs/chapter-3-29.png 'rating app')
  
虽然，我们将*LoginCtrl*留空。只要你想要，你就可以给**Submit**按钮绑定一个*ng-click*然后在控制器里面调用一个方法做你的有效验证。
你可以从按钮标签移除*ui-sref*属性使用控制器里面的*$state*服务导航到app视图。*www/templates/login.html*可以用以下代码替换掉：
```
<button ng-click="validate()" ng-disabled="!email || !password" class="button button-block button-positive">Sign In</button>
```
然后*www/js/app.js*里面的*LoginCtrl*完整代码如下：
```
.controller('LoginCtrl', function($scope, $state) {
$scope.validate = function() {
// some other validations...
$state.go('app');
}
})
```
  
下一个范例中，我们将使用状态路由建立一个稍微复杂一些的UI。我们将建立一个标签组件。但是，首先，我们的看一下AngularUI路由器里面的命名视图。  
假设在这么一个情景里，当我们路由改变的时候页面有3个地方需要更新。使用AngularJS的*ngRoute*的话，是做不到的，因为*ngRoute*路由器只允许我们每个app里面存在一个*ng-view*。
但是AngularUI路由器提供了一些名为“命名视图”的东西，在这里你可以在页面上有多个*ui-view*并且对他们命名。
这样，根据视图状态的不同，将会在这些视图里面加载不同的模板。  
考虑以下HTML，我们在一个页面上有3个视图部分：
```
<body>
<div ui-view="partialview1"></div>
<div ui-view="partialview2"></div>
<div ui-view="partialview3"></div>
</body>
```
这样，当配置我们的路由的时候，我们将在我们路由配置对象中引入一个新的属性，名为*views*。
随后，我们将设计当状态改变的时候，需要调用哪一个控制器和模板。例如：
```
$stateProvider
.state('page1',{
views: {
'partialview1': {
templateUrl: 'page1-partialview1.html',
controller: 'Page1Partialview1Ctrl'
},
'partialview2': {
templateUrl: 'page1-partialview2.html',
controller: 'Page1Partialview2Ctrl'
},
'partialview3': {
templateUrl: 'page1-partialview3.html',
controller: 'Page1Partialview3Ctrl'
}
}
})
.state('page2',{
views: {
'partialview1': {
templateUrl: 'page2-partialview1.html',
controller: 'Page2Partialview1Ctrl'
},
'partialview2': {
templateUrl: 'page2-partialview2.html',
controller: 'Page2Partialview2Ctrl'
},
'partialview3': {
templateUrl: 'page2-partialview3.html',
controller: 'Page2Partialview3Ctrl'
}
}
})
```
如你所见，当你在*page1*状态的时候，三个命名视图都将调用对于的视图和控制器，*page2*也是如此。  
为将相同的理念带入Ionic，我们将对*ion-nav-view*指令添加*name*属性然后使用他来进行命名视图的工作。
我们将使用两个或者或者两个标签页来构建一个页面。  
还是用以下命令搭建一个新的空白模板项目：
```
ionic start -a "Example 11" -i app.example.eleven example11 blank
```
我们的标签界面将会有两个标签页-*login*和*register*。在模组初始化完成之后，我们将会在*www/js/app.js*中去配置这两个状态：
```
.config(function($stateProvider, $urlRouterProvider) {
$stateProvider
.state('login', {
url: '/login',
views: {
login: {
templateUrl: 'templates/login.html'
}
}
})
.state('register', {
url: '/register',
views: {
register: {
templateUrl: 'templates/register.html'
}
}
})
$urlRouterProvider.otherwise('/login');
})
```
注意观察*view*属性，以及他的子属性(*login*和*register*)的名称。这就是我们声明*views*对象的方法。  
接下来，我们将会使用Ionic tabs指令(http://ionicframework.com/docs/api/directive/ionTabs/)进行工作。
这是个非常简单的指令，他使用*ion-tabs*指令包装了*ion-tab*指令来创建一个标签界面。  
这样一来，在我们的*www/index.html*中，我们将*body*标签部分的代码替换为以下：
```
<body ng-app="starter">
<ion-nav-bar class="bar-royal">
</ion-nav-bar>
<ion-tabs class="tabs-royal">
<ion-tab icon="ion-power" ui-sref="login">
<ion-nav-view name="login"></ion-nav-view>
</ion-tab>
<ion-tab icon="ion-person-add" ui-sref="register">
<ion-nav-view name="register"></ion-nav-view>
</ion-tab>
</ion-tabs>
</body>
```
如你所见，*ion-tabs*指令是由两个*ion-tab*指令组成的，*ion-tab*指令是由*ion-nav-view*作为内容视图的。
每个*ion-nav-view*都设置了需要加载的*name*属性。  
现在我们要做的是搭建这两个模板。在*www*文件夹内新建一个名为*templates*的文件夹，然后在其中新建一个名为*login.html*的文件。
*www/templates/login.html*文件的内容如下：
```
<ion-view view-title="Login">
<ion-content class="padding">
<div class="list">
<label class="item item-input">
<span class="input-label">Email</span>
<input type="email" ng-model="email">
</label>
<label class="item item-input">
<span class="input-label">Password</span>
<input type="password" ng-model="password"
ng-minlength="3">
</label>
<div class="padding">
<button ng-disabled="!email || !password" class="button button-block button-royal">Sign In</button>
</div>
</div>
</ion-content>
</ion-view>
```
注意看我们是如何在*ion-view*指令里面包装模块的，然后是在*ion-conent*指令里。接下来，在*www/templates*下面，
新建一个名为*register.html*的文件，内容如下：
```
<ion-view view-title="Register">
<ion-content class="padding">
<div class="list">
<label class="item item-input">
<span class="input-label">Email</span>
<input type="email" ng-model="email">
</label>
<label class="item item-input">
<span class="input-label">Password</span>
<input type="password" ng-model="password"
ng-minlength="3">
</label>
<label class="item item-input">
<span class="input-label">Re-Enter Password</span>
<input type="password" ng-model="password2"
ng-minlength="3">
</label>
<div class="padding">
<button ng-disabled="(!email || !password) || (password != password2)" class="button button-block buttonroyal">Sign In</button>
</div>
</div>
</ion-content>
</ion-view>
```
保存所有文件然后运行如下命令：
```
ionic serve
```
效果图如下：  
![names views](imgs/chapter-3-30.png 'named view')
  
在上面的范例中，我们使用开始模板，从无到有的建立了一个标签组件。我们不用每次都这么做。因为我们有对应的项目模板。
我们可以很快的通过以下命令来新建一个标签页项目：
```
ionic start -a "Example 12" -i app.example.twelve example12 tabs
```
一旦标签页模板项目搭建完成，进入*www/js/app.js*然后滚动*config*方法，你将会看到app的路由配置。如下：
```
.config(function($stateProvider, $urlRouterProvider) {
$stateProvider
.state('tab', {
url: "/tab",
abstract: true,
templateUrl: "templates/tabs.html"
})
.state('tab.dash', {
url: '/dash',
views: {
'tab-dash': {
templateUrl: 'templates/tab-dash.html',
controller: 'DashCtrl'
}
}
})
.state('tab.chats', {
url: '/chats',
views: {
'tab-chats': {
templateUrl: 'templates/tab-chats.html',
controller: 'ChatsCtrl'
}
}
})
.state('tab.chat-detail', {
url: '/chats/:chatId',
views: {
'tab-chats': {
templateUrl: 'templates/chat-detail.html',
controller: 'ChatDetailCtrl'
}
}
})
.state('tab.account', {
url: '/account',
views: {
'tab-account': {
templateUrl: 'templates/tab-account.html',
controller: 'AccountCtrl'
}
}
});
$urlRouterProvider.otherwise('/tab/dash');
});
```
如果你有留意的话，会发现*tab*状态属性有一个新的属性叫做*abstract*设置为*true*。抽象状态是不能切换过去的。
他只会在他的一个子类激活的时候被激活。  
在我们的场景中，*tab*持有者将会是一个抽象状态，并且当他的子tab激活的时候，这个tab状态都将自动激活。  
> 更多关于抽象状态的信息，参考: https://github.com/angular-ui/ui-router/wiki/Nested-States-%26-NestedViews#abstract-states

当你打开*templates/tabs.html*的时候，你可以看到*ion-tabs*指令是设置在一个模板文件里面的而不是像之前的范例一样设置在*index.html*里面的。
这个模板将作为*tabs*组件的抽象状态进行工作。
同时，你也可以发现，*tab-dash.html*，*tab-chats.html*以及*tab-account.html*和我们上一个范例的架构方式是一样的。  
使用如下口令运行app进行测试：
```
ionic serve
```
你大概会看到，当你点击*chat*列表里面的一个条目的时候，会把你带到一个新的视图展示详细信息。
这种设置叫做主详情视图（master detail view），“master”是聊天列表，“detail”是聊天详情。
同时，注意看不同的聊天URL不一样，例如： http://localhost:8100/#/tab/chats/0和 http://localhost:8100/#/tab/chats/1 等等。
去*www/js/app.js*中查看*tab.chat-detail*的状态配置的时候，可以看到如下代码：
```
.state('tab.chat-detail', {
url: '/chats/:chatId',
views: {
'tab-chats': {
templateUrl: 'templates/chat-detail.html',
controller: 'ChatDetailCtrl'
}
}
})
```
*url*属性的值是*'/chats/:chatId'*。注意*chatId*前面的冒号。这里告诉路由器*chatId*是一个动态值；
当遇到这个路由的时候，在*chats*部分之后在验证路由，然后将URL里*chats*部分后面的值的存储到一个名为*chatId*的变量里。
现在，当我们实时处理我们的应用的时候，这个值将可以在*$stateParams*上得到。参考*www/js/controllers.js –ChatDetailCtrl*：
```
.controller('ChatDetailCtrl', function($scope, $stateParams, Chats) {
$scope.chat = Chats.get($stateParams.chatId);
})
```
以上代码向你展示如何对标签视图和一个主详情视图进行混合与匹配。  
你也可以试试搭建一个*sidemenu*模板然后看侧边菜单是如何配置路由的。  

## 总结
在本章中，我们学习了大部分的Ionic CSS 组件。我们也看过了可用的一些样色样本。接下来，我们整合Ionic CSS组件与AngularJS并添加了一些功能。
我们从0开始使用Ionic状态路由器进行工作，创建了一个简单的双页面app。最后，我们探索了标签页界面和主详情视图。  
在接下来的章节里，我们将会学习使用SCSS的强力来定制Ionic CSS。