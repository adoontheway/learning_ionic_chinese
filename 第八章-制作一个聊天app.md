# 制作一个聊天app
鉴于我们已经学习过了所有移动混合app开发需要的只是，本章我们就真枪实刀的来做一个了。
我们将要制作的是一个信息应用（聊天应用，短信应用），叫做*Ionic Chat*。
*第六章 书店App*中，我们着重整合REST API，
本章我们将要制作的Ionic Chat app更多的关注于利用Ionic整合设备功能，例如摄像头和Geolocation，
同时也会重点关注与实时数据存储（如Firebase）对话。  
我们将涵盖如下主题：
* [初步理解Firebase与设置一个Firebase账号](81--理解firebase.md)
* [了解AngularFire](82-了解angularfire.md)
* [了解应用架构](83-理解应用架构.md)
* [搭建Ionic app并进行编译](84-新建ionic.md)
* 安装所需插件并整合到Ionic App:[8.5.1](85-安装插件.md),[8.5.2](852-安装插件-2.md),[8.5.3](853-安装插件-3.md)
* [在设备上测试app](86-测试.md)

> 关于本章，你也可以通过以下Github目录来访问源代码，发起issue，与作者沟通:
https://github.com/learning-ionic/Chapter-8

## Ionic聊天应用
我们本章中将要制作的原因名为Ionic Chat。app的目的是让你熟悉使用AngularFire和Ionic制作的聊天应用，同时使用ngCordova整合Cordova插件与Ionic。  
首先我们会学习Firebase，然后聊一点AngularFire，最后学习如何整合AngularFire与Ionic Chat应用。我们将使用Firebase作为实时数据存储。
Firebase将负责实时同步数据。同时我们将使用oAuth Cordova插件与Firebase Auth组合来管理用户的认证。  
一旦用户登录后，他将在第一个标签页中看到所有的在线用户。第二个标签页是聊天历史和当前参与聊天的用户组成。
最后，第三个标签页是设置和登出页。  
当用户点击聊天列表里的人名的时候，将会打开一个聊天也，聊天页里可以看到过往聊天历史记录，可以发送新的信息，图片以及地理信息给其他用户。
> 为简单起见，应用中我们只展示所有的在线用户。你喜欢的话，可以实现一个“添加好友”功能。