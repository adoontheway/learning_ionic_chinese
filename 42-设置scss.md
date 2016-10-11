> 用到的名词：
* dependencies 依赖库


## 在Ionic项目中使用SCSS
下面我们将要在已有的Ionic项目中设置SCSS。我们将从新建一个空白模板的项目开始。新建一个文件夹名为*chapter4*然后在其中打开终端/命令行，运行：
```
ionic start -a "Example 13" -i app.example.thirteen example13 tabs
```
我们可以通过两种方式在项目中设置SCSS：
* 手动设置
* Ionic CLI任务

### 手动设置SCSS
遵循以下步骤，手动设置SCSS：
1. 使用*cd*命令，进入*example13*： **cd example13**
2. 安装所必需的依赖库。利用模板搭建的Ionic项目都会有一个*package.json*文件。这个文件里面有设置SCSS需要的所有的依赖。
同时，项目也有一个*gulpFile.js*文件，这个文件也定义好了SCSS任务，这个任务是用来监视SCSS文件的更改，并即时编译成CSS文件的。
3. 运行以下命令可以安装这些依赖：** npm install **
4. 如果你没有全局安装Gulp，可以通过以下命令全局安装： ** npm install gulp --global **
5. 接下来，打开*www/index.html*文件，在*head*标签里面有一行注释掉的代码，大概是这样的：
```
<!-- IF using Sass (run gulp sass first), then uncomment below and remove the CSS includes above 
<link href="css/ionic.app.css" rel="stylesheet">
-->
```
移除掉注释部分，流行*link*标签。接下来，移除掉之前提及的*ionic.css*的引用。因为我们不再需要他了。
6. 回到终端/命令行，运行以下代码:** gulp sass ** 这个将会在*www/css*文件夹内生成* ionic.app.css*与*ionic.app.min.css*。

这些是你在Ionic项目里面设置SCSS所需的全部了。稍后我们会学习自定义SCSS。
### 使用Ionic CLI任务设置SCSS
现在我们要学习的是使用Ionic CLI设置任务对项目设置SCSS。鉴于我们已经在*example13*里面设置好了SCSS，我们需要新建另一个项目的来学习：  
** ionic start -a "Example 14" -i app.example.fourteen example14 tabs **  
接下来，进入*example14*目录，运行如下命令：  ** ionic setup scss **  
这个命令将会下载依赖库，移除*index.html*里面的注释内容，在*www/css*文件夹里面新建*ionic.app.css*和*ionic.app.min.css*文件。这个文件里面有设置SCSS需要的所有的依赖。
就问你，这个屌不屌！