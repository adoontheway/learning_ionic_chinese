
> 用到的名词：
* Single Page Application (SPA) 单页面应用
* dependency/dependencies 依赖库
* bundle 包
* template 模板
* scaffold 搭建（如果更好的建议，请联系我）

# Hello Ionic
现在咱们完成了软件安装的部分，我们将搭建一些Ionic app。  
Ionic有三个可用模板：
* Blank： 空的Ionic项目，有一个页面
* Tabs： 使用Ionic tabs构建的一个范例应用
* Side Menu： 这个是一个侧边菜单驱动导航的范例应用
为了便于理解如何搭建Ionic app，我们从空白模板（Blank template）开始。  
为了保持我们的学习进程清晰明了，我们将创建一个文件夹作为Ionic项目来工作。创建一个文件夹名为*ionicApp*，
然后在里面创建一个文件夹名为*chapter2*。  
接下来，打开一个新的命令行/终端，进入到*ionicApp*目录下的*chapter2*。然后执行如下命令：
```
ionic start -a "Example 1" -i app.example.one example1 blank
```
以上命令中：
* -a "Example 1"：这是供凡人识别的app名字
* -i app.example.on：这是app的ID/域名倒转
* example1：这个是文件夹的名字
* blank：模板名
> 更多Ionic start任务信息，请参考*附录，更多主题与贴士*

