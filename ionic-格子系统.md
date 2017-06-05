> 用到的名词，如有更好的建议请联系我
* layout 布局
* FlexBox 弹性盒
* grid 格子，栅格
* class 类
* markup 标记
* viewport 视窗，窗口
* header 头，页头
* footer 脚，页脚

## Ionic 格子系统

你需要使用Ionic提供的格子系统对组件布局进行一个良好的管理。  
Ionic格子系统的一个优秀之处是他是基于FlexBox的。FlexBox，或者说CSS Flexible Box Layout Module（CSS弹性盒布局模块）提供了一个盒子模型供用户界面设计。  
> FlexBox的更多信息参考： http://www.w3.org/TR/css3-flexBox/，以及一个很不错的FlexBox手册：https://css-tricks.com/snippets/css/a-guide-to-flexbox/

基于FlexBox的格子系统的好处是你不需要任何一个固定行的格子系统。你可以随你乐意在一列中铺任何行数，格子系统将会自动帮你设置到相同宽度。
这一点不同与其他基于CSS的格子系统，你无需担心添加到格子系统中的行的class数量。  
为了初步了解格子系统，打开*exmaple5/www*文件夹里面的*index.html*，在*ion-content*里面加上以下代码：
```
<div class="row">
    <div class="col">col-20%-auto</div>
    <div class="col">col-20%-auto</div>
    <div class="col">col-20%-auto</div>
    <div class="col">col-20%-auto</div>
    <div class="col">col-20%-auto</div>
</div>
```
然后在*<head>*标签后加上以下样式，以区分不同内容：
```
<style>
.col {
    border: 1px solid red;
}
</style>
```
> 以上样式不是使用格子系统的必须样式；他在这里只是用来明显区分布局里面列之间的分界线。

> 译者测试：使用yo建立的项目， style.css的路径和 app.js的文件路径可能会有问题，这个地方自己改就可以了，有可能是generator-ionc的版本比较旧或者是其他原因造成的

保存文件，然后通过*cd*进入到*example5*文件夹然后运行以下命令：
```
ionic serve
```
然后你将看到如下效果：  
![Cover](imgs/chapter-3-0.png 'cover')

为验证宽度是否是自动变化的，我们将子*div*缩减到3个，如下：
```
<div class="row">
    <div class="col">col-33%-auto</div>
    <div class="col">col-33%-auto</div>
    <div class="col">col-33%-auto</div>
</div>
```
之后你可以看到：
![Cover](imgs/chapter-3-1.png 'cover')

没有任何麻烦，也不需要任何计算；你需要做的仅仅是添加你要使用的列进去就可以了，他们将会自动帮你调整到相等的宽度。  
但是这也意味着你不能使用自定义的宽度。想到使用自定义宽度可以简单的通过使用Ionic提供的类就可以达成。  
例如，假设你要将之前例子中的第一列跨度改为50%其余2列使用剩下的宽度；你需要做到的是给第一个*div*添加一个名为*col-50*的类，如下：
```
<div class="row">
    <div class="col col-50">col-50%-set</div>
    <div class="col">col-25%-auto</div>
    <div class="col">col-25%-auto</div>
</div>
```
然后你将看到：  
![Cover](imgs/chapter-3-2.png 'cover')

以下列表是关于宽度使用方面的一些预定义的类：  
![Cover](imgs/chapter-3-3.png 'cover')

对于所有的*col*类，你可以使用上表中的任意类来制定宽度。  
你也可以对列进行偏移。例如，将以下标记加入到我们的范例中：
```
<div class="row">
    <div class="col col-offset-33">col-33%-offset</div>
    <div class="col">col-25%-auto</div>
</div>
```
然后你将看到：  
![Cover](imgs/chapter-3-4.png 'cover')

第一个*div*偏移量33%，其他两个*div*瓜分了剩下的*~66%*。*offset*类做的是在*div*左边进行一个指定比例的填充。  
以下是一份应用宽度偏移的预定义类的列表：    
![Cover](imgs/chapter-3-5.png 'cover')

你也可以在垂直方向上排列这些列。这是格子系统中使用FlexBox带来的另一个好处。  
在早先添加了*offset*的列后面添加以下标记：
```
<div class="row">
    <div class="col col-top">.col-top</div>
    <div class="col col-center">.col-center</div>
    <div class="col col-bottom">.col-bottom</div>
    <div class="col">1
        <br>2
        <br>3
        <br>4
    </div>
</div>
```
结果如下：  
![Cover](imgs/chapter-3-6.png 'cover')

如果你行中的列有高于其他列的，你可以给那一列添加*col-top*类以将他的内容定位的本行的顶部，如上。
或者你可以添加*col-center*类以将他的内容在本行中居中，或者*col-bottom*以将他的内容相对本行进行底部对齐。  
有了这个简单而强大的格子系统，布局是无限可能的。  
> 在*第六章 创建一个书店APP*中，我们将会研究响应式格子（布局）和使用*ng-repeat*构建一个动态格子（系统）。
更多关于Ionic格子系统的信息，请参考： http://ionicframework.com/docs/components/#grid
