
简单来讲，Cordova是将你的网页应用与本地应用缝合的软件。  
Apache Cordova上面是这么声明的：
> Apache Cordova is a platform for building native mobile applications using
HTML, CSS and JavaScript.
> Apache Cordova是一个使用HTML,CSS,JavaScript制作本地移动应用的平台。

Apache Cordova不止是缝合网页应用和本地应用那么简单，他同时也提供了一套JavaScript写的API用来与设备的本地功能进行交互。
是的，你可以通过JavaScript调用你的摄像头，照相，然后发送到e-mail。屌不屌？  
  
为了让你理解这其中发生了什么事情，我们可以看一下下面这张图：  
![screentshot](imgs/20160922145147.png '')  
可以看到，我们有一个执行HTML/CSS/JS代码的web view。这些代码可以只是简单独立的用户界面片段；
你需要向远程服务器请求数据而发送的AJAX请求。甚至，这些代码可以做更多的事情，例如直接跟蓝牙对话，取得附近的蓝牙设备列表。

> 关于本章，你可以通过以下Github地址获取源代码，发出issue，和作者沟通https://github.com/learning-ionic/Chapter-2

在后续的用例当中，Cordova有一系列的使用的JavaScript 与web view交互然后与设备本地语言（例如，Android的Java）交流，
从而提供在他们之间建立桥梁的API。例如，如果你想知道你的app中的JavaScript是运行在何种设备上的，你只需要在你的JS文件里面使用以下代码就可以实现：
```
var platform = device.platform;
```
  
安装好设备插件之后，你还可以通过JavaScript访问UUID，model，OS版本，以及设备内部web view使用的Cordova版本：
···
var uuid = device.uuid;
var model = device.model;
var version = device.version;
var Cordova = device.Cordova;
···  

后续[第七章，Corfova和ngCordova]()我们会使用更多的Cordova插件的。
  
前面的解说是为了给你一个大概的了解，移动混合应用是如何构建的，你在web view里面如何通过JavaScript使用设备功能的。

> Cordova不会将HTML，CSS,以及JS代码转换为系统指定的二进制代码。他做的工作是包装HTML,CSS,JS代码然后在web view里面执行他们。

那么，你现在可能开始想到了，Ionic是 **将创建好HTML/CSS/JS代码运行在web view里面并且与Cordova对话以访问设备指定API**的 一个框架。