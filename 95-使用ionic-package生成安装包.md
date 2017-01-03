## Ionic package
编写本书的时候，Ionic package任务还是beta版。因此，我最后才讲他。

### 将项目上传到Ionic云
使用Ionic云服务生成安装包非常简单。首先，使用如下命令将应用上传到我们的Ionic账号：
```
ionic upload
```

> 在执行上面的命令之前先登录Ionic账号。如果你的项目有敏感信息的话，在将应用上传到云服务之前与Ionic许可证交叉对比

一旦应用上传成功，将会为你的应用生成一个app ID。你可以在项目根目录下的*ionic.project*中找到app ID。

### 生成所需密钥
按照Android安装包部分的*第五步*，生成*keystore*文件。  
接下来，我们使用*ionic package*命令来生成安装包，如下：
```
ionic package <options> [debug | release] [ios | android]
```
有如下可用选项：  
![ionic package options](imgs/chapter-9-1.png 'ionic package options')

例如，如果你想生成一个Android发布版：
```
ionic package release android -k app-name-release-key.keystore -a myionic-app -w 12345678 -r 12345678 -o ./ -e arvind.ravulavaru@gmail.com -p 12345678
```

> 我们在*deploy-keys*文件夹内运行此命令

同样，对于iOS：
```
ionic package release ios -c certificate-file -d password -f profilefile -o ./ -e arvind.ravulavaru@gmail.com -p 12345678
```

> *ionic package*命令在Ionic CLI 1.5.2中已经移除。相关信息参考： https://github.com/driftyco/ionic-cli/issues/214#issuecomment-109349399


# 总结
完成这些之后，我们的Ionic之旅就正式结束了。快速总结一下，我们从了解为何使用AngularJS开始。
然后，我们学习了移动混合应用如何运行，Cordova和Ionic可以用在哪里。
接下来，我们学习了大量的Ionic模板然后快速浏览了Ionic CSS组件，Ionic指令以及服务。
我们利用这些知识为一个secure REST API制作了一个Ionic客户端。
接着，我们学习了Cordova和ngCordova，以及如何使用它们。
我们利用Ionic与Cordova的长处制作了一个聊天应用。
最后，我们学习如何为制作好的应用生成安装包并发布到app store。
