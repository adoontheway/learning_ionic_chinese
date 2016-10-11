> 词汇求助：
* popover:
* pin dialog:


## ionicPopup
接下来学习的服务是*$ionicPopup*。这个服务用来创建一个弹出框以供用户响应来确定是否继续。  
这些弹出框都是JavaScript本身的*alert*，*prompt*和*confirm*方法的自定义样式。  
新建一个空白模板项目进行测试：
```
ionic start -a "Example 24" -i app.example.twentyfour example24 blank
```
通过*cd*命令进入到*example24*文件夹运行如下命令：
```
ionic serve
```
然后浏览器中将会运行此项目。  
我们将会实现展示，确认和警告方法的app样式。  
我们将使用*show*方法显示一个pin对话框，用户需要在这个对话框中输入pin。如果pin有效，我们就给用户展示一个我们代码的安全区域。
安全区域有一些展示确认框和警告框的按钮。  
如果用户直接退出了pin对话框，我们将把用户带到一个不安全区域，然后重新询问用户一遍。  
此范例使用AngularJS和Ionic介绍了一个根据条件显示内容的途径。  
创建一个*AppCtrl*控制器作为开始。在*www/js/app.js*中，添加以下代码：
```
.controller('AppCtrl', function($scope, $ionicPopup) {
    $scope.data = {};
    $scope.state = {};
    $scope.error = {};
    $scope.prompt = function() {
        // reset app states
        $scope.state.cancel = false;
        $scope.state.success = false;
        // reset error messages
        $scope.error.empty = false;
        $scope.error.invalid = false;
        var prompt = $ionicPopup.show({
            templateUrl: 'pin-template.html',
            title: 'Enter Pin to continue',
            scope: $scope,
            buttons: [{
                text: 'Cancel',
                onTap: function(e) {
                    $scope.state.cancel = true;
                }
            }, {
            text: '<b>Login</b>',
            type: 'button-assertive',
            onTap: function(e) {
                $scope.error.empty = false;
                $scope.error.invalid = false;
                if (!$scope.data.pin) {
                    // disable close if the
                    // user does not enter
                    // a valid pin
                    $scope.error.empty = true;
                    e.preventDefault();
                } else {
                    if ($scope.data.pin === '1234') {
                        $scope.state.success = true;
                        return $scope.data.pin;
                    } else {
                        $scope.error.invalid = true;
                        e.preventDefault();
                    }
                }
            }
            }]
        });
    };
    $scope.confirm = function() {
        var confirm = $ionicPopup.confirm({
            title: 'Confirm Popup Heading',
            template: 'Are you sure you want to do that?'
        });
        confirm.then(function(res) {
            if (res) {
                console.log('Yes!');
            } else {
                console.log('Nooooo!!');
            }
        });
    };
    $scope.alert = function() {
        var alert = $ionicPopup.alert({
            title: 'You are secured!',
            template: 'You are inside a secure area!'
        });
        alert.then(function(res) {
            console.log('Yeah!! I know!!');
        });
    };
    // invoke the prompt on controller init.
    $scope.prompt();
})
```
代码好多啊！！（作者原话）但是也很简单。（作者原话）好长，懒得看，后续跑代码的时候再看。（译者原话）  
我们在scope上创建了3个对象，分别名为*data*，*state*，和*error*。这些对象将用来存储数据，应用状态和错误信息。  
我们添加了一个*prompt*方法。在这个方法里面，我们调用了*$ionicPopup.show*方法，传入模板和取消按钮以及**Login**按钮的点击方法的定义。
当用户点击了prompt的**Cancel**按钮的时候，我们将状态对象的*cancel*属性设置为*true*。这个属性将用作切换视图。  
当用户点击**Login**按钮的时候，我们将会检查是否输入了有效的pin。如果没有的话，我们会将对于的错误信息设置为*true*。
如果用户输入了有效的pin并且等于*1234*的话，我们会把状态对象上的success属性设置为*true*。这样将会切换到另一个视图，这个视图上将会有**Confirm**和**Alert**按钮。  
*confirm*和*alert*方法都是自解释性的。他们分别使用*$ionicPopup.confirm*和*$ionicPopup.alert*进行设置。
这些方法返回一个promise，当在点击按钮的时候，就可以解析了。  
最后，我们在控制器加载的时候调用*prompt*方法来显示*Pin*对话框。  
*www/index.html*的body部分更新如下：
```
<body ng-app="starter" ng-controller="AppCtrl">
<ion-pane ng-cloak>
    <ion-header-bar class="bar-positive">
        <h1 class="title">Super Secure App</h1>
    </ion-header-bar>
<ion-content class="padding">
    <div class="card" ng-show="state.cancel">
        <div class="item item-divider">
        Oops!! you cancelled!
        </div>
        <div class="item item-text-wrap">
        To see the secure content enter pin
            <button class="button button-assertive buttonblock" ng-click="prompt()">
            Try Again!
            </button>
        </div>
    </div>
    <div class="card" ng-show="state.success">
        <div class="item item-divider">
        You are viewing secure content!
        </div>
        <div class="item item-text-wrap">
            <button class="button button-positive buttonblock" ng-click="confirm()">
            Show Confirm Dialog
            </button>
            <button class="button button-positive buttonblock" ng-click="alert()">
            Show Alert Dialog
            </button>
        </div>
    </div>
    </ion-content>
</ion-pane>
<script type="text/ng-template" id="pin-template.html">
    <input type="password" ng-model="data.pin">
    <label ng-show="error.empty" class="assertive text-center
    block padding">Please enter a valid Pin</label>
    <label ng-show="error.invalid" class="assertive textcenter block padding">Invalid Pin, Try Again!</label>
</script>
</body>
```
我们添加了两个卡片视图；一个在*state.cancel*为*true*的时候显示，另一个在*state.success*为*true*的时候显示。在body标签的最后，我们添加了*pin-template.html*模板。  
重点关注一下我们给*ion-panel*指令添加的*ng-cloak*属性。这个属性确保了AngularJS处理完成之前不显示任何内容。
> 更多信息关于*ng-cloak*请参考：https://docs.AngularJS.org/api/ng/directive/ngCloak

