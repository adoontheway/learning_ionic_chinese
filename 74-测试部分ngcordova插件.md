### $cordovaToast
第一个学习的插件是toast插件。这个插件弹出一个展示文本而不会阻断app的用户交互。  
新建一个空白模板项目：
```
ionic start -a "Example 30" -i app.example.thirty example30 blank
```
接下来，给项目添加ngCordova支持。想要使用toast API的话，那么得先将toast插件添加到项目里：
```
ionic plugin add https://github.com/EddyVerbruggen/Toast-PhoneGapPlugin.git
```
现在，我们给每个需要用到的插件分别创建一个控制器，而不是去*run*方法中处理。这样，当你回顾参考的时候就会比较简单。  
打开*www/index.html*在*body*标签上添加*ng-controller="ToastCtrl"*。然后在*www/js/app.js*中的*run*方法下面，添加控制器定义代码：
```
.controller('ToastCtrl', ['$ionicPlatform', '$cordovaToast',function($ionicPlatform, $cordovaToast) {
    $ionicPlatform.ready(function() {
        $cordovaToast
            .show('This is a long toast!', 'long', 'center')
            .then(function(success) {
            // success
            }, function(error) {
            // error
            });
    });
}])
```
接着，给Ionic App添加一个平台然后模拟之。效果图如下：  
![finally](imgs/chapter-7-11.png 'finally')

在本范例中，我们将使用仅可能小版本的API方法。在学完每个插件之后，我们提供他们的API链接；你可以查看他们支持的其他方法。
> 更多信息，请访问：http://ngcordova.com/docs/plugins/toast/

### $cordovaDialogs
接着学习的是对话框插件。他会触发警告，确认以及提醒窗。  
新建一个空白模板项目：
```
ionic start example31 blank
```
接下来，给项目添加ngCordova支持。然后使用以下命令给项目添加对话框插件：
```
ionic plugin add cordova-plugin-dialogs
```
我们将创建一个对话框控制器。打开*www/index.html*在*body*标签处添加*ng-controller="DialogsCtrl" *。  
本范例中，我们将显示一个提醒框供用户输入文本。用户在输入文本的时候，我们将文本输出到屏幕上。我们将*www/index.html*的body部分更新如下：
```
<body ng-app="starter" ng-controller="DialogsCtrl">
    <ion-pane>
        <ion-header-bar class="bar-stable">
            <h1 class="title">Ionic Blank Starter</h1>
        </ion-header-bar>
        <ion-content>
            <span class="padding">Hello {{name}}!!</span>
        </ion-content>
    </ion-pane>
</body>
```
在*www/js/app.js*中添加*DialogsCtrl*：
```
.controller('DialogsCtrl', ['$ionicPlatform', '$scope','$cordovaDialogs', function ($ionicPlatform, $scope,$cordovaDialogs) {
    $ionicPlatform.ready(function () {

        $cordovaDialogs.prompt('Name please?', 'Identity',    ['Cancel', 'OK'], 'Harry Potter')
            .then(function (result) {
                if (result.buttonIndex == 2) {
                    $scope.name = result.input1;
                }
            });
    });
}])
```
接下来给Ionic App添加一个平台然后进行模拟。效果图如下：  
![dialog](imgs/chapter-7-12.png 'dialog')

> 更多信息参考： http://ngcordova.com/docs/plugins/dialogs/

