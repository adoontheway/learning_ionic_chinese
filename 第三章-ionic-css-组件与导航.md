#Ionic CSS组件与导航
> 用到的名词，如果有更合适的请联系我：
* grid system 格子系统 


到目前为止，我们知道了什么是Ionic，在移动混合应用开发的大舞台上他适合做什么。我们也了解了两种搭建Ionic app的方法：Ionic CLI与generator-ionic。
本章，我们将学习Ionic CSS组件，Ionic格子系统，以及Ionic状态路由。我们将要使用丰富多彩的Ionic组件来创建优秀用户体验的app。
  
本章将要涵盖的范围：
* [Ionic格子系统](ionic-格子系统.md)
* [丰富的CSS组件](大量的css组件.md)
* [整合Ionic CSS组件与AngularJS](大量的css组件.md)
* [Ionic状态路由器](ionic状态路由.md)
> 可以通过Github地址获得本章源代码，发起issue，以及与作者沟通，地址是：https://github.com/learning-ionic/Chapter-3

## Ionic CSS 组件
Ionic是一个强力的移动CSS框架与一系列牛逼的AngularJS指令与服务的组合。有了这些，任何想法投入市场需要的时间极大的缩小了。
Ionic CSS框架包含了创建一个app所需要的绝大部分组件。  
为测试驱动CSS组件的可用性，我们将搭建一个空白的启动版本，然后添加Ionic组件。  
开始搭建之前，新建一个文件夹名为*chapter3*，本章的所有范例都将在此文件夹下搭建。  
> 本章中，我们每个组件建立一个app以便更好的理解。当然，如果你想要只用一个app也是可以的。

新建一个空白app的命令如下：
```
ionic start -a "Example 5" -i app.example.five example5 blank
```