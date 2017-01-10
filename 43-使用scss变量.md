> 用到的名词
* mixin 混合式，混合体

## 使用Ionic SCSS
这部分涵盖了如何自定义Ionic SCSS变量和混合式。  
我们将要编写的代码是基于你已经达到了使用SCSS的基本需求。
> 如果对于SCSS你是个新手，建议你参考以下指引：http://sass-lang.com/guide

### 基础样本
早先的时候，我们看过了Ionic提供的一些基本的样色样本：Positive，Assertive,Calm等等。他们都是Ionic团队预定义和设置好的。
如果你想将组件的颜色改为positive类，怎么办呢？下面就来学习一下。  
回到*example14*文件夹，打开*www/index.html*将*ion-nav-bar*指令上的*bar-stable*改为*bar-positive*。
接着，打开*www/templates/tabs.html*文件，移除*ion-tabs*指令上的*tabs-color-active-positive*类，添加*tabs-positive*。
> 编写本书的时候，*tabs*模板对*ion-nav-bar*使用了stable样式

为了直观显示，运行：** ionic serve **  
标签界面显示效果如下：  
![tabbed stable](imgs/chapter-4-0.png 'tabbed stable')

我们假设这是我们最后的app吧。接下来我们将要为这个布局定制主题，这个主题非常简单。所有蓝色将被替换成鸭绿色（teal）。  
打开*scss/ionic.app.scss*，复制文件顶部的这行注释：
** $positive: #387ef5 !default; **  
然后在注释块的后面加上他。接下来，我们将*$positive*变量的值的设为*teal*：
** $positive: teal; **  
保存文件，然后后台会运行Sass任务，自动生成新的*ionic.app.css*和*ionic.app.min.css*文件。之后页面就自动刷新了。
然后你就可以看到这个鸭绿色主题的页面了：  
![tabbed stable](imgs/chapter-4-1.png 'tabbed stable')

注意看positive类的所有引用是如何更新到teal的。是不是很屌？管理你的移动app的主题不再像载人航空科技那么复杂了！  

