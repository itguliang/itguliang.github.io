---
title: Cordova 项目适配 iPhone X 
date: 2018-10-01
categories:
  - IT
abbrlink: 8072bd2e
---

界面在中间没有全屏显示

`config.xml` 增加iPhoneX用的启动图（1125*2436）可以让画面充满屏幕，要重现build

```xml
<platform name="ios">
  <splash height="2436" src="resources/ios/splash/1125_2436.png" width="1125" />
</platform>
```

或者，xcode `项目/Resources/Images.xcassets/LaunchImage` 直接往iPhoneX位置处拖放入iPhoneX的1125 * 2436尺寸的启动图片，如果LauchImage中没有iPhoneX型号，可以右键LauchImage，Show in Finder，进入LaunchImage.launchimage文件夹，修改Contents.json文件，然后再拖入

Contents.json:
```json
{
  "extent" : "full-screen",
  "idiom" : "iphone",
  "subtype" : "2436h",
  "filename" : "1125_2436.png",
  "minimum-system-version" : "11.0",
  "orientation" : "portrait",
  "scale" : "3x"
}
```

剩下的内页微调，safe-area-inset-top和safe-area-inset-bottom 设定具体的安全区域


