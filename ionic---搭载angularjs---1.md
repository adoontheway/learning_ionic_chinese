## AngularJS 指令

引用自AngularJS文档：

> "At a high level, directives are markers on a DOM element (such as an attribute,
   element name, comment or CSS class) that tell AngularJS's HTML compiler
 ($compile) to attach a specifed behavior to that DOM element or even transform
 the DOM element and its children."
 
> 指令是一个高级别的DOM元素（例如一个属性，元素名，备注或者CSS类）的制作者，告诉AngularJS的HTML编译器（$compiler）给对应的DOM元素添加指定的行为或者转变DOM元素和他的子元素

当你想要在网页上抽离出公用功能的时候，这是一个非常有用的特性。AngularJ的指令已经达成了这些功能，例如：
* ng-app：在没有传入值的时候，这个指令初始化一个新的默认的AngularJS模组；反之，会初始化一个有名字的模组。
* ng-model： 将输入的元素的值映射到当前scope
* ng-show： 当传递给ng-show的表达式的值为true的时候，显示对应的DOM元素。
* ng-hide：当传给ng-hide的表达式的值为true的时候，隐藏对应的DOM元素。
* ng-repeat： 这条指令根据传入的表达式迭代当前标签以及他的子元素。

回想我们之前创建的Search App，想象一下应用中还有很多其他页面需要用到这个搜索表单。并且，他们期望的结果也差不多。  
此时，我们可以将这个表单功能抽离成一个自定义的指令，而不是去复制粘贴controller和HTML模板代码。

然后你就可以在DOM元素的属性里用到一条新的指令，例如<div my-search></div>，或者你可以创建你自己的标签/元件，例如<my-search></my-search>。

这样你只用写一遍搜索功能，然后就可以各处使用。AngularJS负责在进入view的时候初始化指令，在离开view的时候销毁指令。

我们将会为Search App创建一个名为*my-search*的新指令。
这条指令的唯一功能是渲染一个文本框和一个按钮。当点击**Search**按钮的时候，我们将会拿取结果然后将它们展示到搜索表单下面。

现在开工。

和其他AngularJS组件一样，所有的指令都绑定到一个模组。在咱们的案例中，我们已经有一个名叫searchApp的模组。我们将绑定一条新的指令到这个模组：

```
searchApp.directive('mySearch', [function () {
  return {
    template : 'This is Search template',
    restrict: 'E',
    link: function (scope, iElement, iAttrs) {

    }
  };
}]);
```

这条指令名为*mySearch*，命名方式的驼峰式。当在HTML中使用指条指令的时候，AngularJS将负责用*my-search*匹配这条指令。
我们将在*template*属性中设置一个范例文本。我们限制此指令用作为元素（E）类型。

> AngularJS里面其他可以限制的值是 A （attribute 属性，） C(class 类），以及M（comment 备注）。你也可以允许指令使用所有的4个格式（ACEM）。

我们创建了一个连接方法。每当指令进入到view的时候，这个方法都会被调用。这个方法有以下3个参数注入其中：

* scope： 这个参数规定了这个标签在DOM里面的范围。例如，他可以在AppCtrl里面，甚至直接在rootScope(ng-app)里面。
* iElement： 指令附加到的宿主元素
* iAttrs： 当前元素的所有属性。

在我们的指令中，我们不会使用iAttrs，因为我们的*my-search*标签上没有属性。
在复杂的指令当中，最好的做法是将你的指令模板抽离成另一个文件，然后在指令中使用*templateUrl*来引用他。
接下来，我们就使用这种做法。  
在*index.html*的同一目录下，新建一个文件名为*directive.html*，然后添加以下内容：
```
<form>
  <label>Search : </label>
  <input type="text" name="query" ng-model="query" required>;
  <input type="button" ng-disabled="!query" value="Search" ngclick="search()">
</form>
<div ng-repeat="res in results">
  <h2>{{res.heading}}</h2>
  <span>{{res.summary}}</span>
  <a ng-href="{{res.link}}">{{res.linkText}}</a>
</div>
```

就这么简单，我们在*index.html*使用这条指令替换相应的搜索代码。
现在，我们将会在指令内部给按钮注册一个*click*事件监听器。更新后的指令如下：

```
searchApp.directive('mySearch', [function() {
  return {
  templateUrl: './directive.html',
  restrict: 'E',
  link: function postLink(scope, iElement, iAttrs) {
    scope.search = function() {
      var q = {
        query : scope.query
      };

      //Interact with the factory (next step)
    }
  }
  };
}])
```

如上代码所示，当按钮的*click*事件触发之后执行了*scope.search*，*scope.query*返回了文本框的值。这跟我们在控制器里面做的差不多。

现在，当用户在输入一些文本然后点击**Search**按钮的时候，我们会调用*ResultFactory*里的*getResults*方法。然后，一旦结果返回，我们将会把他们绑定到scope的*results*属性上。
完整的指令代码如下：
```
searchApp.directive('mySearch', ['ResultsFactory',
function(ResultsFactory) {
  return {
    templateUrl: './directive.html',
    restrict: 'E',
    link: function postLink(scope, iElement, iAttrs) {
      scope.search = function() {
        var q = {
          query : scope.query
        };
        ResultsFactory.getResults(q).then(function(response){ 
          scope.results = response.data.results;
        });
      }
    }
  }
}]) 
```

有了这些，我们可以将index.html更新为：
```
<html ng-app="searchApp">
<head>
  <script src="angular.min.js" type="text/JavaScript"></script>
  <script src="app.js" type="text/JavaScript"></script>
</head>
<body>
  <my-search></my-search>
</body>
</html>
```
然后app.js更新为以下：
```
var searchApp = angular.module('searchApp', []);
searchApp.factory('ResultsFactory', ['$http', function($http){
    return {
      getResults : function(query){
      return $http.post('/getResults', query);
    }
  };
}]);
searchApp.directive('mySearch', ['ResultsFactory',function(ResultsFactory) {
  return {
    templateUrl: './directive.html',
    restrict: 'E',
    link: function postLink(scope, iElement, iAttrs) {
      scope.search = function() {
        var q = {
          query : scope.query
        };
        ResultsFactory.getResults(q).
          then(function(response){
            scope.results = response.data.results;
        });
      }
    }
  };
}]);
```  
异常简单，但是非常强大！  
现在，当你在哪里需要一个搜索条的时候，直接扔一个<my-search></my-search>就可以了。  
你也可以给这条指令进行升级：给他传入一个名为*results-target*的属性。这实际上是页面上的某元素的ID。
因此，你可以在这个提供的元素里面显示结果，而不是像之前那样在搜索条下面显示结果。

> AngularJS自带一个轻量版本的jQuery库名为jqLite。
jqLite不支持选择器查询。如果你想要用jQuery替代jqLite的话，那么需要在AngularJS之前添加jQuery。
更多关于jqLite的信息请参考：https://docs.angularjs.org/api/ng/function/angular.element  

在处理DOM元素的时候，这个特殊功能使得AngularJS指令成为组件重用的完美解决方案。

此时，当你需要给你的Ionic app添加一个新的导航条的时候，你只需要扔一个**ion-nav-bar**标签进去就可以了，如下：
```
<ion-nav-bar class="bar-positive">
  <ion-nav-back-button>
  </ion-nav-back-button>
</ion-nav-bar>
```  
然后，一切将会尘埃落定。  
当我们回首理解自定义指令的痛苦的时候，你将会很容易的的联想到所有由AngularJS指令所创建的Ionic组件。