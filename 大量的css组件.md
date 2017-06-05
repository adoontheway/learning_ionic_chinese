> 用到的名字，如有更好的建议，请联系我：
* item 条目
* button  按钮
* class-named 类名为
* list 列表
* element 元素
* header 页头
* footer 页脚

## 页面结构
接下来，我们将要了解单页面Ionic应用需要的页面结构。接下来的内容我们可以新建一个空项目（空白模板的项目）。  
运行以下命令以创建一个空app：
```
ionic start -a "Example 6" -i app.example.six example6 blank
```
使用*cd*命令，进入*example6*文件夹然后运行以下命令：
```
ionic serve
```
你将会在默认的浏览器中看到空白app启动了。  
打开*example6/www/index.html*文件。在*body*标签中，你应该看到这样的一个结构：
```
<ion-pane>
    <ion-header-bar class="bar-stable">
        <h1 class="title">Ionic Blank Starter</h1>
    </ion-header-bar>
    <ion-content>
    </ion-content>
</ion-pane>
```
整个页面都被包装在一个*ion-pane*指令中。  
*ion-pane*是一个简单的适配窗口的容器。http://ionicframework.com/docs/api/directive/ionPane/  
接下来，你可以看到一个*ion-header-bar*指令(http://ionicframework.com/docs/api/directive/ionHeaderBar/)。
这个指令给页面添加了一个固定的页头。要注意一下添加给*ion-header-bar*指令的类属性。  
除此以外，Ionic有9个心情的颜色样本。如下：  
![Cover](imgs/chapter-3-7.png 'cover')

你可以看到我们当前*ion-header-bar*指令应用了*bar-stable*类。可以通过将stable替换为上表中的任意名字来改变页头。  
例如，如果将*bar-stable*替换为*bar-assertive*，页头的背景色将会改变，效果看起来将是这样子的：  
![Cover](imgs/chapter-3-8.png 'cover')

简单快捷，对吧？下一章中，我们将要学习使用SCSS覆盖默认的颜色样本。  
页面上*ion-header-bar*后面的指令是*ion-content*。 * ion-content * （http://ionicframework.com/docs/api/directive/ionContent）指令启用了构建内容区域，这篇区域是屏幕的不动产，且需要滚动。
同时你也可以通过*$ionicScrollDelegate*更好的对他进行控制。这部分的详细信息，参考 *第五章 Ionic指令与服务*。  
为了完善页面结构，我们将会给他添加一个页脚（footer）。在*ion-pane*的末尾，添加：
```
<ion-footer-bar class="bar-assertive">
    <div class="title">Footer</div>
</ion-footer-bar>
```
然后，保存文件；在浏览器中，你将看到如下：  
![Cover](imgs/chapter-3-9.png 'cover')

你可以像上面这样使用Ionic搭建一个单页面app，这样子去构建你的app：
```
<body ng-app="starter">
    <ion-pane>
        <ion-header-bar class="bar-assertive">
        ...
        </ion-header-bar>
        <ion-content>
        ...
        </ion-content>
        <ion-footer-bar class="bar-assertive">
        ...
        </ion-footer-bar>
        </ion-pane>
</body>
```
以上结构的无指令版本：
```
<div class="pane">
    <div class="bar bar-header bar-assertive">
    ...
    </div>
    <div class="content has-header has-footer padding">
    ...
    </div>
    <div class="bar bar-footer bar-assertive">
    ...
    </div>
</div>
```
当然，你也可以很方便的给页头或者页脚添加按钮。在header里面添加按钮的结构大概是这样子的：
```
<ion-header-bar class="bar-assertive">
    <div class="buttons">
        <button class="button">Left</button>
    </div>
    <h1 class="title">Ionic Blank Starter</h1>
    <div class="buttons">
        <button class="button">Right</button>
    </div>
</ion-header-bar>
```
*ion-header-bar*中，*h1*指令前添加的按钮将会显示在左边，*h1*标签后面的将显示在页头的右边。如下：  
![Cover](imgs/chapter-3-10.png 'cover')

在页脚中使用同样的标注也可以同样生成这些按钮。  
*ion-header-bar*指令是生成页头的一个途径。*ion-header-bar*用来生成静态页头是完美之选。但是当我们引入Ionic状态路由的时候，这些事情很快就变得棘手起来。
当你在处理一个多页面应用的时候，你想让Ionic基于导航自动显示返回按钮，我们将会使用*ion-nav-bar*替代*ion-header-bar*。
稍后我们在学习Ionic状态路由的时候会讲到*ion-nav-bar*。  
> 更多关于页头组件的信息，请参考：http://ionicframework.com/docs/components/#header。
更多关于content组件信息，请参考： http://ionicframework.com/docs/components/#content
更多关于页脚组件的信息，请参考： http://ionicframework.com/docs/components/#footer

## 按钮
Ionic在尺寸和样式上，为按钮提供了丰富多彩的变化。  
在*www/index.html*的*ion-content*指令中，更新如下代码你就会看到不同按钮的变化:
```
<ion-content class="padding">
    <button class="button">
    Default
    </button>
    <button class="button button-full button-positive">
    Full Width Block Button
    </button>
    <button class="button button-small button-assertive">
    Small Button
    </button>
    <button class="button button-large button-calm">
    Large Button
    </button>
    <button class="button button-outline button-dark">
    Outlined Button
    </button>
    <button class="button button-clear button-energized">
    Clear Button
    </button>
    <button class="button icon-left ion-star button-balanced">
    Icon Button
    </button>
</ion-content>
```
注意看*ion-content*指令的类属性。这个将会给*ion-content*指令的元素间加上一个10px的间隔。保存文件之后，可以看到如下效果：  
![Cover](imgs/chapter-3-11.png 'cover')

以上截屏显示了所有基于Ionic颜色样本的按钮。  
> 更多关于按钮组件的信息，请参考：http://ionicframework.com/docs/components/#buttons

## 列表
任何app最具代表性的组件莫过于显示一系列条目的列表了。列表和其他Ionic CSS组件一样，结构非常简单，列表都是由CSS类和HTML结构驱动的。
在Ionic中，如果你有一个带有名为list的类名父元素和任意数量的带有类名item的子元素，这些条目会以Ionic式列表的方式自动排列。例如：
```
<ul class="list">
    <li class="item">
    Item 1
    </li>
    <li class="item">
    Item 2
    </li>
    <li class="item">
    Item 3
    </li>
</ul>
```
你也可以这么写：
```
<div class="list">
    <div class="item">
    Item 1
    </div>
    <div class="item">
    Item 2
    </div>
    <div class="item">
    Item 3
    </div>
</div>
```
两者都会导向同样的布局显示，如下：  
![Cover](imgs/chapter-3-12.png 'cover')

基于我至今为止的Ionic经验，当列表有超过250个通过ng-repeat与对象数组创建的条目的时候，并且每个对象有超过10个属性，这个时候应用的响应就会变慢。
就这么说，你可以根据这个限制基于需求去优化app的执行效率。  
Ionic组件的多功能性都在他的类里面。大部分你想要的布局这些类都提供。  
例如，如果你想给个在每个列表条目的左边添加图标，你只需要给条目加一个名为*item-icon-left*的类就可以了。这样，条目的左边就会为添加图标留出足够的空间。  
> 可以参考这个范例: http://ionicframework.com/docs/components/#item-icons

同样的，当你想要在每个条目的左边添加一个缩略图的时候，你只需要添加一个名为*item-thumbnail-left*的类就可以了。  
> 缩略图的范例可以参考： http://ionicframework.com/docs/components/#item-thumbnails
关于列表更多信息，请参考：  http://ionicframework.com/docs/components/#list

## 卡片(Cards)
卡片上移动设备上最好的内容陈列设计模式。对于用来展示用户个人内容的页面或者app，卡片上最佳之选。
整个世界在移动设备上展示内容以及桌面上展示某些案例，都在走向卡片模式。
典型的代表有[Twitter]((https://dev.twitter.com/cards/overview)，和[Google Now](http://www.google.com/landing/now/#cards)。  

所以，你也可以简单的将这种设计模式带入到你的app中。你需要做的只是将你的个人内容设计为适合卡片展示，然后添加一个名为card的类给容器。
如果你想展示一系列卡片作为列表，你只需要将card类添加到列表容器即可。  
以下是一个简单的展示天气信息的卡片秀，如下：
```
<ion-content class="padding">
    <div class="list card">
        <div class="item text-center">
         <h1>Today's Weather</h1>
        </div>
        <div class="item item-body">
        <p>
        <div class="text-center">
        <i class="icon ion-ios-partlysunny"  style="font-size:128px"></i>
        </div>
        <div class="text-center">
        <h2>Partly Sunny</h2>
        </div>
        </p>
    </div>
    <div class="item tabs tabs-secondary tabs-icon-left">
        <a class="tab-item" href="#">
            <i class="icon ion-thumbsup"></i> Like
        </a>
        <a class="tab-item" href="#">
            <i class="icon ion-chatbox"></i> Comment
        </a>
        <a class="tab-item" href="#">
            <i class="icon ion-share"></i> Share
        </a>
        </div>
    </div>
</ion-content>
```
你的页面效果应该是这样的：  
![Cover](imgs/chapter-3-13.png 'cover')

这个设计在同时显示了所有的潜在信息的时候又不失优雅。下次你想要向用户展示你的个性的时候，考虑一下使用卡片。

## Ionicons 图标
Ionic有大量的字体图标，上面看到的天气图标就是其中一个。导航到http://ionicons.com/， 你看到的所有字体图标都可以立即使用。

为了方便起见，提供一个搜索条了来搜索指定类型的图标。例如，当你在搜索条里输入 *sunny* ，你就可以看到上面截屏里面显示的图标。  
重要警告，一定要确保你的Ionicons版本和你的Ionic CSS文件里面的Inicons版本一致。
Ionic团队持续添加新的图标和更新版本号。此处使用的sunny图标是在Ionicons 2.0.1版本发布的。
> 更多关于Ionicons的信息，请参考：http://ionicframework.com/docs/components/#icons

## 表单元素
Ionic也带来了他的表单元素和布局。
以下是一个简单的登录表单结构：
```
<ion-content class="padding">
    <div class="list">
        <label class="item item-input">
          <span class="input-label">Username</span>
          <input type="text">
        </label>
        <label class="item item-input">
          <span class="input-label">Password</span>
          <input type="password">
        </label>
    </div>
</ion-content>
```
效果图：  
![Cover](imgs/chapter-3-14.png 'cover')

你也可以通过给标签添加一个*item-floating-label*类来创建一个花式表单：
```
<ion-content class="padding">
    <div class="list">
        <label class="item item-input item-floating-label">
          <span class="input-label">Username</span>
          <input type="text" placeholder="Username">
        </label>
        <label class="item item-input item-floating-label">
          <span class="input-label">Password</span>
          <input type="password" placeholder="Password">
        </label>
    </div>
</ion-content>
```
效果图如下：  
![Cover](imgs/chapter-3-15.png 'cover')

你也可以给这些表单元素添加图标。只需要在标签（label）里面添加一个带*placeholder-icon*的*i*标签（tag）就可以了：
```
<ion-content class="padding">
    <div class="list list-inset">
        <label class="item item-input">
          <i class="icon ion-search placeholder-icon"></i>
          <input type="text" placeholder="Search...">
        </label>
    </div>
</ion-content>
```
效果图如下：  
![Cover](imgs/chapter-3-16.png 'cover')

你也可以添加其他的表单元素，例如文本域(text ares)和选择列表(select)；他们会如期出现并且会非常整齐的融入到其他表单组件中:
```
<ion-content class="padding">
    <div class="list">
        <label class="item item-input">
          <textarea placeholder="This is a &lt;textarea&gt;
          &lt;/textarea&gt;"></textarea>
        </label>
        <label class="item item-input item-select">
          <div class="input-label">
          Gender
          </div>
          <select>
            <option>Male</option>
            <option>Female</option>
          </select>
        </label>
    </div>
</ion-content>
```
效果图如下：  
![Cover](imgs/chapter-3-17.png 'cover')

有两种方法展示复选框。你可以将他展示为复选框，也可以作为切换开关。  
以下标记代码展示了一个可选的水果列表：
```
<ion-content class="padding">
    <ul class="list">
        <li class="item item-checkbox">
            <label class="checkbox checkbox-assertive">
              <input type="checkbox">
            </label> Apples
        </li>
        <li class="item item-checkbox">
            <label class="checkbox">
              <input type="checkbox">
            </label> Oranges
        </li>
        <li class="item item-checkbox checkbox-energized">
            <label class="checkbox">
              <input type="checkbox">
            </label> Lemons
        </li>
    </ul>
</ion-content>
```

![Cover](imgs/chapter-3-18.png 'cover')

> Ionic给app默认的是iOS样式的主题；因此，你可以看到那个圆圈复选框。在[第五章， Ionic指令与服务]()中我们将学习如果修改他。

以下标记显示了可开闭的的切换开关：
```
<ion-content class="padding">
    <ul class="list">
        <li class="item item-toggle">
            Wifi
            <label class="toggle toggle-assertive">
            <input type="checkbox">
            <div class="track">
              <div class="handle"></div>
            </div>
            </label>
        </li>
        <li class="item item-toggle">
            Bluetooth
            <label class="toggle toggle-positive">
            <input type="checkbox">
            <div class="track">
              <div class="handle"></div>
            </div>
            </label>
        </li>
        <li class="item item-toggle">
            Aeroplane Mode
            <label class="toggle toggle-calm">
            <input type="checkbox">
            <div class="track">
              <div class="handle"></div>
            </div>
            </label>
        </li>
    </ul>
</ion-content>
```

![Cover](imgs/chapter-3-19.png 'cover')

最后，我们将会给基于Ionic CSS的组件添加一个输入范围。
在处理用户输入的时候，这是一个非常便利和强大的组件。最佳展示范例是一个亮度调节滑块，看起来是这样子的：  
![Cover](imgs/chapter-3-20.png 'cover')

以上效果图需要的代码是这样子的：
```
<ion-content class="padding">
    <div class="list">
        <div class="item range range-positive">
            <i class="icon ion-ios-sunny-outline"></i>
            <input type="range" name="volume" min="0" max="100" value="33">
            <i class="icon ion-ios-sunny"></i>
        </div>
    </div>
</ion-content>
```
