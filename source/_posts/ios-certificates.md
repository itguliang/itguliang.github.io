---
title: IOS申请证书到发布常见问题
date: 2019-02-01 00:00:00
categories:
  - IT
abbrlink: 6aef7dde
---
待整理 待整理 待整理…………

<!-- ### 疑惑
什么是App ID？Explicit/Wildcard App ID有何区别？什么是App Group ID？
什么是证书（Certificate）？如何申请？有啥用？
什么是Key Pair（公钥/私钥）？有啥用？与证书有何关联？
什么是签名（Signature）？如何签名（CodeSign）？怎样校验（Verify）？
什么是（Team）Provisioning Profiles？有啥用？
Xcode如何配置才能使用iOS真机进行开发调试？
多台机器如何共享开发者账号或证书？
遇到证书配置问题怎么办？
Xcode 7免证书调试真机调试 -->

### 一般步骤：

部分常用证书
  开发证书：app development(开发和真机调试，有效期1年)，push development(调试Apple Push Notification，有效期1年)
  发布证书： app store证书，ad hoc证书(有效期3年)， push Production(发布时使用的push证书，有效期1年)

1、申请app ID （将项目中的ID向苹果申请）
2、申请请求证书，导入钥匙串（这个就是允许在Mac上签名的作用）
3、生成Development证书 （作用是真机调试、发包测试）
4、生成Distribution证书 （作用是提交到appStore）
5、申请Development描述文件（作用是输入UDID绑定设备、配置第7步）
6、申请Distribution描述文件（作用是配置第7步）
7、在项目General、Built Settings中配置好证书
8、上传到appStore

{% qnimg 6aef7dde-1.png %}

Certificates是app打包，开启苹果相关服务所需的证书，是让开发者使用的设备（也就是你的Mac）有真机调试，发布APP的权限。

Provisioning Profiles 包含了App可以运行的设备，开发者打包所需要的各种证书(push证书，app打包证书等)，也就是包含了App ID和Devices，是让开发者的项目（APP）能有真机调试，发布的权限。

### 常见问题及解决
- Xcodde Unable to install "XXX". 
The certificate used to sign “appName” has either expired or has been revoked.An updated certificate is required to sign and install the application.
{% qnimg 6aef7dde-2.png?20190219 %}
<!-- 解决：Xcode -> Preferences -> Accounts -> View Details  然后分成上下两栏，上栏全部Reset一下，下栏左下角 Download All即可解决上述问题 -->

- Signing certificate is invalid.
Signing certificate "iPhone Developer: XXXXX", serial number "XXXXXXXX", is not valid for code signing. It may have been revoked or expired.
{% qnimg 6aef7dde-3.png %}

