## AngularFire
鉴于Ionic使用AngularJS作为他的客户端JavaScript框架，我们将使用一个Firebase向的AngularFire以Angular的方式来与Firebase交互。  
我们将快速浏览一遍AngularFire，如下：
```
var app = angular.module("nameApp", ["firebase"]);
app.controller("NamesCtrl", function($scope, $firebaseArray) {
    var ref = new Firebase("https://<YOUR-FIREBASEAPP>.firebaseio.com/names");
    // create a synchronized array
    $scope.names = $firebaseArray(ref);

    $scope.addName = function() {
        $scope.names.$add({
            text: $scope.newName
        });
        };
});
```
首先我们将新建一个AngularJS应用，然后添加了firebase作为依赖。然后我们创建了一个控制器，注入*$firebaseArray*作为依赖。
一旦调用控制器的时候，我们将创建一个Firebase App的引用。此时，我们将使用name创建一个子集或者内置集然后保存，而不是将他直接存放到根基和。  
将*$firebaseArray(ref)* 指派给 *$scope.names* 就可以将他变为同步集合了。简单来说，如果数据存储里面的数据变更的时候，我们的scope变量将会自动同步更新，同时触发视图或者模板里面的更新。
这种现象也称为三方数据（Three-Way Data）绑定。
> 更多关于三方数据绑定，请参考： https://www.firebase.com/blog/2013-10-04-firebase-angular-databinding.html
如果想要上面的代码正常执行，需要在你的页面上导入Firebase，AngularJS，以及AngularFire脚本文件。

我们将实现快速实现一个范例来展示AngularFire是如何工作的。新建一个文件夹名为*example36*，在其中新建一个文件名为*index.html*，在其中加入代码如下：
```
<!DOCTYPE html>
<html>
<head>
    <title>AngularFire Test Page</title>
    <script src="https://cdn.firebase.com/js/client/2.2.2/firebase.js">
    </script>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.3.15/angular.min.js"></script>
    <script src="https://cdn.firebase.com/libs/angularfire/1.1.1/angularfire.min.js"></script>
</head>
    <body ng-app="NamesApp" ng-controller="NamesCtrl">
        <input type="button" ng-click="addNewName()" value="Add New Name">
        <br>
        <ul>
            <li ng-repeat="n in names">
            Name : <b> {{n.name}} </b>
            </li>
        </ul>
        <script type="text/javascript">
        var app = angular.module("NamesApp", ["firebase"]);
        app.controller("NamesCtrl", function($scope, $firebaseArray) {
            var ref = new Firebase("https://<YOUR-FIREBASE-APP>.firebaseio.com/names");

            // create a synchronized array
            $scope.names = $firebaseArray(ref);

            $scope.addNewName = function() {
                var name = prompt('Enter Name');
                if (name) {
                    $scope.names.$add({
                        name: name
                    });
                };
            }
        });
        </script>
    </body>
</html>
```
以上范例中，我们引入了Firebase，AngularJS以及AngularFire的源文件。
> 一定要记住在Firebase和AngularJS之后加载AngularFire。

我们新建了一个模组名为*NamesApp*，添加了一个新的控制器名为*NamesCtrl*。我们的HTML由一个重复scope里面的names数组的ng-repeat组成。names变量是一个同步数组。  
当用户点击**Add New Name**按钮的时候，我们给用户提醒他在何处输入一个名字。一旦名字输入完成，我们将使用我们的名字同步数组的*$add*方法将新对象推送到数据存储。
然后Firebase负责在数据之间进行同步。  
现在，当你打开 *https://<your-firebase-url>.firebaseio.com* 你将看到：  
![updated Firebase](imgs/chapter-8-1.png 'updated Firebase')

这次，数据将被添加到一个子对象或者内置对象的names属性上了。
> 在运行范例之前，我已经删掉了旧数据。如果想利用Firebase制作一个**Create**，**Read**，**Update**和**Delete**（CRUD，增删改查）的应用的话，参考：
http://thejackalofjavascript.com/getting-started-withfirebase/
