# 第五章 Ionic指令与服务

回顾一下咱们到现在为止的旅程，我们从了解AngularJS作为一个客户端MVW框架的重要性。我们浏览了一下Apache Cordova，以及他是如何应用到整个混合应用开发栈里面来。
稍后，我们介绍了Ionic，解释了他是什么以及他是如何让我们轻松的开发混合应用的。
在*第三章 Ionic CSS组件和导航*中，我们看到了Ionic是如何在你的移动页面开发中用作一个唯一的CSS框架的，
在*第四章 Ionc与SCSS*中，我们学习了如何通过SCSS的能力去给组件定制主题。  

在本章中，我们将要学习Ionic指令与服务，他们将给我们提供可重用的组件以及功能以帮助我们更快的开发应用。  

看完本章，你将学会：
* [Ionic指令](51-ionic指令.md)
* [Ionic服务](52-ionic服务.md)

## Ionic指令与服务
Ionic有纯CSS驱动的组件和需要借助JavaScript的魔力达到圆满的组件。由于Ionic使用AngularJS作为他的JavaScript框架，
所有的可重用的用户界面组件都会被写成指令形式，所有可重用的JavaScript功能片都将会被写成服务形式。  
以下是一些Ionic指令的示例：
* 导航 *ion-nav-view*
* 页头与页脚 *ion-header-bar*和*ion-footer-bar*
* 列表 *ion-list*和*ion-item*
* 标签页 *ion-tabs*和*ion-tab*
* 侧边菜单 *ion-side-menus*和*ion-side-menu*

以下是一些Ionic服务的示例：
* 平台 *$ionicPlatform*
* 滚动 *$ionicScrollDelegate*
* Modals （什么鬼） *$ionicModal*
* 导航条 *$ionicNavBarDelegate*
* 历史 *$ionicHistory*
* 弹出框 *$ionicPopup*

在接下来的部分中，我们将浏览一些Ionic指令和服务，理解如何使用他们。我们将根据需求交替选择使用Ionic核心指令和服务。  
> 关于本章，你也可以通过以下Github目录来访问源代码，发起issue，与作者沟通:
https://github.com/learning-ionic/Chapter-5