### $cordovaFlashlight
下一个学习的是一个工具插件。这个插件用户开关设备上的手电筒。这个插件不能在模拟器上测试。所以你得找个设备来测试这个插件。  
新建一个空白模板项目：
```
ionic start -a "Example 32" -i app.example.thirtytwo example32 blank
```
接下来，给项目添加ngCordova支持。运行如下命令添加手电筒插件到项目：
```
ionic plugin add https://github.com/EddyVerbruggen/FlashlightPhoneGap-Plugin.git
```
打开*www/index.html*，在*body*标签处添加*ng-controller="FlashlightCtrl"*。  
本例中，我们使用*ion-toggle*指令给用户展示一个开关，然后根据他的状态，对设备上的手电筒进行开与关操作。由此，更新*www/index.html*如下：
```
<body ng-app="starter" ng-controller="FlashlightCtrl">
    <ion-pane>
        <ion-header-bar class="bar-stable">
            <h1 class="title">Ionic Blank Starter</h1>
        </ion-header-bar>
        <ion-content>
            <ion-list>
                <ion-item>
                    <ion-toggle ng-disabled="notSupported" ng-model="torch" ng-change="toggleTorch()">
                    Torch
                    </ion-toggle>
                </ion-item>
            </ion-list>
        </ion-content>
    </ion-pane>
</body>
```
在*www/js/app.js*的*run*方法后面添加*FlashlightCtrl*：
```
.controller('FlashlightCtrl', ['$scope', '$ionicPlatform','$cordovaFlashlight', function($scope, $ionicPlatform,$cordovaFlashlight) {
    $scope.notSupported = true;

    $ionicPlatform.ready(function() {
        $cordovaFlashlight.available().then(function(availability)
        {
            // availability = true || false
            $scope.notSupported = !availability;
        });
        $scope.toggleTorch = function() {
            if ($scope.notSupported) return;

            $cordovaFlashlight.toggle()
                .then(function(success) { /* success */ },
                function(error) { /* error */ });
        }
    });
}])
```
我们先检查插件是否可用。如果可用，我们激活开关；否则，保持开关的禁用状态。  
然后在用户切换开关的时候，我们调用*toggleTorch*方法，这样就可以切换手电筒的开关状态了。  
如果你在设备上运行app的时候，将会看到如下效果：  
![flashlight](imgs/chapter-7-13.png 'flashlight')

如果想要验证开关是否会禁用，可以在模拟器中模拟此app。
> 更多信息参考：http://ngcordova.com/docs/plugins/flashlight/

### $cordovaLocalNotification
接下来学习的是Notification（通知）插件。这个插件主要是用来通知或者提醒用户App相关的活动。有时候，当后台运行一些活动的时候也会显示通知-例如，上传大文件的时候。  
新建一个空白模板App：
```
ionic start -a "Example 33" -i app.example.thirtythree example33 blank
```
接下来，给项目添加ngCordova支持。运行如下命令添加通知插件到项目：
```
ionic plugin add de.appplant.cordova.plugin.local-notification
```
在本例中，我们将在按下按钮的时候触发一个通知，然后展示用户在文本框中输入的文本。
因此，我们需要添加一个名为*NotifCtrl*的控制器和一个文本框，一个按钮。  
更新*www/index.html*的*body*部分为以下代码：
```
<body ng-app="starter" ng-controller="NotifCtrl">
    <ion-pane>
        <ion-header-bar class="bar-stable">
            <h1 class="title">Ionic Blank Starter</h1>
        </ion-header-bar>
        <ion-content>
            <div class="list">
                <label class="item item-input">
                    <span class="input-label">Enter Notification text</span>
                    <input type="text" ng-model="notifText">
                </label>
                <label class="item item-input">
                    <button class="button button-dark" ng-click="triggerNotification()">
                    Notify
                    </button>
                </label>
            </div>
        </ion-content>
    </ion-pane>
</body>
```
在*www/js/app.js*中添加*NotifCtrl*如下：
```
.controller('NotifCtrl', ['$scope', '$ionicPlatform','$cordovaLocalNotification', function($scope, $ionicPlatform,$cordovaLocalNotification) {
    $ionicPlatform.ready(function() {
        $scope.notifText = 'Hello World!';
        $scope.triggerNotification = function() {
            $cordovaLocalNotification.schedule({
                id: 1,
                title: 'Dynamic Notification',
                text: $scope.notifText
            }).then(function(result) {
                console.log(result);
            });
        }
    });
}])
```
模拟此Ionic app的时候，会问你是否允许显示通知。允许之后，你可以根据需求分发通知。  
![notification](imgs/chapter-7-14.png 'notification')