在执行任务的时候，Ionic CLI非常详细。这点你可以通过命令行/终端看得到，因为开始创建项目的时候里面会打印出大量的信息。  
开始之后，会下载一个新的空白项目然后保存到example
文件夹。接下来，会从[ionic-app-base的GitHub目录](https://github.com/driftyco/ionic-app-base)下载ionic-app-base，
在这之后， 会从[ionic-starter-template的GitHub目录](https://github.com/driftyco/ionic-starter-blank)下载 ionic-starter-template。

之后，会将config文件里面的app名字和ID更新。接下来，会运行一段脚本然后下载5个Cordova插件：
* org.apache.cordova.device (https://gitHub.com/apache/cordovaplugin-device):这个是用来获取设备信息，我们在之前的章节已经看到过
* org.apache.cordova.console (https://gitHub.com/apache/cordovaplugin-console): 这个插件的用处是确保*console.log（）*有用
* cordova-plugin-whitelist (https://github.com/apache/cordovaplugin-whitelist): 这个插件实现了白名单政策，用来在Cordova 4.0的应用的web view中进行导航。
* cordova-plugin-splashscreen (https://github.com/apache/cordovaplugin-splashscreen): 这个插件在应用启动期间实现闪屏的展示与隐藏。
* com.ionic.keyboard (https://gitHub.com/driftyco/ionic-pluginskeyboard):这个插件提供更容易的键盘交互功能，在键盘隐藏/显示的时候发出事件。

所有这些信息稍后都会添加到*package.json*文件里面，然后一个暂时的*ionic.project*诞生了。  
一旦项目创建成功，你将会看到一系列的指引告诉你后续如何操作。
输出信息大概是这么个效果：  
![screentshot](imgs/chapter-2-7.png '')

为了继续深入项目，我们将使用*cd*命令进入到*example1*文件夹。我们不按照终端/命令行的指引走，因为我们始终了解项目的设置。
一旦我们熟悉了Ionic的多样化组件，我们可以开始使用终端/命令行的指引了。  
进入到*example1*目录之后，我们可以通过以下命令为app服务了：
```
ionic serve
```
这个命令将在端口*8100*上启动一个*dev*服务器，然后在你的默认（谁敢说缺省砍死谁）浏览器中启动app。
由于你需要使用Ionic，我个人（是作者不是译者）强烈推荐使用Google Chrome浏览器或者Mozilla Firefox。  
当浏览器启动后，你可以看到空白模板。  
如果此时你运行这个：
```
ionic serve
```
你将会看到显示了以下错误信息：  
![screentshot](imgs/chapter-2-8.png '')

这个错误信息的意思是你本机的其他应用正在使用端口8100.想要解决这个问题，你可以在运行Ionic serve命令的时候指定其他端口，例如 *8200*：  
```
ionic serve -p 8200
```
应用成功启动后，我们看到了浏览器中的输出，我们在终端/命令行可以看到如下信息：  
![screentshot](imgs/chapter-2-9.png '')

如前所述，Ionic CLI的任务非常详细。他不会让你闲着。在Ionic serve命令运行的时候你就可以看到了，你可以输入**R**然后回车，此时应用会重启。
同样，你可以按**C**来启用或者急用浏览JavaScript打印日志到终端/命令行。  
运行完应用之后，按**Q**然后回车可以停止服务器。在键盘上按下**Ctrl + C**也是一样的。

## 浏览器开发者工具设置
在我们深入之前，我建议使用以下的方式去设置好你的浏览器的开发者工具。

### Google Chrome
Ionic应用启动后，通过*Cmd + Opt +I*（Mac）和 *Ctrl + Shift + I*（Windows/Linux）打开开发者工具。
然后点击顶行最后一个图标，也就是关闭按钮旁边那个，如下：  
![screentshot](imgs/chapter-2-10.png '')

这个操作将会把开发者工具码到当前也的右边。拖动浏览器与开发者工具之间的分界条知道视图看起来像一个移动设备。如下：  
![screentshot](imgs/chapter-2-11.png '')
（图中红色信息为译者标注）  

这个视图设置对修复bug和调试非常有用。  

### Mozilla Firefox
如果你是一个Mozilla Firefox（火狐）粉，上面的效果同样也可以实现。
当Ionic应用启动完成之后，通过 **Cmd + Opt + I**（Mac）和**Ctrl + Shift + I**（Windows/Linux）打开开发者工具（不是Firebug，Firefox的本地开发工具）。
然后点及浏览器窗口顶部按钮，如下：  
![screentshot](imgs/chapter-2-12.png '')

现在，可以拖动分界条来达成我们在Chrome上达到的效果了。  
![screentshot](imgs/chapter-2-13.png '')

## Ionic项目结构
到目前为止，我们搭建了一个空白Ionic应用，并且在浏览器中启动了。我们现在就过一遍项目结构。  
快速提醒你一下，我们知道Ionic是躺在Cordova应用里面的。 所以在我们过一遍Ionic代码之前，我们先来聊聊Cordova包装。  
如果已经在你的文本编辑器里面打开了*chapter2 example1*文件夹，你将在项目的根目录下看到如下结构：  
![screentshot](imgs/chapter-2-14.png '')

以下简单解释一下每个项目：
* bower.json：这个文件是由需要通过Bower加载的依赖库组成。后续我们将安装其他的Bower包以在应用中使用。
* config.xml：这个文件是由Cordova在将你的Ionic应用转换成指定平台安装包的所需元信息所组成。如果你打开config.xml，
你将会看到大量的XML标签描述你的项目。我们将会再次详细阅读此文件。
* gulpfile.js：这个文件是我们在开发Ionic应用期间需要用到的构建任务所组成。
* ionic.project：这个文件是由Ionic应用的相关信息组成。
* hooks：这个文件夹是由Cordova任务执行时候执行的脚本所组成。Cordova任务可以是以下这些：
*after_platform_add*（添加了新的平台之后），*after_plugin_add*（添加了新的插件之后），*before_emulate*（模拟之前），
*after_run*（app运行之后），等等。每一个任务都放在一个单独的文件夹，文件夹名字是Cordova任务的名字。当你打开*hooks*文件夹的时候，
你将会看到一个 *after_prepare*和一个*README.md*文件。在*after_prepare*文件夹里面，你会找到一个脚本文件名为*010_add_platform_class.js*。
这个脚本文件将会在Cordova的准备任务执行完成之后被执行。这个脚本做的事情是给*<body>*标签添加一个class，这个class的名字和应用运行的平台的名字一样。
这样可以帮助我们更好的基于平台为应用进行风格化。你可以在hooks文件夹下的*README.md*文件里找到一个可以勾搭的任务列表。
* plugins：这个文件夹是由所有添加到本项目的插件所组成。我们后续会添加一些其他的插件，然后你就可以在这里看到反应。
* scss：此文件夹是由我们将要用来风格化Ionic组件样式的的*scss*文件所组成。更多相关内容参考*第四章 Ionic与SCSS*
* www：这个文件夹是有Ionic代码所组成。你在此文件夹里面写下的任何东西都将呈现在web view中。这也是我们花费时间最多的地方。

## config.xml文件
*config.xml*文件是一个平台无关的配置文件。如前所述，这个文件是由Cordova在将你的Ionic应用转换成指定平台安装包的所需元信息所组成。  
*config.xml*文件的设置是基于W3C的Packaged Web Apps(Widgets)规格书（http://www.w3.org/TR/widgets/）的，扩展到为指定Cordova核心API功能，插件，
和指定平台设置。有两种设置你可以添加到这个文件。一个全局的（global），适用于所有设备；另一个就是指定平台的。  
如果在文本编辑器中打开*config.xml*的话，第一个遇到的标签上XML的根标签。接下来，你可以看到widget标签：
```
<widget id="app.example.one" version="0.0.1" xmlns="http://www.w3.org/ns/widgets" xmlns:cdv="http://cordova.apache.org/ns/1.0">
```
上面指定的*id*是app域名的倒置，这个是我们在搭建项目的时候提供的。其他规格都是定义在widget标签里面作为他的子标签的。
子标签包括app名字（在设备上安装完成之后显示在app图标下面的名字），app描述信息，以及作者详情。  
同时，他也有在转换*www*文件夹里面的代码为本地安装器的时候所需要的附加配置。  
content标签定义了应用的启动页面。access标签定义了app中允许加载的URL。他默认是加载所有的URL。
preferrence标签里面是一系列的名值对。例如，*DiallowOverScroll*描述的是当用户滚动超过文档顶部或者底部的时候，是否需要视觉反馈。  
更多关于平台特殊配置（platform-specific configuration），请参考此链接：
* Android： http://docs.phonegap.com/en/4.0.0/guide_platforms_android_config.md.html#Android%20Configuration
* iOS: http://docs.phonegap.com/en/4.0.0/guide_platforms_ios_config.md.html#iOS%20Configuration
> 平台特殊配置和全局配置是同样重要的。关于全局配置，请参考：http://docs.phonegap.com/en/4.0.0/config_ref_index.md.html#The%20config.xml%20File

## www文件夹
如前所述，这个文件组成了我们的Ionic应用，HTML，CSS以及JS代码。当你打开*www*文件夹，你会看到如下结构：  
![screentshot](imgs/chapter-2-15.png '')

这个图不全  
![screentshot](imgs/chapter-2-16.png '')

  没错，这个图是我本地的

让我们详细的看一下这些东西：
* index.html 这个是应用的启动文件。*config.xml*里面的的*src*标签就指向的这个文件。由于我们使用AngularJS作为我们的JavaScript框架。
此文件用作我们的单页面应用的基本页/主页是再理想不过了。当你打开*index.html*的时候，你将会发现一个ng-app属性，这个属性在js/app.js里面定义指向到了开始模块。
* css：这个文件夹是有咱们app使用的特有样式组成。
* img：这个文件夹里面都是咱们app需要使用的图片。
* js：这个文件夹里面都是咱们app需要使用到的JavaScript代码。我们也是在这里写AngularJS代码的。打开app.js文件，就会发现里面设置好了AngularJS模块，并且传入了Ionic作为依赖。
* lib：这里是我们通过bower安装的包的存放处。当我们创建app的时候，这个文件夹是随之建立的，里面也加载了Ionic文件。
如果你想要重新下载一遍素材与其依赖，可以通过在终端/控制台cd进入到example1文件夹，然后运行以下命令：
```
bower install
```
然后你就可以看到下载了额外的4个文件夹。这些依赖都是在ionic-bower包里面列出的在项目的根目录的bower.json文件里面展示的。  
理想状态下，我们不一定需要在我们的app里面使用这些依赖库。相反，我们更倾向于使用建立在这些依赖库之上的Ionic 包。

这样咱们就看完了这个空白模板。在开始搭建另一个模板之前，我们快速的瞥一眼*www/js/app.js*。  
如你所见，我们创建了一个名为ie*starter*的AngularJS模块，然后将*ionic*注入其中。  
*$ionicPlatform*服务注入到*run*方法中作为依赖。这个服务用来检测当前工作平台，同时也会处理设备（Android）上的物理按钮。当前内容中，我们使用的是
*$ionicPlatform.ready*方法，这个方法用来在设备准备完成之后进行相关操作。  
最好的做法或者说在某些案例中必需要这么做：将你所有的代码包含到*$ionicPlatform.ready*中去。这样一来，你的代码将只会在整个app初始化之后执行。  
目前为止，你可能在使用AngularJS进行开发的时候用到的是他的Web方面。但是当使用Ionic的时候，我们还需要使用AngularJS代码里面设备功能相关的代码。
Ionic给我们提供了组织好的了服务来达成这些功能。我们可以回首[第一章 Ionic - 搭载AngularJS](ionic---搭载angularjs---service.md)的自定义服务，并且，我们
后续将会在[第五章 Ionic 指令与服务]()中更加深入Ionic服务。  

## 搭建tabs模板
为了对Ionic CLI和项目结构有个更好的感觉，我们也会搭建另外两个开始模板。先来tabs模板。  
使用*cd*密码返回*chapter2*文件夹，然后运行以下爱命令：
```
ionic start -a "Example 2" -i app.example.two example2 tabs
```
你可以看到，*example2*文件夹里面就建好了一个新的tabs模板了。使用*cd*命令进入*example2*然后执行以下命令：
```
ionic serve
```
你将会看到如下截屏的标签界面应用：  
![screentshot](imgs/chapter-2-17.png '')

标签排在页面底部。我们将在[第三章 Ionic CSS 组件与导航]()以及 [第五章 Ionic 指令与服务]()中深入订制方法的知识。  
当你返回*example2*文件夹里面分析项目结构的时候，所有的内容基本与空白模板一样，除了*www*文件夹里面的内容。  
这一次，你会发现一个新的文件夹叫做*templates*。这个文件夹将由每个AngularJS路由页面部分所组成。*js*文件夹里面也会有两个新的文件：
* controller.js：这里是由AngularJS的控制器代码所组成
* services.js：这里是由AngularJS的服务代码所组成

现在，你大概对 **Ionic是如何与AngularJS整合以及所有的组件是如何手牵手的去进行协作** 有了一个比较好的理解。
当我们再深入了解Ionic之后，我们将会对这个结构有更多的感觉。  

## 搭建侧面菜单模板
现在我们将进行最后一个模板的搭建。使用*cd*命令回到*chapter2*，然后运行以下命令：
```
ionic start -a "Example 3" -i app.example.three example3 sidemenu
```
然后使用*cd*命令进入*example3*文件夹，运行如下命令：
```
ionic serve
```
然后将会输出类似如下截屏的效果：  
![screentshot](imgs/chapter-2-18.png '')

你可以自己分析项目结构，然后对比一下较之其他两个模板有何不同。
> 你可以使用*ionic start -l*或者*ionic templates*查看可用的模板列表。然后使用*ionic start*任务试着去搭建这些模板。
