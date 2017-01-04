## generator-ionic
> 用到的名字，有更好的想法请联系我：
* host v. 主导
* scaffold 搭建，脚手架
* generator 生成器
* build 构建
* confguration-over-code 编码之上配置
* code-over-confguration 配置之上编码


我觉得虽然Ionic CLI非常不错了，但是他没有相对应的工作流。我说的工作流指的是开发代码和生产代码之间的分界。在通过Ionic CLI搭建的项目中，
*www*文件夹主导了开发代码和生产代码。当你的代码越来越多的时候，这会成为一个需要面对的问题了。  
这就是generator-ionic价值体现的地方了。generator-ionic是一个用来搭建Ionic项目的Yeoman生成器。如果你还不知道Yeoman是什么东西的话，
那么我就告诉你吧。Yeoman是一个使用Grunt，Bower以及Yo搭建app的脚手架工具。并且，他们很快就会支持gulp了。  
> **为什么选择Yeoman？**  和其他语言的IDE不同的是JavaScript或者说web开发没有统一的开发环境，IDE里用户只需导航到 **File | New | AngularJS项目**或者**File | New | HTML5项目**。这也是Yoeman为什么适合的地方。

Ionic有自己的CLI来搭建app。但是对于其他没有生成器的框架，Yeoman提供了基本的生成器。
> 想要了解Yeoman更多信息，请参考：http://yeoman.io/，想要研究Yeoman生成器，请参考：http://yeoman.io/generators/

Ionic也有其他一些生成器，但是我对[generator-ionic](https://gitHub.com/diegonetto/generator-ionic)的工作流和功能情有独钟。  

## 安装generator-ionic
安装generator之前，我们需要全局安装*yo*，*grunt*，以及*grunt-cli*。使用如下命令安装即可：
```
npm install yo grunt grunt-cli -g
```
Grunt是另一个与Gulp类似的构建工具。Grunt和Gulp最大的不同是：Grunt是一个编码之上配置的构建工具，而Gulp是配置之上编码的构建工具。
> 更多关于Grunt的信息参考： http://gruntjs.com/ 关于我对Gulp与Grunt的见解，这里有更详细的解读：http://arvindr21.github.io/building-n-Scaffolding

接下来，我们将要全局安装generator-ionic：
```
npm install generator-ionic –g
```
> 安装的时候带有-g标识的值需要安扎ungyici就够了。不用每次使用的时候都去安装一遍。

现在，我们可以使用generator-ionic创建一个新的 Ionic项目了。通过*cd*命令进入到*chapter2*，然后在其中建立一个新的文件夹名为*example4*。然后运行如下命令：
```
yo ionic example4
```
与Ionic CLI不同的是，你需要回到一些关于你想要怎么创建你的应用的问题。你可以参考以下回答：  
![screentshot](imgs/chapter-2-19.png '')

Yeoman将会下载项目运行所需的所有的东西。一旦Yeoman搭建完成之后，你可以进入到*example4*文件夹。你会看到里面会有大量的文件以及文件夹。
> 关于完整项目结构，可以参考： https://gitHub.com/diegonetto/generatorionic#project-structure

Ionic-CLI搭建的app和generator-ionic搭建的app有一些很关键的不同点：
* app：与Ionic CLI搭建的app不同的是，我们的开发将在*app*文件夹中进行，而不是*www*文件夹内。这就是我之前提到的代码分界。
我们在*app*文件夹中进行开发，然后运行构建脚本以清理文件和将他们放到*www*文件夹内共生产之用。
* hooks：*hooks*文件夹里面会多出了4个脚本。
* Gruntfile.js：与Ionic CLI不同的是generator-ionic使用Grunt管理任务。如果你觉得这里需要学习的东西太多的话，我建议你用Ionic CLI与GULP，而不是generator和Grunt。
> 如果你使用generator-ionic搭建app的话，不要在动*www*里面的东西。当你运行*build*命令的时候，这个文件夹里面的内容将会被清空，然后从*app*文件夹中重新生成。
可以通过这个地址查看运行app时可以用到的工作流命令： https://gitHub.com/diegonetto/generatorionic#workflow-commands  所有的CLI方法都被*grunt*命令包装起来了。
因此，例如当你想要执行Ionic服务的时候，在使用generator-ionic的情况下通过运行*grunt serve*即可。

所以，我们通过以下命令来运行搭建的app吧：
```
grunt serve
```
你可以看到跟用Ionic CLI搭建的tab app一样的输出。  
还有三个使用generator-ionic而不是Ionic CLI的理由是他支持以下：
* 代码提醒：https://github.com/diegonetto/generatorionic#grunt-jshint
* 使用Karma（一个测试框架）进行测试，使用Istanbul进行覆盖：https://github.com/diegonetto/generator-ionic#grunt-karma
* Ripple模拟器：https://github.com/diegonetto/generatorionic#grunt-ripple
想你举证使用generator-ionic的主要原因是当你的app变大的时候，你可以导入这个工作流。再次声明，这个个人推荐，可能你喜欢Ionic CLI也说不定。  
你也可以使用其他你觉得用起来比较舒服的Ionic生成器。

## 总结
本章中，你收获了一些移动混合应用架构的知识。同时你也学会了混合app是如何工作的。我们也看到了app中Cordova是如何将HTML，CSS,以及JS代码缝合到一起然后在web view中执行的。
然后，我们在本地安装了Ionic开发需要的软件。我们使用Ionic CLI搭建了一个空白模板并且分析了他的项目结构。紧接着，我们搭建了另外两个模板并且区分了他们之间的同步。
我们还安装了generator-ionic并且用他搭建了一个范例app，分析了他与Ionic CLI搭建的项目之间的不同点。
> 更多信息可以查看：http://ionicframework.com/present-ionic/slides

接下来的章节我们将理解Ionic CSS组件以及路由。这些知识会帮助我们使用Ionic API搭建有趣的用户界面和多页面应用。