保存所有文件，返回浏览器，将会看到：  
![run](imgs/chapter-5-20.png 'run')
  
在不输入任何数据的情况下点击**Login**，将会显示：  
![run](imgs/chapter-5-21.png 'run')
  
在输入一个无效的pin的时候，将会看到：  
![run](imgs/chapter-5-22.png 'run')
  
取消弹出框的时候，会把你带到一个不安全的区域，这里更你另一次做人的机会：  
![run](imgs/chapter-5-23.png 'run')
  
终于，在你输入了有效的pin值之后，你会被带到一个安全区域，其中可以看到**Confirm**和**Alert**按钮。点击他们之后会看到：  
![run](imgs/chapter-5-24.png 'run')
  
上面的代码不仅讨论了*$ionicPopup*服务，也让你知道了如何搭建自己的应用。  

### ion-list和ion-item指令
鉴于咱们是在熟悉大部分的Ionic指令和服务，我想应该值得提一下*ion-list*和*ion-item*指令。  
在移动应用中，列表是使用最广泛的显示模式之一。在Ionic中，我们可以像在*第三章 Ionic CSS组件和导航*中那样使用CSS版本的列表，或者使用指令版的列表。  
使用指令版的列表的好处是他提供了很多额外属性用来更好的管理列表，例如：*ion-delete-button*, *ion-reorder-button*以及*ion-option-button*。  
新建一个空白模板项目进行测试:
```
ionic start -a "Example 25" -i app.example.twentyfive example25 blank
```
老规矩：
```
ionic serve
```
然后浏览器就运行了这个项目了。  
我们将实现一个Ionic文档里面提供的范例，涵盖上面提到的所有指令。  
> 接下来的范例来源于这里，不同的是我们改为通过工厂添加数据的： http://codepen.io/ionic/pen/JsHjf

