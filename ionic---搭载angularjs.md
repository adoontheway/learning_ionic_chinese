## 关注点分离

尽管服务端应用发展这么多年，Web演变至今，客户端驱动应用已经逐步超过服务端驱动应用。想当年，服务端告诉客户端该去干什么，客户端就乖乖的干什么。

随着异步调用与高交互网页的快速发展，使用客户端驱动要比服务端驱动能带来更好的用户体验。例如jQuery和Zepto这样的第三方库可以轻松帮助我们达到这个目标。

举一个经典的例子为证：用户在文本框里面输入数据，然后点击一个*Submit*按钮。之后这些数据将会经过*AJAX* **post**到服务端，无需刷新页面，UI使用服务端响应数据局部更新即可。

如果使用 *jQuery*（伪代码）的话，代码如下：

```
var textBox = $('#textbox');
var subBtn = $('#submitBtn');
subBtn.on('click', function(e) {
    e.preventDefault();
    var value = textbox.val().trim();
    if (!value) {
        alert('Please enter a value');
    return;
}

// 调用AJAX请求以获取服务端数据
var html2Render = '';
$.post('/getResults', {
    query: value
})
.done(functio\(data) {
    // 处理返回结果
    var results = data.results;
    for (var i = 0; i < results.length; i++) {
        // 为每条结果数据生成一个html元素标签
        var res = results[i];
        html2Render += ' < div class = "result" > ';
        html2Render += ' < h2  > ' + res.heading + ' < /h2 >';
        html2Render += ' < span > ' + res.summary + ' < /span >';
        html2Render += ' < a href = "' + res.link + '" < ' + res.linkText + ' < /a >';
        html2Render += ' < /div >'
    }

    // 将整个结果的html元素添加到页面上的容器里面
    $('#resultsContainer').html(html2Render);

    });
});

```

> 以上代码不可执行，仅供参考之用。  

当按钮被点击的时候，文本框的值将被post到服务端。然后，前端将会根据服务端返回的结果（json对象）生成一个html标记，注入到结果容器里面。

但是，以上代码的可维护性如何？

怎样才能测试其中单独的代码片段呢？比如，测试数据的验证工作是否正常或者服务端响应是否正常。

假设我们要对结果页面进行更改（例如在每个结果页面之间添加一个favicon作为内联图标），那么在之前的代码里面导入这个变更的难易度又是怎样的呢？

这就是一个分离的关注点。这些分离点都是存在于数据验证，进行AJAX请求和构建html标记之间的。他们之间都是高度耦合的，如果其中一个break掉了，所有其他的都会break，上述所有代码就都无法重用了。

如果我们把上述代码分离成不同的组件，那么到最后我们将会得到一个**Model View Controller（MVC）**架构。在典型的MVC架构中，model是存放数据的地方，controller是在把数据展示到view之前对数据进行处理（原作：massage 马杀鸡，按摩）的地方。

与服务端MVC不同的是，客户端MVC有一个额外的组件名为router（路由器）。路由器一般是页面的URL，用来指示需要加载哪个model/view/controller。

这就是AngularJS背后的基本理念，也是他如何在提供单页面应用架构的同时分离了关注点。

在前面的示例中，服务端交互层（AJAX）将会从主代码中分离出来，然后根据需求与某个controller进行交互。

在理解了这些之后，我们现在将要快速学习几个AngularJS关键组件。

## AngularJS 组件
与大部分客户端JavaScript框架不同的是，AngularJS是从HTML驱动的。
在一个标准的网页中， AngularJS负责在各个关键代码片之间进行连接。
所以，当你想要为你的HTML页面添加一些AngularJS指令并且包含AngularJS的时候，你可以不用写一行代码就能够轻松加愉快的搞定一个简单的app。

为了证明我不是吹牛，我们将构建一个带验证功能的没有一行JavaScript代码的登录表单：
```
<html ng-app="">
<head>
    <script src="angular.min.js" type="text/JavaScript"></script>
<head>
    <body>
        <h1>Login Form</h1>
        <form name="form" method="POST" action="/authenticate">
            <label>Email Address</label>
            <input type="email" name="email" ng-model="email" required>
            <label>Password</label>
            <input type="password" name="password" ng-model="password" required>
            <input type="submit" ng-disabled="!email || !password"  value="Login">
        </form>
    </body>
</html>
```

上面的的代码片段中，以 *ng-* 开头的属性叫做AngularJS指令。

在这里， *ng-disable* 指令的职责是当输入的e-mail和password的值无效时启用**Submit**的disabled属性。

同时，指令的范围被安全的限制到了他声明的元件以及他的子元件。
这里解决了JavaScript语言的另一个关键性问题：如果没有正确的声明一个变量，那么他就会被附加到Global Object，也就是Window Object。

