## AngularJS 服务  

> 用到的名词：
* directives 指令
* controller 控制器
* Dependency Injection 依赖注入
* lazy initialize 延迟初始化
* singlton 单例
* destroy 销毁
* factory 工厂
* local storage 本地存储

AngularJS服务都可以通过DI注入到指令与控制器里。  
这些对象是由一系列的简单业务逻辑碎片组成。  
  
当组件需要依赖他们的时候，AngularJS服务都会延迟初始化。同时，所有的服务都是单例，单例的意思是在每个app里面仅初始化一次。  
这样可以让服务在控制器之间完美的共享数据，并根据需求将他们存入缓存。  

*$interval*是一个AngularJS服务。*$interval*和*setTimeInterval()*是一样的。
在注册这个服务的时候，他将*setTimeInterval()*包装起来并且返回一个*promise*对象。
这个promise后续可以用来销毁*$interval*。  
  
另一个比较简单的服务是*$log*。这个服务将日志信息输出到浏览器控制台。来个比较简单的例子：  
```
myApp.controller('logCtrl', ['$log', function($log) {
    $log.log('Log Ctrl Initialized');
}]);
```  
由此，可以看到服务的强大之处以及理解业务逻辑碎片是如何在整个app中重用的。  
  
你也可以自定义自己的服务。例如建一个计算器服务试试看，包括了加，减，乘，除，平方等等方法。  
  
回到我们的搜索App，我们使用了一个工厂负责服务端通讯。现在，我们将添加我们自己的服务。  

> 服务与工厂组件之间是可以互换的。可以参考这里:  http://stackoverflow.com/questions/15666048/servicevs-provider-vs-factory 

例如，当用户搜索指定关键字然后展示搜索结果的时候，我们比较倾向于将结果缓存中本地。
这样可以保证下次用户搜索同一个关键字的时候，我们无需发起AJAX请求，而是直接展示本地存储中的数据（和离线模式类似）。
  
我们就这么设计我们的服务。我们的服务将包含以下三个方法：  
* isLocalStorageAvailiable():这个方法检查浏览器是否支持本地存储API
* saveSearchResult(keyword, searchResult): 这个方法将关键字和搜索结果存放到本地存储
* isResultPresent(keyword):这个方法返回指定关键字的搜索结果

我们的服务大概是这样的：
```
searchApp.service('LocalStorageAPI', [function() {
    this.isLocalStorageAvailable = function() {
        return (typeof(localStorage) !== "undefined");
    };
    this.saveSearchResult = function(keyword, searchResult) {
        return localStorage.setItem(keyword, JSON.stringify(searchResult));
    };
    this.isResultPresent = function(keyword) {
        return JSON.parse(localStorage.getItem(keyword));
    };
}]);
```

> 本地存储不能存储对象。因此，我们必须在缓存对象之前将其字符串化，在获得本地缓存的时候对象化。
  
现在，当我们处理搜索的时候，我们的指令将会使用这个服务。更新后的mySearch指令是这样子的：
```
searchApp.directive('mySearch', ['ResultsFactory', 'LocalStorageAPI',
    function(ResultsFactory, LocalStorageAPI) {
        return {
        templateUrl: './directive.html',
        restrict: 'E',
        link: function postLink(scope, iElement, iAttrs) {
            var lsAvailable = LocalStorageAPI.isLocalStorageAvailable();
            scope.search = function() {
                if (lsAvailable) {
                var results = LocalStorageAPI.isResultPresent(scope.query);
                    if (results) {
                        scope.results = results;
                        return;
                    }
                }
                var q = {
                    query: scope.query
                };
                ResultsFactory.getResults(q).
                then(function(response) {
                    scope.results = response.data.results;
                    if (lsAvailable) {
                        LocalStorageAPI.saveSearchResult(scope.query, data.data.results);
                    }
                });
            }
            }
        };
}]);
```
之前说过，我们会先检查本地存储是否可用，然后使用*LocalStorageAPI*服务进行数据的存与取。
  
同样，Ionic也提供了自定义的服务供我们去消化。详细信息我们可以去 *第五章 - Ionic指令与服务* 查看。  
下面这个例子是Ionic的加载服务。这个服务展示了一个加载条和你提供的文本。例子代码大概是这样子的：
```
$ionicLoading.show({
    template: 'Loading...'
});
```
  
然后你就可以看到一个覆盖层（overlay）展示后台活动并阻断用户操作。