首先，我们创建一个工厂用来分发随机的数据给列表。工厂和我们在*example17*里面使用的那个差不多，除了返回的数据有所不同：
```
.factory('DataFactory', function($timeout, $q) {
    var API = {
        getData: function(count) {
            // Spoof a network call using promises
            var deferred = $q.defer();
            var data = [],
            _o = {};
            count = count || 20;

            for (var i = 0; i < count; i++) {
                _o = {
                    // http://stackoverflow.com/a/8084248/1015046
                    id: i + 1,
                    title: (Math.random() + 1).toString(36).substring(7)
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
此处返回了一个带有*id*和*title*属性的对象。  
接下来会在*www/js/app.js*创建一个名为*AppCtrl*的控制器。他将负责从工厂拿取数据以及建立列表。同时，我们将为**Edit**, **Delete**以及**Option**按钮提供点击处理函数：
```
.controller('AppCtrl', function($scope, DataFactory) {
    $scope.items = [];
    $scope.data = {
        showDelete: false
    };

    $scope.edit = function(item) {
        alert('Edit Item: ' + item.id);
    };

    $scope.share = function(item) {
        alert('Share Item: ' + item.id);
    };

    $scope.moveItem = function(item, fromIndex, toIndex) {
        $scope.items.splice(fromIndex, 1);
        $scope.items.splice(toIndex, 0, item);
    };

    $scope.onItemDelete = function(item) {
        $scope.items.splice($scope.items.indexOf(item), 1);
    };

    // get data on page load
    DataFactory.getData().then(function(data) {
     $scope.items = data;
    });
})
```
接着，修改*www/index.html*的body部分：
```
<body ng-app="starter" ng-controller="AppCtrl">
<!-- http://codepen.io/ionic/pen/JsHjf -->
<ion-header-bar class="bar-positive">
<div class="buttons">
<button class="button button-icon icon ion-ios-minusoutline" ng-click="data.showDelete = !data.showDelete; data.showReorder = false"></button>
</div>
<h1 class="title">Ionic Lists</h1>
<div class="buttons">
<button class="button" ng-click="data.showDelete =
false; data.showReorder = !data.showReorder">
Reorder
</button>
</div>
</ion-header-bar>
<ion-content>
<ion-list show-delete="data.showDelete" showreorder="data.showReorder">
<ion-item ng-repeat="item in items" item="item"
class="item-remove-animate">
{{ item.id }}. {{ item.title }}
<ion-delete-button class="ion-minus-circled" ngclick="onItemDelete(item)">
</ion-delete-button>
<ion-option-button class="button-assertive" ngclick="edit(item)">
Edit
</ion-option-button>
<ion-option-button class="button-calm" ngclick="share(item)">
Share
</ion-option-button>
<ion-reorder-button class="ion-navicon" onreorder="moveItem(item, $fromIndex, $toIndex)"></ion-reorderbutton>
</ion-item>
</ion-list>
</ion-content>
</body>
```
页头里有两个两个按钮用来切换列表上的删除和重排图标。在*ion-list*指令中，我们使用了*show-delete*和*show-reorder*属性来显示和隐藏列表条目上的图标。  
在每个*ion-item*指令中，我们添加了*ion-delete-button*来调用*onItemDelete*函数；添加了*ion-option-button*来显示**Share**和**Edit**按钮；
最后添加了*ion-reorfer-button*来显示重排图标。重排的时候，我们将调用*moveItem*方法。  
保存所有文件，返回浏览器，可以看到：  
![run](imgs/chapter-5-25.png 'run')
  
点击页头的删除图标的时候：  
![run](imgs/chapter-5-26.png 'run')
  
可以通过点击条目左边的删除图标来删除他。点击页头的重排按钮的时候：  
![run](imgs/chapter-5-27.png 'run')
  
可以使用每行提供的操作随便移着玩。你也可以关闭重排，擦掉条目的左边显区域，显示**Share**和**Edit**选项。
![run](imgs/chapter-5-28.png 'run')
> 在使用*collection-repeat*替代*ng-repeat*的情况下，重排将会有问题。
请参考： https://github.com/driftyco/ionic/issues/1714
更多列表信息，请参考： http://ionicframework.com/docs/api/directive/ionList/

## 手势指令和服务
接下来的指令和服务都是关于手势的。手势是用户在与应用交互是在屏幕上的操作行为。一个简单的例子就是，在屏幕上进行捏的手势的时候缩小，拉的时候进行放大。  
Ionic通过*$ionicGesture*支持这些手势。  
为了使讲解更通用，我将解释一个手势指令然后给你展示如何使用他。此逻辑可以应用与所有其他手势。  
新建一个空白模板项目来测试*on-drag-up*指令：
```
ionic start -a "Example 26" -i app.example.twentysix example26 blank
```
使用*cd*口令进入*example26*文件夹运行如下命令：
```
ionic serve
```
之后，项目将会在浏览器中运行。  
首先，在*www/js/app.js*中创建一个名为*AppCtrl*的控制器：
```
.controller('AppCtrl', function($scope, $ionicGesture) {
    $scope.scopeGesture = 'None';
    $scope.delegateGesture = 'None';

    $scope.onDragUp = function() {
        $scope.scopeGesture = 'Drag up fired!'
    };

    // Event listener using event delegation
    // The logic below would be typically written in a directive
    // We have added this to the controller for illustration
    purposes
    var $element =  angular.element(document.querySelector('#gestureContainer'));
    $ionicGesture.on('dragup', function() {
        $scope.delegateGesture = 'Drag up fired!';
    }, $element);
})
```
我们之前说过，我们要实现*dragup*手势。我们用了两种方法实现了，一个是使用指令，这个方法可以通过HTML模板看到，另一个是使用*$ionicGesture.on*方法实现的。  
通常任何与DOM相关的代码都将写成一个（自定义的）指令。在这里，我们将他写在控制器里面以阐明目的。事件类型是*dragup*。我们使用*document.querySelector*API来取得DOM元素，
然将此元素封装到一个AngularJS元素对象中。这个对象将作为*$ionicGesture.on*方法的第三个传入参数。  
接下来，更改*www/index.html*的body部分：
```
<body ng-app="starter" ng-controller="AppCtrl">
    <ion-header-bar class="bar-dark">
        <h1 class="title">Gestures</h1>
    </ion-header-bar>
    <ion-content>
        <div class="card">
            <div id="gestureContainer" class="item text-center" on-drag-up="onDragUp()">
            Drag me up!!
            </div>
        </div>
        <div class="card">
            <div class="item text-center">
            Scope Gesture : {{scopeGesture}}
            <br>
            Delegate Gesture : {{delegateGesture}}
            </div>
        </div>
    </ion-content>
