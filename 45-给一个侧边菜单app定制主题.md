> 用到的名词
* markup 标记
* reflect 反射
* brand 个人品牌，牌子
* teal 鸭屎蓝

## 创建一个样本
为了更好地理解之前的流程，我们将创建一个自己的主题，重写变量和类。我们将搭建一个侧边菜单app，然后对他默认的外观和感觉进行更改。  
运行如下命令以创建一个新的侧边菜单app：
```
ionic start -a "Example 15" -i app.example.fifteen example15 sidemenu
```
然后通过*cd*命令进入到*example15*文件夹，运行此命令：
```
ionic setup scss
```
这个命令将会为你的项目下载和设置SCSS依赖库。打开*ionic.app.scss*。此处的主旨是不更改任何标记或者添加任何新类，只是修改一些对于的变量来映射到新的主题。  
> 这不是给你的应用定制主题的唯一途径。你也可以通过修改标记，添加新类以反射你的个人品牌。（例如 *button-mybrand*或者*bar-mybrand*），然后在SCSS里创建对应的变量和类。

我们将使用鸭屎蓝来给侧边菜单app指定主题。第一步我们要做的是修改*$stable*变量：** $stable: #009688 **  
如果在添加上面的样式之后启动app的服务，你将看到页头已经变成了鸭屎蓝，如下：  
![teal header](imgs/chapter-4-2.png 'teal header bar')
  
页头的文本是黑色的。我们把他设为白色吧。打开*_bar.scss*，找到以下部分：
```
.title {
    color: #fff;
}
```
这个看起来不是个变量。所以，在这个案例中，我们将重写这个类。在*ionic.app.scss*中，在包含了Ionic CSS框架之后，我们添加以下代码：
```
.bar.bar-stable .title{
    color:#eee;
}
```
保存之后就可以即可看到标题文本颜色发生了改变。但是，你看到那个汉堡包菜单的文本还是黑色的。这个我们也想改掉。
当你打开浏览器开发者工具检查这个汉堡包菜单的时候，你会发现一个这样的按钮：
```
<button class="button button-icon button-clear ion-navicon" menutoggle="left"></button>
```
在开发工具里面这个元素的样式里会看到*bar-stable button button-clear*这些类，他会将颜色设为#444。现在我们要追踪这个变量，然后重置他。  
由于类的第一个部分是*.bar-stable*，所以这个可能是*_bar.scss*里面的定义。然后可以在*_bar.scss*里面找到：
```
.bar-stable {
.button {
    @include button-style($bar-stable-bg, $bar-stable-border,$bar-stable-active-bg, $bar-stable-active-border, $bar-stabletext);
    @include button-clear($bar-stable-text, $bar-title-font-size);
}
}
```
注意观察混合式*button-clear*。我们可以在*_mixins.scss*里面看到他的眼代码：
```
@mixin button-clear($color, $font-size:"") {
&.button-clear {
    border-color: transparent;
    background: none;
    box-shadow: none;
    color: $color;
    @if $font-size != "" {
        font-size: $font-size;
    }
}
&.button-icon {
    border-color: transparent;
    background: none;
}
}
```
我们可以看到，这个混合式的第一个参数名为*$color*。这样我们就知道我们需要重写哪个变量了。  
回到*ionic.app.scss*文件，我们将重写*$bar-stable-text*变量：  
** $bar-stable-text: #eee; **  
保存文件，然后就可以看到以下效果：  
![teal header](imgs/chapter-4-3.png 'teal header bar')
  
接下来，我们将更改字体的颜色。我们需要重写变量*$base-color*。现在我们要给列表条目增加更多的填充空间。我们再次打开*_items.scss*，找到这个：** padding: $item-padding; **  
我们可以将*$item-padding*重写为30px。  
接下来，为每个条目制作一个淡绿色背景。鉴于没有相关变量可用，我们需要去重写这个类：
```
.item-complex .item-content{
    background:#E0F2F1;
    color:#00695C;
}
```
保存文件之后，可以在浏览器里面看到如下效果：  
![light green bg](imgs/chapter-4-4.png 'light green bg')
  
当点击登录连接的时候，将会出现一个modal弹出框。弹出框里面的**Login**按钮使用了positive类作为样式。使用以下值重写这部分：
```
$button-positive-bg: #00BFA5;
$button-positive-border: #00BFA5;
$button-positive-active-bg: #80CBC4;
$button-positive-active-border: #80CBC4;
$button-positive-text: #eee;
```
可以在*button-style*里找到所有*button-positive*使用的变量列表。这里我们处理了*button*类的两个状态。  
**Login**modal看起来将会是这样的：  
![light green bg](imgs/chapter-4-5.png 'light green bg')
  
一切看起来都是那么的美好，除了列表条目的边框颜色和列表条目被选中之后的激活的背景色。我们现在就来处理这些。
打开*_items.scss*，找到*item-style*混合式，这个混合式接收的第二个参数是*$item-default-border*。这个参考将被用作边框色。  
我们重写他为：
```
$item-default-border: #009688;
```
最后是激活背景色。在链接和按钮激活状态的部分，我们会看到一个*item-mixin-style*混合式。第一个参数名为*$item-default-active-bg*，这就是激活背景色了。
我们将在*ionic.app.scss*中重写他：
```
$item-default-active-bg: #B2DFDB;
```
完整的*ionic.app.scss*代码如下：
```
// override all $stable themed components
$stable: #009688;
// override the burger menu color
$bar-stable-text: #eee;
// override app base color
$base-color: #00695C;
// increase the item padding
$item-padding : 30px;
// override buttons
$button-positive-bg: #00BFA5;
$button-positive-border: #00BFA5;
$button-positive-active-bg: #80CBC4;
$button-positive-active-border: #80CBC4;
$button-positive-text: #eee;
// border & active bg color
$item-default-border: #009688;
$item-default-active-bg: #B2DFDB;
// The path for our ionicons font files, relative to the built CSS in www/css
$ionicons-font-path: "../lib/ionic/fonts" !default;
// Include all of Ionic
@import "www/lib/ionic/scss/ionic";
// Override title color
.bar.bar-stable .title{
    color:#eee;
}
// Override the item background and color
.item-complex .item-content{
    background:#E0F2F1;
    color:#00695C;
}
```
最终效果图如下：  
![light green bg](imgs/chapter-4-6.png 'light green bg')
  
这是一个基本的使用SCSS给你的Ionic app制定主题的范例。你可以给他添加更多的组件，然后给他们分别制定主题作为练习。  
> 我给你展示了如何在Ionic SCSS设置中找到响应的变量和混合式。这个知识点可以用在任何重写变量和重用混合式来定制主题的Ionic SCSS项目。

## 总结
本章中，我们学习了如何定制Ionic app主题。我们由设置SCSS开始，然后浏览了Ionic SCSS结构。之后，我们通过重写变量以改变应用的主题。
最后，我们建立了一个app并且改变了他的外观。  
接下来的章节里，我们将要浏览大量的Ionic指令和服务，他们将会帮助我们轻松的创建混合app。