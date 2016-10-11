## 更新config.xml文件
我们已经知道，*config.xml*是Cordova API在生成平台特定安装包的时候唯一信任的真理。
因此，在开始部署之前一定要验证好此文件。可以根据以下检查列表来检查是否正常：
* Widget ID是否定义与有效
* widget 版本是否定义且有效
* 未防应用更新，更新widget版本并验证是否有效
* name标签已定义且有效
* description已定义且有效
* 作者（Author）信息已定义且有效
* Access标签已定义且限制到所需的域名：https://github.com/apache/cordova-plugin-whitelist#network-requestwhitelist
* 允许跳转的连接已定义好且限制到所需的域名：https://github.com/apache/cordova-plugin-whitelist#intent-whitelist
* 交叉检查（Cross=check）偏好设置
* 交叉检查图标和截屏路径
* 交叉检查许可（如果有的话）
* 更新*index.html*的内容安全策略元标签https://github.com/apache/cordova-plugin-whitelist#content-securitypolicy

验证完上面的点之后，我们就可以开始生成安装包了。
  
  
> 关于本章，你也可以通过以下Github目录来访问源代码，发起issue，与作者沟通:
https://github.com/learning-ionic/Chapter-9