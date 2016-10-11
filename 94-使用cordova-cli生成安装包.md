## 使用Cordova CLI生成安装包
现在，我们将使用Cordova CLI来生成Android和iOS安装包。

### Android安装包
首先来看Android安装包的生成。步骤如下：
1. 在项目的根目录下打开终端/命令行
2. 移除不需要的插件：** ionic plugin rm cordova-plugin-console **
3. 以发布模式构建应用：** cordova build --release android **。
将会在/platforms/android/build/outputs/apk/android-release-unsigned.apk生成一个未签名的安装包
4. 接下来，我们需要制作一个签名密钥。如果已经有了的话或者你是更新一个已有的app的话，可以直接进行到*第六步*
5. 使用密钥工具生成私有密钥。
创建一个文件夹名为*deploy-key*，所有的密钥都将存放在此。
文件夹创建好了之后，通过*cd*命令进入到此文件夹，然后运行：  
** keytool -genkey -v -keystore app-name-release-key.keystore -alias alias_name -keyalg RSA -keysize 2048 -validity 10000 **  
然后你将被问到如下问题，你可以如下回答：  
![key tool](imgs/chapter-9-0.png 'key tool')
> 如果你丢失了此文件或者忘记别名或者密码的话，你将永远不能像app store提交更新了，永远。

6. 可选步骤：你可以将*android-release-unsigned.apk*拷贝到*deploy-key*文件夹，然后运行在其中运行下面的命令。但是，我还是他这些文件留在他们原先的位置。
7. 接下来，使用*jarsigner*工具，给未签名的APK签名：
** jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -
keystore app-name-release-key.keystore
../platforms/android/build/outputs/apk/android-releaseunsigned.apk my-ionic-app **
这个过程将会问到你密码，也就是创建keystore第一步输入的。一旦签名流程完成，*android-release-unsigned.apk*将被同名的签名版替换掉。
> 以上命令在*deploy-keys*文件夹内运行

8. 最后，我们运行*zipalign*来优化APK。
** zipalign -v 4 ../platforms/android/build/outputs/apk/androidrelease-unsigned.apk my-ionic-app.apk **

  
以上命令将会在*deploy-keys*里面创建一个*my-ionic-app.apk*。  
现在，你可以将APK投放到app store了。

### iOS安装包
接下来，我们将使用Xcode来为iOS生成安装包。执行如下步骤即可：
1. 在项目根目录下打开命令行/终端
2. 移除不需要的插件： ** ionic plugin rm cordova-plugin-console **
3. 运行：** ionic build –release ios **
4. 导航到 *platform/ios* 文件夹，使用Xcode启动*projectname.xcodeproj*
5. 一旦项目导入Xcode完成，选择了**iOS Device**之后，从导航菜单中选择**Product**，然后**Archive**
> 如果**Archive**选项没有激活，参考：http://stackoverflow.com/a/18791703

6. 接下来，在导航菜单中选择**Window**然后选择**Organizer**。你将会看到一系列创建好的结构体。
7. 点击你现在已经创建好的结构体的缩略图，点击**Submit to App Store**。将会验证你的账户然后应用将上传到Apple Store。
8. 最后，登录Apple Store设置截屏，描述，等等。