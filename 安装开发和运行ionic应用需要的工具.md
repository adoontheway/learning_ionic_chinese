# 软件安装 
现在我们要开始安装运行Ionic应用所必需的软件了。  

## 安装Node.js
由于Ionic的命令行工具和构建任务需要用到Node.js，那么我们第一个需要安装的就是他了：
1. 导航至网站 https://nodejs.org/
2. 点击主页上的安装按钮就会自动下载你当前操作系统对于的安装包。或者可以导航到 https://nodejs.org/download 然后下载指定的副本。
3. 直接执行安装包就可以安装Node.js了

为了验证Node.js是否安装成功了，打开一个Terminal终端（*nix 系统）或者Prompt（Windows 系统）然后运行如下命令：
```
node -v
```
安装成功的话你就可以看到Node.js的版本了。接下来执行这个命令：
```
npm -v
```
你应该会看到npm的版本：  
![screentshot](imgs/chapter-2-1.png '')    
  
npm是Node Package Manager（Node包管理器），我们将使用他来为我们的Ionic项目下载不同的依赖包。  
  
> 你只是在开发期间需要Node.js。上面图片显示的版本号只是为了展示正确的输出。你自己的版本有可能跟这里的版本一样，或者更新一些。
本章其他版本号的显示也是同理。
  
## 安装Git
Git是一个开源的免费版本控制系统。在我们的案例中，我们会使用一个名为Bower的包管理器，这个管理器就是用Git下载需要的库的。
同时Ionic CLI也是使用Git下载项目模板的。  
  
导航至* http://git-scm.com/downloads *，下载对应平台的安装包，然后就可以安装了。
一旦你完成了安装，你就可以去你的终端/命令行运行如下命令：
```
git  --version
```
你将会看到如下输出：  
![screentshot](imgs/chapter-2-2.png '')    
  

## 安装Bower
我们将会使用[Bower](http://bower.io)来管理我们应用的依赖库。Bower是一个与npm一样的包管理器，只是过他liner/flat（线性/平面）。
这种包管理器更适合去下载网页开发所需的素材。  
Bower是建立在Node.js之上的。为了全局安装Bower，在终端/命令行里输入：  
```
npm install -g bower
```
  
> 如果你需要sudo来运行上面的命令，请重新检查你的npm安装。
想要忽略sudo进行npm全局安装，请参考：http://competa.com/blog/2014/12/how-to-run-npm-without-sudo/

一旦Bower安装成功，你可以同样通过以下口令去验证：  
```
bower -v
```
![screentshot](imgs/chapter-2-3.png '')    
  
## 安装Gulp
接下来，我们将要安装[Gulp](http://gulpjs.com/)，这是一个基于Node.js的构建系统。
他会将很多单调无聊的任务自动化处理。  
例如，当你的网络项目准备好的时候，你可能想要压缩CSS，JS和HTML文件，为Web进行图片优化，将代码推送到你的生产环境；
在此情景下，Gulp就是你需要的工具。  
Gulp有很多插件用来自动处理你大部分的单调的任务，他主要得益与开源社区的驱动。
在Ionic中，我们主要用Gulp来将SCSS代码转换成CSS。我们利用SCSS代码定制Ionic视觉元素。
在*第四章 Ionic与SCSS*中，我们会了解更多此方面的事宜。  
为能全局安装gulp，运行如下命令：
```
npm install gulp -g
```
对于 *nix系统，运行这个命令：
```
sudo npm install gulp -g
```
一旦Gulp成功安装，我们同样可以使用以下命令去验证：
```
gulp -v
```
![screentshot](imgs/chapter-2-4.png '')   
  
## 安装Sublime Text
这完全是个可选的安装。因为每个人都有他自己喜欢的文本编辑器。在用了一些其他的文本编辑器之后，我深深的爱上了Sublime Text，
纯粹是因为他的简单，还有很多的Plug和Play包。  
  
> 如果你想试试这个编辑器，请导航至 [http://www.sublimetext.com/3 ](http://www.sublimetext.com/3 )下载Sublime Text 3
  
## 安装Cordova和Ionic CLI
最后，为了完成Ionic安装，我们需要安装Ionic CLI。Ionic CLI是一个包装了Cordova CLI以及一些额外功能的包装。
> 本书中所有范例代码使用的是Cordova 5.0.0， Ionic CLI版本1.5.0，Ionic版本1.0.0（uranium-unicorn）

运行一下命令就可以安装Ionic CLI了：
```
npm install cordova@5.0.0 ionic@1.5.0 -g
```
验证安装是否成功可以运行如下命令：
```
cordova –v
```
同时也可以用这个命令：
```
ionic –v
```
![screentshot](imgs/chapter-2-5.png '') 
  
如果好奇Ionic CLI里面有些什么，试试这个命令:
```
ionic
```
你将会看到这么一对任务：  
![screentshot](imgs/chapter-2-6.png '') 
  
> 上面的截屏由于尺寸问题显示不完全，ionic还有其他一些任务

你可以阅读以下每个任务的解释以了解他们分别是做什么的。需要注意的是，其中有些任务时至今日仍是beta（试用）状态。  
  
做完以上这些事情，我们已经安装好所有Ionic开发需要的软件了。  

# 平台指引
在本书的最后，我们将会创建好app可以部署到设备上去。
由于Cordova使用HTML,CSS,以及JS代码作为输入和生成平台指定安装包，你需要在你的机器上准备好构建环境。
> Android用户可以按照Android Platform Guide： http://cordova.apache.org/docs/en/edge/guide_platforms_android_index.md.html#Android%20Platform%20Guide
的指引在你的本机上设置SDK。  
> iOS用户可以按照iOS Platform Guide：http://cordova.apache.org/docs/en/edge/guide_platforms_ios_index.md.html#iOS%20Platform%20Guide
的指引在你的本机上设置SDK。
> 开发iOS应用必需要OSX环境。

直至今日，Ionic只至此Android 4.0+（虽然他在2.3上也可以运行）和iOS 6+版本的移动平台。但是Cordova支持的更多一点。
> Cordova支持的平台参考这里：http://cordova.apache.org/docs/en/edge/guide_platforms_index.md.html#Platform%20Guides