> 如果你对范围（scope，范围或者作用域）一无所知，那么建议你先过一遍 https://docs.angularjs.org/guide/scope 。如果你对scope和root scope没有正确的认识的话，那么本书学起来将会非常的难。

现在，我们将会进行到下一个AngularJS组件，名为**Dependency Injection（DI，依赖注入）**。DI负责在有需求的的地方注入相应的代码片。这也是关注点分离的关键促成点之一。

你可以根据需求注入不同的AngularJS组件。例如，你可以在controller里面注入一个service。

> DI是AngularJS中另一个你必须了解的核心组件。关于DI的具体信息，可以查看：https://docs.angularjs.org/guide/di

为了便于理解service和controller，我们回退一步。在一个典型的客户端MVC框架中，我们知道model存放数据，view展示数据，controller处理model里面的数据并展示到view。

在AngularJS中，你可以这么理解上面的信息：
* HTML - Views
* AngularJS 控制器 - Controllers
* Scope 对象 - Model

在AngularJS中，HTML充当模块媒介的角色。AngularJS控制器从scope对象中或者服务的响应数据中取得数据，然后将它们组合成最终的view展示到网页上。
这和我们在搜索范例中做的非常类似，我们通过遍历结果，构建HTML字符串，然后将他注入到DOM中。
在这里，你可以看到，我们将功能分离到不同的组件中去了。
重申一下，HTML页面表演的角色是模板，factory组件负责发起AJAX请求。
最终，controller负责将factory的数据传递到view，view会生成实际UI。

AngularJS版本的搜索范例如下。

*index.html* 应该是这样的：
```
<html ng-app="searchApp">
    <head>
        <script src="angular.min.js" type="text/JavaScript">
        <script src="app.js" type="text/JavaScript">
    </head>
    <body ng-controller="AppCtrl">
        <h1>Search Page</h1>
        <form>
            <label>Search : </label>
            <input type="text" name="query" ng-model="query" required>
            <input type="button" ng-disabled="!query" value="Search" ng-click="search()">
        </form>
        <div ng-repeat="res in results">
        <h2>{{res.heading}}</h2>
        <span>{{res.summary}}</span>
        <a ng-href="{{res.link}}">{{res.linkText}}</a>
        </div>
    </body>
</html>
```
*app.js*是这样的：
```
var searchApp = angular.module('searchApp', []);
searchApp.factory('ResultsFactory', ['$http', function($http) {
    return {
        getResults : function(query){
            return $http.post('/getResults', query);
        }
    };
}]);

searchApp.controller('AppCtrl', ['$scope','ResultsFactory',function($scope, ResultsFactory) {
    $scope.query = '';
    $scope.results = [];
    $scope.search = function(){
        var q = {   
            query : $scope.query
        };

        ResultsFactory.getResults(q).then(function(response){
        $scope.results = response.data.results;

        });
    }
}]);
```

> 在AngularJS中，factory组件和service组件是交互使用的。想要更多对他们了解更多，
请参考stack overflow里面的一个讨论：http://stackoverflow.com/questions/15666048/angularjs-service-vs-provider-vs-factory

*index.html*是由HTML模板组成的。当页面加载时，模板默认（打死我也不会说缺省）是隐藏的。当结果数组发出数据的时候，模板将通过*ng-repeat*指令生成html标记。

在*app.js*中，我们先创建一个名为*searchApp*的AngularJS模组。然后我们创建了一个叫做*ResultsFactory*的工厂，这个工厂的唯一作用是发起AJAX请求返回一个promise。
最后，我们创建了一个controller，名为*AppCtrl*，用于与factory合作更新视图。

> 如果你对promise一无所知，参考：http://www.dwmkerr.com/promises-in-angularjs-the-definitive-guide/

按钮上*ng-click*指令声明的*search*在AppCtrl中已经设置好了。这个按钮只有在搜索框里面输入有效数据的时候才会启用。当点击**Search**按钮的时候，controller里面注册的事件监听器将会被调用。
这里我们将创建服务端需要的query对象以推进和调用*ResultFactory*里面的*getResults*方法。*getResults*方法返回一个promise，这个promise对象在服务端响应返回之后才可以被解析。
假设服务端响应成功，我们会把服务端返回的搜索结果传入*$scope.results*。  

*$scope*对象的变更将触发*results*里面所有实例的更新。这样一来就触发了HTML模板上的*ng-repeat*指令，然后这个指令解析新的*results*数组，生成html标记。这样，UI上就成功的显示了最近=新的搜索结果。  


> 下载范例代码或者咨询作者，请访问Github：
Github地址:https://github.com/learning-ionic/Chapter-1

上面的范例给你展示了如何有序推进你的代码架构，使你的代码可维护，可测试。
现在，如果想要在每个搜索结果之间插入一个图片这样的需求就变得非常简单了。