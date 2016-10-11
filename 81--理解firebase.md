## Firebase
Firebase是一个BAAS(Backend As A Service后端即服务)，提供了云后端服务，实时数据存储，用户验证，以及静态主机。  
> 更多信息关于Firebase请参考：https://www.firebase.com/features.html

想要快速了解Firebase如何运作，查看以下代码片：
```
var ref = new Firebase("https://<YOUR-FIREBASEAPP>.firebaseio.com");
ref.set({ name: "Arvind Ravulavaru" });
ref.on("value", function(data) {
    var name = data.val().name;
    alert("My name is " + name);
});
```
第一行中，引用了我们的Firebase示例（我们下一部分将会创建）。有了他之后，我们可以设置和保存一个JSON文档到默认的终端。
Firebase作为一个实时数据存储，使用事件驱动的方式管理和同步数据。第3行可以看到，我们订阅了一个value事件，当有新的数据插入到默认终端的时候，这里将会介入。  
为更好的理解第3行，想象一下当用户1以及在数据存储中设置了value并注册了value事件。现在，当用户在浏览器中加载这段代码，他先设置value；这样将触发用户1第3行的回调函数以告知用户1用户2的value。  
value回调将被调用，传入新加的data值的快照。*data.val*方法将返回新加的值的记录。
> 如果想要代码在页面上正常工作，还需要将Firebase的代码包含进来：
<script src="https://cdn.firebase.com/js/client/2.2.2/firebase.js"></script>

### 设置Firebase账号
可以通过  https://www.firebase.com/signup/ 申请账号，或者可以使用Github账号登录： https://www.firebase.com/login/   

一旦注册或者登录成功之后，你会被带到 https://www.firebase.com/account/#/ ，此处可以添加一个新的项目。输入app名，Firebase会判断名字是否被占用。
例如，当你输入“ionic-chat-app”的时候，你会发现这个名字被占用了（是我为制作本章app而使用的）。  

选择一个合适可用的名字点击**Create New App**。这样将会为你创建一个新的app，并附送一个Firebase URL。简单点说这个URL就是你的账户的API KEY。
相对于给用户一堆乱七八糟的字符串作为API密钥来讲，这是非常优雅的解决方案了。  

为测试一切是否正常，我们将执行之前的代码片。新建一个文件夹名为*chapter8*,在其中建立另一个文件夹*example35*。然后在*chapter8*中创建一个新文件*index.html*。写入如下代码：
```
<!DOCTYPE html>
<html>
    <head>
    <title>Firebase Test Page</title>
    <script src="https://cdn.firebase.com/js/client/2.2.2/firebase.js"></script>
    </head>
    <body>
        <input type="button" onclick="addNewName()" value="Add New Name">
        <br>
        <ul id="namesList"></ul>
        <script type="text/javascript">
        var ref = new Firebase("https://<YOUR-FIREBASEAPP>.firebaseio.com");
        ref.on('value', function(data) {
            var names = data.val();
            clearList();
            for (var n in names) {
                setName(names[n].name);
            }
        });
        function clearList() {
            document.querySelector('#namesList').innerHTML = '';
        }
        function setName(name) {
            var newName = document.createElement('li');
            newName.innerHTML = 'Name : <b>' + name + '</b>';
            document.querySelector('#namesList').appendChild(newName);
        }
        function addNewName() {
            var name = prompt('Enter Name');
            if (name) {
                // the below statement will save data to the Firebase
                // data store and will invoke the ref.on('value') callback. This will
                // the call the saveName to setdata
                ref.push({
                    'name': name
                });
            }
        }
        </script>
    </body>
</html>
```
在早先的范例中，我们在源文件头部添加了Firebase源文件的引用。在body部分，我们添加了一个按钮，点击此按钮的时候将会弹出提醒，此时用户可以输入他的名字。
用户输入名字完成之后，数据将会以数组的形式保存到Firebase默认的集合中。一旦数据保存完成，*ref.on('value')*事件将会被触发。
一旦触发回调，我们就会清除页面上的HTML元素，重新使用*setName*方法展示名字列表。  
你也可以打开一个新的标签页打开相同页面。默认之前添加的值都会展示出来。你这边不用做任何事情。你可以多添加些数据观察这两个页面的数据同步。  
之前范例展示了一个实时数据存储是如何工作的。现在你可以看到Firebase对于我们的聊天应用如何简单易用。
> 当用户输入用户名的时候，我们不直接展示这个值。我们等待他被存放到数据存储中，然后我们等待Firebase调用value事件。在value回调里，我们给用户展示了value。
参考此处理解数据实时更新：https://<your-firebase-url>.firebaseio.com

如果在Firebase上导航到你的app的时候，你可以看到：  
![app on Firebase](imgs/chapter-8-0.png 'app on Firebase')
  
所有添加的名字都将直接在你的app名字下展示出来。  
