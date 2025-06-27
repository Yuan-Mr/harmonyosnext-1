各位开发者朋友好！本文将详细讲解如何基于HarmonyOS ArkTS框架集成华为AppGallery Connect（AGC）认证服务，涵盖从项目创建到SDK集成全流程。无论您是首次接入AGC服务，还是需要优化现有流程，本文均可提供完整指引。

一、开发流程详解
1. 创建项目与应用​​
​​作用​​：项目是AGC资源的组织实体，支持同一应用的多平台版本（如手机、平板）集中管理。

​​场景建议​​：

通过创建不同项目区分测试环境与生产环境。
每个项目可独立管理不同版本的认证服务配置。
​​2. 开通认证服务​​
登录AGC控制台，进入目标项目，在「构建 > 认证服务」页面启用所需认证方式（如手机、邮箱、华为账号等）。
​​3. 获取agconnect-services.json文件​​
​​操作路径​​：AGC控制台 → 项目设置 → 应用配置 → 下载配置文件。
​​作用​​：该文件包含应用与AGC服务通信的必要密钥和配置信息。
​​4. 集成SDK​​
​​核心依赖​​：AGC SDK + 认证服务SDK。

​​详细步骤​​：

配置HarmonyOS工程依赖（见下文「集成SDK」章节）。
初始化SDK并添加网络权限。
​​5. 实现账号登录认证​​
​​支持方式​​：

​​标准登录​​：手机、邮箱、华为账号、自有账号、匿名账号。

​​高级功能​​：

​​关联账号​​：支持多账号体系关联同一用户身份。
​​匿名转正​​：匿名用户升级为实名账号。
​​6. 登出​​
​​功能说明​​：

清除本地用户信息及Token。
适用场景：用户切换账号或临时退出登录。
​​7. 销户​​
​​安全要求​​：

用户需主动发起注销，确保符合隐私合规要求。
销户后，AGC侧用户数据将被永久删除。
二、集成SDK全流程
​​前提条件​​
​​开发工具​​：DevEco Studio 5.0.3.100+

​​SDK版本​​：

Compile SDK Version ≥ 12
Compatible SDK Version ≥ 12
​​1. 添加应用配置文件​​
将agconnect-services.json拷贝至工程目录：

AppScope/resources/rawfile/  
​​注意​​：若rawfile目录不存在，需手动创建。
​​2. 配置SDK依赖​​
​​方式一：通过oh-package.json5​​

在应用级oh-package.json5中添加依赖：

"dependencies": {  
  "@hw-agconnect/auth": "^1.0.4"  
}  
点击右上角 ​​Sync Now​​ 同步配置。

​​方式二：命令行安装​​

进入entry目录执行命令：

ohpm install @hw-agconnect/auth  
​​3. 初始化SDK​​
在EntryAbility.ets的onCreate中初始化：

import auth from '@hw-agconnect/auth';  

onCreate(want, launchParam) {  
  // 读取配置文件  
  let file = this.context.resourceManager.getRawFileContentSync('agconnect-services.json');  
  let json: string = buffer.from(file.buffer).toString();  
  // 初始化AGC SDK  
  auth.init(this.context, json);  
}  
​​添加网络权限​​：
在module.json5中声明：

"requestPermissions": [  
  { "name": "ohos.permission.INTERNET" }  
]  
​​4. 手动设置Client ID/Secret（可选）​​
​​适用场景​​：配置文件未包含密钥时（下载时勾选“不包含密钥”）。

​​操作步骤​​：

在AGC控制台「项目设置 > 常规」获取Client ID和Secret。

初始化后补充参数：

auth.setClientId("xxx");  // 替换为实际值  
auth.setClientSecret("xxx");  
​​5. 配置混淆规则​​
​​规则文件​​：entry/obfuscation-rules.txt

​​添加内容​​：

-keep  
XXX/oh_modules/@hw-agconnect/auth  
​​路径说明​​：XXX为SDK在oh_modules中的实际路径（如entry/oh_modules）。
三、结尾总结
通过本文，您已完成AGC认证服务的HarmonyOS ArkTS集成流程。后续可结合业务需求扩展登录方式（如第三方社交账号），并通过AGC控制台监控用户行为数据。如果在实践中遇到问题，欢迎访问华为开发者论坛或AGC官方文档获取技术支持。

如果有其他想了解的功能，欢迎在评论区留言告诉我！咱们下期见~ 👋