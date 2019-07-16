---
title: CodePush 热更新接入教程
date: 2018-12-22
categories:
  - IT
abbrlink: bdc17ac0
toc: true
---
微软开源，图像化界面：https://www.appcenter.ms/
文档：https://www.npmjs.com/package/code-push-cli


## 安装

```bash
$ npm install -g code-push-cli # 首先要安装Node.js
$ code-push -v #2.1.9
```

## 注册账号

```bash
$ code-push register
#Please login to Mobile Center in the browser window we've just opened.
#Enter your token from the browser: 
```
会自动打开登录界面，用GitHub登录
{% qnimg bdc17ac0-1.png %}

登录后自动打开下面界面，按提示复制token
{% qnimg bdc17ac0-2.png %}

将token粘贴到命令行
```bash
$ code-push register 

  Please login to Mobile Center in the browser window we've just opened.
  Enter your token from the browser:  xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

  Successfully logged-in. Your session file was written to .../.code-push.config. 
  You can run the code-push logout command at any time to delete this file and terminate your session.
```

我们使用下面的命令来验证我的登录是否成功
```bash
$ code-push login #如果登录了，会提示 [Error]  You are already logged in from this machine.
```

CodePush注册登录相关命令：
```bash
$ code-push whoami
$ code-push login  #登陆
$ code-push logout #注销
$ code-push access-key ls #列出登陆的token
$ code-push access-key rm #删除某个 access-key
```

## 注册App
在CodePush服务器注册App
```bash
$ code-push app add <appName> <os> <platform> 
# 例子：
# code-push app add MyApp-iOS ios react-native     添加 iOS(os)、react-native(platform) 应用
# code-push app add MyApp-Android android cordova  添加 android(os)、cordova(platform) 应用
```

图形化界面：
{% qnimg bdc17ac0-3.png %}

CodePush管理App的相关命令：
```bash
$ code-push app remove/rm <appName> #在账号里移除一个app
$ code-push app rename <appName> <newAppName>     #重命名一个存在app
$ code-push app list/ls    #列出账号下面的所有app
$ code-push app transfer <appName> <newOwnerEmail> #把app的所有权转移到另外一个账号
$ code-push collaborator add <appName> <collaboratorEmail> #添加协作者
$ code-push collaborator rm <appName> <collaboratorEmail>  #删除协作者
$ code-push collaborator ls <appName> #列出应用下列出所有的参与者
```

开发环境管理:
```bash
$ code-push deployment add <appName> <deploymentName> #增加开发环境
$ code-push deployment rm <appName> <deploymentName>  #移除开发环境
$ code-push deployment rename <appName> <deploymentName> <newDeploymentName>  #开发环境换名字
$ code-push deployment ls <appName>  #列出所有的开发环境
$ code-push deployment ls <appName> --displayKeys/-k     #列出所有的开发环境和对应access-key
$ code-push deployment clear <appName> <deploymentName>  #清除更新记录 
```


## React-Native集成 

```bash
$ npm install react-native-code-push --save
$ react-native link react-native-code-push
```



## Cordova集成 

cordova-plugin-code-push: https://github.com/Microsoft/cordova-plugin-code-push

(网上很多用这个https://github.com/nordnet/cordova-hot-code-push，可以试试)

- Android (cordova-android 4.0.0+)
- iOS (cordova-ios 3.9.0+) 

<!-- $ cordova plugin add cordova-hot-code-push-plugin #添加cordova-hot-code-push插件
$ cordova  plugin add cordova-hot-code-push-local-dev-addon #添加用于本地开发的插件
$ npm install -g cordova-hot-code-push-cli  #安装Cordova Hot Code Push CLI客户端 -->
安装：
```bash
$ cordova plugin add cordova-plugin-code-push@latest
```

```bash
$ code-push release-cordova <appName> <platform> [options]
# 选项：
#   --build, -b                 [布尔-默认:false] Invoke "cordova build" instead of "cordova prepare"
#   --isReleaseBuildType, --rb  [布尔-默认:false] If "build" option is true specifies whether perform a release build
#   --deploymentName, -d        [字符串-默认:"Staging"] Deployment
#   --description, --des        [字符串-默认:null] 描述
#   --disabled, -x              [布尔-默认:false] Specifies whether this release should be immediately downloadable
#   --mandatory, -m             [布尔-默认:false] Specifies whether this release should be considered mandatory
#   --privateKeyPath, -k        Specifies the location of a RSA private key to sign the release with  [字符串] [默认值: false]
#   --noDuplicateReleaseError   [布尔-默认:false] When this flag is set, releasing a package that is identical to the latest release will produce a warning instead of an error
#   --rollout, -r               Percentage of users this release should be immediately available to  [字符串] [默认值: "100%"]
#   --targetBinaryVersion, -t   [字符串-默认:null] Semver expression that specifies the binary app version(s) this release is targeting (e.g. 1.1.0, ~1.2.3). If omitted, the release will target the exact version specified in the config.xml file.
#   -v, --version               显示版本号  [布尔]
```



更多命令详见：https://github.com/Microsoft/code-push/tree/master/cli

<!-- 

https://www.jianshu.com/p/6a5e00d22723 

https://www.cnblogs.com/huangenai/p/7137475.html

https://blog.csdn.net/jiangbo_phd/article/details/52692320

-->