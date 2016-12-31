> 用到的名词：


## 整合Ionic CSS组件与AngularJS
如果你有一些漂亮的页面，里面有很些很酷的组件，但是他们在实时环境中什么都不做，在这种情况下，我们需要的是什么？
所以在这个子标题中，我们将要看一下整合这些美丽的Ionic组件与AngularJS以使得我们的页面更加功能化。  
第一个例子用来处理的是，在表单域全部有效之前禁用表单提交。我们将要创建一个由邮件地址和密码组成的表单。
在用户输入的邮件和密码的长度最少为3之前，我们将禁用登录按钮。  
我们先通过以下命令搭建一个空白模板的的app：  
```
ionic start -a "Example 7" -i app.example.seven example7 blank
```
接下里，我们将要对*index.html*进行更改，给他添加一个表单和一个名为*ng-disabled*的按钮。
当邮件的模型值和密码的模型值都是*false*的时候*ng-disabled*的值为*true*。  
> 关于JavaScript中真(truthy)与假(falsy)的概念，请参考：
http://adripofjavascript.com/blog/drips/truthyand-falsy-values-in-javascript.html
  
*www/index.html*文件里面相关代码应该是这样的：
```
<div class="list">
<label class="item item-input">
<span class="input-label">Email</span>
<input type="email" ng-model="email">
</label>
<label class="item item-input">
<span class="input-label">Password</span>
<input type="password" ng-model="password" ng-minlength="3">
</label>
<div class="padding">
<button ng-disabled="!email || !password" class="button button-block button-positive">Sign In</button>
</div>
</div>
```
保存文件，然后运行以下命令：
```
ionic serve
```
如果表单里面没有输入，或者输入了无效的数据，按钮将被禁用，大概是这样子的：  
![invalid form](imgs/chapter-3-21.png 'invalid form')
  
如果表单输入有效，按钮将会被激活：  
![valid form](imgs/chapter-3-22.png 'valid form')
  
这个简单的示例展示了AngularJS和Ionic如何一起工作来创建一个伟大的用户体验的。
上面的范例可以扩展显示有效信息。  
接下来的范例中，我们将处理一个稍微复杂一些的Ionic和AngularJS的整合。我们将实现一个简单的打分小部件。
这个小部件是由5个镂空的小星星组成的。当用户点击其中任何一个小星星来打分的时候，我们将会对从开始的那个星星到用户点击的那个星星进行填充。  
运行以下指令以创建一个新的空白模板项目：
```
ionic start -a "Example 8" -i app.example.eight example8 blank
```
接下来，在*www/js/app.js*中添加以下控制器代码：
```
.controller('MainCtrl', ['$scope', function($scope) {
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
}])
```
如上代码所示，我们创建了一个名为*ratingArr*的数组。这个数组元素由两部分组成，星星的值和需要应用到星星上的样式。
我们也创建了另一个名为*setRating()*的方法，这个方法将在星星被点击的时候进行调用。这个方法读取作为参数传入的星星的值。
然后，我们遍历从开始的星星到选中的星星的所有的打分对象的，我们将图标设置为填满的星星，其他的作为轮廓。  
*www/index.html*的body部分将是如下：
```
<body ng-app="starter" ng-controller="MainCtrl">
<ion-pane>
<ion-header-bar class="bar-positive">
<h1 class="title">Ionic Blank Starter</h1>
</ion-header-bar>
<ion-content class="padding">
<div class="padding text-center">
<h3>Rate the App</h3>
<div>
<a href="javascript:" ng-repeat="r in  ratingArr" class="padding" style="text-decoration:none;">
<i class="icon {{r.icon}}" ng-click="setRating(r.value)"></i>
</a>
</div>
</div>
</ion-content>
</ion-pane>
</body>
```
我们给*body*标签添加了*ng-controller*指令，在*ion-content*指令里面，我们有添加了*div*用来在遍历*ratingArr*的时候渲染星星。  
保存完所有文件后，运行:
```
ionic serve
```
你可以看到如下：  
![valid form](imgs/chapter-3-23.png 'valid form')
  
当你选择了第三个星星的时候，显示效果差不多是这样子的：  
![valid form](imgs/chapter-3-24.png 'valid form')
   
完成这些之后，我们已经简单理解了如何整合Ionic CSS组件和AngularJS。下一个主题中，我们将学习AngularUI路由器。