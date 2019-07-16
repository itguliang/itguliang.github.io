---
title: Cordova 常用的插件汇总
date: 2018-05-19
categories:
  - IT
tags:
  - Cordova
copyright: true
abbrlink: 4493d05e
---
### 打开外部链接：cordova-plugin-inappbrowser

地址：https://github.com/apache/cordova-plugin-inappbrowser


### 分享到微信：cordova-plugin-wechat

地址：https://github.com/xu-li/cordova-plugin-wechat

安装：

```bash
cordova plugin add cordova-plugin-wechat --variable wechatappid=YOUR_WECHAT_APPID
//把YOUR_WECHAT_APPID替换成自己申请的微信APPID
```


### 分享到QQ：cordova-plugin-qqsdk

地址：https://github.com/iVanPan/Cordova_QQ
0.8.0版本地址：https://github.com/iVanPan/Cordova_QQ/releases/tag/0.8.0

安装：

```bash
cordova plugin add cordova-plugin-qqsdk --variable QQ_APP_ID=YOUR_QQ_APPID
//把YOUR_QQ_APPID替换成自己申请的QQ APPID
//IOS安装完插件,用 Xcode 打开工程,查看 URL Types 里面 QQ 的 URL Type 有没有，如果没有手动添加
```

注意：
1、新版需要 `cordova-android 7.0.0` ，如果不升级 `cordova-android 7.0.0` 的话可以下载 `0.8.0` 到本地再安装
2、分享的description不能为null否则会闪退


### 分享到微博：cordova-plugin-weibosdk

地址：https://github.com/iVanPan/cordova_weibo


### 图片预览：cordova-plugin-ImagePicker

地址：https://github.com/giantss/cordova-plugin-ImagePicker

--->功能：相册目录、多选图、相册内部拍照、预览选中的图片、图片压缩


### 二维码扫描：cordova-plugin-qrscanner

地址：https://github.com/bitpay/cordova-plugin-qrscanner

--->功能：扫描二维码，可自定义界面，很多接口可调用


可参考我的另一篇文章：[Cordova 二维码扫描插件 cordova-plugin-qrscanner](https://itguliang.github.io/post/80eb106e.html)



### 保存图片到相册：cordova-plugin-photo-library

地址：https://github.com/terikon/cordova-plugin-photo-library

注意：
1、cordova-plugin-file@^5.0.0
2、相册路径不要加权限
3、使用需要先调用requestAuthorization获取到权限才可保存
4、iCloud设置了照片共享上传照片流等保存经常提示出错Retrieved asset is nil 
解决：笨方法重复调用保存图片，超过5次再提示保存失败

### 复制到剪贴板

https://github.com/VersoSolutions/CordovaClipboard
https://github.com/danielsogl/cordova-plugin-clipboard

### 设备震动：cordova-plugin-vibration 

https://cordova.apache.org/docs/en/latest/reference/cordova-plugin-vibration/index.html


### 录音录视频：cordova-plugin-media-capture

https://cordova.apache.org/docs/en/latest/reference/cordova-plugin-media-capture/index.html

https://github.com/apache/cordova-plugin-media-capture#readme


-----
分享插件整理参考：https://blog.csdn.net/jcy472578/article/details/50718951