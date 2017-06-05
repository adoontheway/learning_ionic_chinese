> 用到的名词：
* pre-process 预处理器
* programmable 可编程的
* semi-colons 分号
* brace 花括号
* function 函数
* mixins 混合式
* CSS-like synax 类CSS语法

## 什么是Sass?
根据Sass文档  http://sass-lang.com/documentation/ :
> Sass is an extension of CSS that adds power and elegance to the basic language. It
allows you to use variables, nested rules, mixins, inline imports, and more, all with
a fully CSS-compatible syntax. Sass helps keep large stylesheets well organized,
and get small stylesheets up and running quickly.
Sass是一个CSS的扩展，给CSS这门基本语言添加了力量与优雅。他允许你使用变量，嵌入规则，混入，内联导入等等，
所有这些特性都是完全兼容CSS语法的。Sass帮助良好的组织大型的样式表，对于小型表单使它结构简单，运行加速。

简单来说，Sass使得SCSS可编程。你也许会问为什么我们这章讲的是SCSS，而这里却在讲Sass。
是这样的Sass和SCSS一样是一个CSS预处理器，CSS预处理器可以用他自己的方法来编写pre-CSS语法。  

Sass是作为另一个名为[HAML]((http://haml.info/)的预处理器的一部分由一群Ruby开发者开发出来的。
因此，他从Ruby那里继承了大量的语法样式，例如躲进，没有花括号，没有分号等等。  
以下是一个Sass文件的范例：
```
// app.sass
brand-primary= blue
.container
    color= !brand-primary
    margin= 0px auto
    padding= 20px

=border-radius(!radius)
    -webkit-border-radius= !radius
    -moz-border-radius= !radius
    border-radius= !radius
*
    +border-radius(0px)
```
当通过一个Sass编译器运行以上Sass代码的时候，他返回了一个老式的良好的CSS。生成的CSS大概是这样的：
```
.container {
    color: blue;
    margin: 0px auto;
    padding: 20px;
}
* {
    -webkit-border-radius: 0px;
    -moz-border-radius: 0px;
    border-radius: 0px;
}
```
但是你有没有注意到在Sass代码中有一个*brand-primary*作为一个变量，在*container*中替换了他的值的，
或者*border-radius*作为一个函数（也可以叫如混合式）当使用参数调用的时候生成CSS规则？这些在CSS都不见了。  
哪些使用*bracket-based*编程语言的人们这么些代码有点费劲。于是，SCSS就出现了。  
Sass是Syntactically Aswsome Style Sheet（语法牛逼的样式表）的简写， SCSS是Sassy CSS（时髦的CSS）的简写。
SCSS和Sass非常像，除了类CSS语法。如果使用SCSS写入上Sass代码的话，是这样子的：
```
// app.scss
$brand-primary: blue;
.container{
    color: !brand-primary;
    margin: 0px auto;
    padding: 20px;
}
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    border-radius: $radius;
}
* {
    @include border-radius(5px);
}
```
这个看起来离CSS靠近了些，对吧？这个令人印象深刻。Ionic使用SCSS给他的组件样式化。
> 关于更多的SCSS vs. Sass的信息，可以参考这个帖子： http://thesassway.com/editorial/sass-vs-scss-which-syntax-is-better

现在，我们对SCSS和Sass有了一个初步的了解，我们将在Ionic app中对组件制定主题的时候对他们进行评估。
