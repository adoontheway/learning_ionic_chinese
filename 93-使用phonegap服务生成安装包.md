## PhoneGap服务
第一个探索的应用安装包生成方式是使用PhoneGap构建服务。这应该是最简单的Android和iOS安装包生成方式了。  

流程非常简单。我们将整个项目上传到PhoneGap构建服务他就会负责构建安装包了。
  
> 如果你觉得上传整个项目不切实际，你可以只上传*www*文件夹；但是，需要做以下变更：
首先，将*config.xml*移动到*www*文件夹内。
接着，将*resources*文件夹移动到*www*文件夹内。
最后，修改*config.xml*里面的*resources*文件夹的路径。
如果你发现你经常干这件事情的话，建议你写一个搅拌来生成一个*PhoneGap deployable*（可部署）文件夹，用来容纳变更后的项目。

如果你打算只发布Android版本，你就不用再做别的操作了。
如果你打算生成iOS安装包，那么你就需要一个Apple Developer Account（苹果开发者账号）并遵照步骤生成所需证书： http://docs.build.phonegap.com/en_US/signing_signing-ios.md.html

> 你也可以按照这里来给对你的Android应用进行签名： http://docs.build.phonegap.com/en_US/signing_signing-android.md.html

一旦得到证书和密钥，你就可以开始生成安装包里。按照以下步骤：
1. 新建一个PhoneGap账号并登录：https://build.phonegap.com/plans
2. 接下来，导航到 https://build.phonegap.com/people/edit ，选择Siging Keys标签页，上传iOS和Android证书
3. 然后，导航到  https://build.phonegap.com/apps ，点击*New App*。
作为免费计划的一部分，在他们还从Pulic Git资源目录拉取的时候，你可以拥有任意多数量的app。
你也可以通过Private repo（资源目录）或者上传一个ZIP文件来创建一个私有应用。
4. 为测试此服务，我们来创建一个*.zip*文件（不是*.rar*或者*.7z*），遵照如下文件夹结构：
 * App（根文件夹）
   config.xml
   resources（文件夹）
   www（文件夹）
   然后，如前所述，更新config.xml
5. 上传ZIP文件到  https://build.phonegap.com/apps 点击 Create app

  
将花费数分钟来完成生成流程。
> 有时候，构建服务会出现错误。等待之后再重试。根据构建服务器的负载，构建流程有时候会比预想的更长。