### 理解Ionic SCSS的设置
在这个部分，我们将学习Ionic SCSS是如何建立的。  
当你查看项目结构的时候，你会发现一个*scss*文件夹，里面是一个*ionic.app.scss*文件。想要对默认的Ionic主题进行自定义的话，你需要重写此处的变量，且只能是此处的变量。
如果你计划制定多个主题，我建议你在*scss*文件夹里面使用*theme1.scss*，*theme2.scss*这样的文件。  
千万记得，所有的主题文件必须有以下这两行：
```
// The path for our ionicons font files, relative to the built CSS in www/css
$ionicons-font-path: "../lib/ionic/fonts" !default;
// Include all of Ionic
@import "www/lib/ionic/scss/ionic";
```
在一个典型的主题文件中，第一部分重写SCSS变量。第二部分加载Ionic核心SCSS文件；最后我们重写生成的类。  
Ionic核心SCSS文件引用自以下两个表达式：
```
$ionicons-font-path: "../lib/ionic/fonts" !default;
@import "www/lib/ionic/scss/ionic";
```
如果你之前用过SCSS的话，你应该会知道任何使用*!default*指定的变量在没有指定值之前是没有默认值的，就跟之前范例一样。  
为了更好的理解这其中发生了什么，我们先导航到*$ionicons-font-path*变量的路径去，也就是*www/lib/ionic/fonts*。
这个文件夹包含了4个字体文件，这4个字体文件根据浏览器的兼容性而决定使用哪一个。  
接着，我们导航至Ionic SCSS框架的所在路径。也就是*www/lib/ionic/scss/ionic*。你可能也发现了，*scss*文件夹内没有叫做*ionic*的文件夹。
因为他是引用自*www/lib/ionic/scss/*文件夹内的*ionic.scss*的。同时注意*scss*文件夹内的其他SCSS文件的名字都是以下划线开头的。  
当你打开*ionic.scss*的文件的时候，你会发现，这个文件所做的只是把当前文件夹内的其他的SCSS文件导入进来：
```
@charset "UTF-8";
@import
    // Ionicons
    "ionicons/ionicons.scss",
    // Variables
    "mixins",
    "variables",
    // Base
    "reset",
    "scaffolding",
    "type",
    // Components
    "action-sheet",
    "backdrop",
    "bar",
    "tabs",
    "menu",
    "modal",
    "popover",
    "popup",
    "loading",
    "items",
    "list",
    "badge",
    "slide-box",
    "refresher",
    "spinner",
    // Forms
    "form",
    "checkbox",
    "toggle",
    "radio",
    "range",
    "select",
    "progress",
    // Buttons
    "button",
    "button-bar",

    // Util
    "grid",
    "util",
    "platform",
    // Animations
    "animations",
    "transitions";
```
如果你想要改动Ionicons，那么就去改*ionicons/ionicons.scss*；如果你想改动modal组件，那么就去改*_modal.scss*。
同理，当你想要改动动画的时候，就去改动*_animations.scss*。  
在对Ionic进行改动之前，有两个文件你一定要理解清楚：
* _variables.scss
* _mixin.scss
就像他们的名字说的一样，他们分别存放了可以被重写的变量和可以被重用的混合式。如果你打开*_variables.scss*的时候，
你会在里面看到所有的颜色，字体，填充，边距，边界等等的变量。  
例如，你可以在里面搜索*button*文本，你将会找到一整块关于按钮如何设置的配置。随便摘抄其中一部分来看看：
```
$button-positive-bg: $positive !default;
$button-positive-text: #fff !default;
$button-positive-border: darken($positive, 10%) !default;
$button-positive-active-bg: darken($positive, 10%) !default;
$button-positive-active-border: darken($positive, 10%) !default;
```
在这里可以看到positive类是如何设置按钮的主题的。只要改动了*$position*变量一下，你就可以看到按钮是外观和感觉是如何随之更新的。  
另外一个范例是关于*Grid(格子)*的部分：
```
// Grids
// -------------------------------
$grid-padding-width: 10px !default;
$grid-responsive-sm-break: 567px !default; // smaller than landscape phone
$grid-responsive-md-break: 767px !default; // smaller than portrait tablet
$grid-responsive-lg-break: 1023px !default; // smaller than landscape tablet
```
如果你想对格子的行为方式进行更改，那么就是改这里了。
同时，你可以参考这里的变量名对小，中，大尺寸的设备的设置媒体查询断点。  
你可以花费一些时间来了解这个文件以确认你可以重写哪些变量。截至目前为止，还没有任何关于SCSS变量和他的作用的官方文档。
但是*_variables.scss*文件里面的一些注释倒是很有帮助。  
接下来，打开*_mixin.scss*文件。这个文件是由Ionic组件使用的混合式组成的。例如，下面是*button-style*的混合式：
```
@mixin button-style($bg-color, $border-color, $active-bg-color,$active-border-color, $color) {
    border-color: $border-color;
    background-color: $bg-color;
    color: $color;
    // Give desktop users something to play with
    &:hover {
        color: $color;
        text-decoration: none;
    }
    &.active,
    &.activated {
        border-color: $active-border-color;
        background-color: $active-bg-color;
        box-shadow: inset 0 1px 4px rgba(0,0,0,0.1);
    }
}
```
这个混合式里面有一个背景色，边框色，以及一个激活状态颜色，然后生成了*border-color*，*background-color*，*color*，*.hover*，*.active*以及*.activated*规则。  
另一个用的比较多的混合式是*clearfix*：
```
@mixin clearfix {
    *zoom: 1;
    &:before,
    &:after {
        display: table;
        content: "";
        line-height: 0;
    }
    &:after {
        clear: both;
    }
}
```
他所做的是使row清晰化，这个类在很多地方用来管理布局。  
再次声明，官方没有任何文档可以用来理解本文的混合式。  
