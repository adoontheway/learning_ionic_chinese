> 用到的名词
* button 按钮
* animation 动画
* transition 过渡

## 使用变量与混合式
现在，我们学习过了Ionic框架的两个核心部分，接下来我们就要学会如何使用他们。  
打开*_button.scss*。如果你记得的话，早先我们使用按钮组件的时候，我们添加的所有按钮样式或者按钮类型都是由*button*开头的。例如：
```
<button class="button button-positive button-block">Click Me</button>
```
*button*类为按钮组件提供了默认的样式。其他的类提供了修改的样式或者类型。多么了不起的一个CSS解耦方法，对吧？  
回到咱们的文件，你将看到*button*类是如何设置的。在*button*类定义里面，我们可以看到很多的变量和混合式，这些我们之前见过一部分。  
*button*类的一部分摘抄如下：
```
&.button-positive {
    @include button-style($button-positive-bg, $button-positive-border,$button-positive-active-bg, $button-positive-active-border, $buttonpositive-text);
    @include button-clear($button-positive-bg);
    @include button-outline($button-positive-bg);
}
```
在这里有三个混合式用来生成一个*button-positive*的样式。这个样式将会在*button*和*button-positive*应用到元素的时候才会生效。  
另一个需要查阅的文件是*_util.scss*。所有的工具类都会生成在这里，例如：*hide*， *show*， *padding*等等。  
如果你想要对动画和过渡做任何变更的话，你可以去查看：*_animations.scss* 和 *_transitions.scss*。  
这些文件名对于查找组件的SCSS代码帮助非常大。  

## SCSS工作流
现在我们知道了SCSS在哪里设置，如何设置的，我们可以使用下面的工作流在Ionic项目中使用SCSS。  
步骤如下：
1. 设置好SCSS。
2. 打开*scss/ionic.app.scss*文件。
3. 在导入Ionic SCSS框架之前，添加/更新我们需要重写的变量。
4. 在导入Ionic SCSS框架之前，添加需要的字体。
5. 在导入Ionic SCSS框架之后，添加/重写预定义的类或者创建一个新的类。
然后，一个典型的自定义的*ionic.app.scss*文件就诞生了：
```
// Override or add variables
$positive: teal;
$custom: #aaa;
// add custom button variables
$button-custom-bg: $custom !default;
$button-custom-text: #eee !default;
$button-custom-border: darken($custom, 10%) !default;
$button-custom-active-bg: darken($custom, 10%) !default;
$button-custom-active-border: darken($custom, 10%) !default;
// define the ionic fonts path
$ionicons-font-path: "../lib/ionic/fonts" !default;
// import Ionic SCSS Framework
@import "www/lib/ionic/scss/ionic";
// build a custom button class that is specific to our app
.button-custom {
    /*
    Usage : <button class="button button-custom">
    Custom Styled Button
    </button>
    */
    @include button-style($button-custom-bg, $button-custom-border,
    $button-custom-active-bg, $button-custom-active-border, $button-customtext);
    @include button-clear($button-custom-bg);
    @include button-outline($button-custom-bg);
}
```
如果你想完全的改变Ionic app的外观和感觉，你可以从重写默认的颜色样本开始：
```
$light: #fff !default;
$stable: #f8f8f8 !default;
$positive: #387ef5 !default;
$calm: #11c1f3 !default;
$balanced: #33cd5f !default;
$energized: #ffc900 !default;
$assertive: #ef473a !default;
$royal: #886aea !default;
$dark: #444 !default;
```
然后你就可以发现你改的是哪一个组件。导航到正确的SCSS文件，查看对这个类有影响的变量。然后返回*ionic.app.scss*对他们进行重写。