> 更多信息参考： http://ngcordova.com/docs/plugins/localNotification/

### $cordovaGeolocation
最后一个学习的插件的Geolocation（定位）插件，这个插件可以帮助我们获取设备坐标。  
新建一个空白模板项目：
```
ionic start -a "Example 34" -i app.example.thirtyfour example34 blank
```
接下来，给项目添加ngCordova支持。运行如下命令添加定位插件到项目：
```
ionic plugin add cordova-plugin-geolocation
```
启动应用的时候，我们将去获取设备的定位信息。在得到定位信息之前，我们展示一个加载内容信息。
一旦我们得到响应，我们就在页面上显示经度，纬度，以及精确度。  
首先，更新*www/index.html*的*body*部分如下：
```
<body ng-app="starter" ng-controller="GeoCtrl">
    <ion-pane>
        <ion-header-bar class="bar-stable">
            <h1 class="title">Ionic Blank Starter</h1>
        </ion-header-bar>
        <ion-content>
            <ul class="list" ng-show="dataReceived">
                <li class="item">
                    Latitude : {{latitude}}
                </li>
                <li class="item">
                    Longitude : {{longitude}}
                </li>
                <li class="item">
                    Accuracy : {{accuracy}}
                </li>
            </ul>
        </ion-content>
    </ion-pane>
</body>
```
之后，在*www/js/app.js*的*run*方法下面添加*GeoCtr*：
```
.controller('GeoCtrl', ['$scope', '$ionicPlatform','$cordovaGeolocation', '$ionicLoading', '$timeout', function($scope, $ionicPlatform, $cordovaGeolocation, $ionicLoading, $timeout) {
    $ionicPlatform.ready(function() {

        $scope.modal = $ionicLoading.show({
            content: 'Fetching Current Location...',
            showBackdrop: false
        });

        var posOptions = {
            timeout: 10000,
            enableHighAccuracy: false
        };

        $cordovaGeolocation
            .getCurrentPosition(posOptions)
            .then(function(position) {
            $scope.latitude = position.coords.latitude;
            $scope.longitude = position.coords.longitude;
            $scope.accuracy = position.coords.accuracy;
            $scope.dataReceived = true;
            $scope.modal.hide();
        }, function(err) {
            // error
            $scope.modal.hide();
            $scope.modal = $ionicLoading.show({
                content: 'Oops!! ' + err,
                showBackdrop: false
            });
            $timeout(function() {
                $scope.modal.hide();
            }, 3000);
        });
    });
}])
```
在模拟此app的时候，你将被询问是否允许访问定位信息。接受之后，就可以看到下面这样的效果：  
![geolocation](imgs/chapter-7-15.png 'geolocation')

> 更多信息参考： http://ngcordova.com/docs/plugins/geolocation/


以上范例应该给你很好的演示了如何使用ngCordova。

> 也可以查看其他关于ngCordova其他一些插件的帖子： http://thejackalofjavascript.com/getting-started-withngcordova
完整的插件列表： http://ngcordova.com/docs/plugins/
当在使用ngCordova的时候，只添加你需要用到的插件。
参考此帖自定义ngCordova： http://ngcordova.com/build/
记住，在自定义ngCordova之后，就不能用bower下载ngCordova了。

# 总结
本章中，我们看了什么是Cordova插件，以及他们在Ionic应用中是如何使用的。刚开始，我们为Android和iOS设置了本地开发环境。
然后学习了如何模拟和运行app。接下来，我们探索了如何给Ionic项目添加Cordova插件以及如何使用他们。
最后，在ngCordova的帮助下，我们将插件作为依赖注入我们的Ionic/Angular app中，然后以Angular的方式使用他们。  
在下一章中，我们将使用Ionic， ngCordova以及Firebase制作另一个app。  
我们将要制作的应用是一个聊天app，用户可以登录其中，查看在线用户。用户可以选中其他用户交换文本，图片以及地理详情等信息。  
聊天app的目的是整合Ionic和一个实时数据存储，例如Firebase，同时通过访问设备功能使通讯内容更丰富。