</body>
```
我们创建了两个卡片容器；第一个包含了一个ID名为*gestureContainer*的*div*，他的*on-drag-up*属性将在事件触发的时候执行*onDragUp*方法。  
第二个荣是是由*scopeGesture*的值组成的，这些值将会在*onDragUp*方法执行的时候进行更新。*delegateGesture*将在*$ionicGesture.on*方法发出*dragup*事件的时候进行设置。  
保存所有文件，返回浏览器，可以看到这两个卡片部分。当你拖动第一个卡片内容的时候，第二个卡片的显示内容将会更新，如下：  
![run](imgs/chapter-5-29.png 'run')
  
管理手势非常简单！下面表格显示的是可用于*$ionicGesture.on*方法中的手势指令以及他的事件类型。可以用上述代码来实现以下任何一个手势：  
![run](imgs/chapter-5-30.png 'run')
> 也可以使用*ionic.EventController*工具来实现手势操作。请参考：http://ionicframework.com/docs/api/utility/ionic.EventController/#onGesture
默认Ionic会移除浏览器添加的300ms点击延迟。浏览器添加这个延迟是用来区别单击与双击的。如果想要给某元素应用这个300ms的延迟，那么请使用*data-tap-disabled*属性。
更多信息，参考： http://ionicframework.com/docs/api/page/tap/