---
title: Cordova 二维码扫描插件 cordova-plugin-qrscanner
date: 2018-06-01
categories:
  - IT
tags:
  - Cordova
copyright: true
abbrlink: 80eb106e
---

二维码扫描插件：
phonegap-plugin-barcodescanner(网上一搜都是用的这个，因原生界面很丑弃之)
cordova-plugin-qrscanner(推荐，可自定义界面，很多接口可用，可按需定制)

## cordova-plugin-qrscanner（推荐）

插件地址：https://github.com/bitpay/cordova-plugin-qrscanner

### 安装

有人反馈说插件在 Android 7.0.0不能用，我没试过，我的platform为：`android 6.3.0` , `ios 4.3.1 `

``` bash
cordova plugin add cordova-plugin-qrscanner
```

### Element duplicated with element declared Validation failed

跟原来项目的有点冲突，build报错

:processDebugManifest  .../platforms/android/AndroidManifest.xml:65:5-90 Error:
Element uses-permission#android.permission.CAMERA at AndroidManifest.xml:65:5-90 **duplicated with element declared at** AndroidManifest.xml:64:5-65   .../platforms/android/AndroidManifest.xml Error:**Validation failed**
:processDebugManifest FAILED
FAILURE: Build failed with an exception.
Execution failed for task ':processDebugManifest'.
Manifest merger failed with multiple errors, see logs

{% qnimg 80eb106e-err1.png %}

解决：
1.修改 `.../platforms/android/android.json` ，把**"AndroidManifest.xml"**里对应的冲突删掉

``` xml
<!-- 删掉这句 -->
{
  "xml": "<uses-permission android:name=\"android.permission.CAMERA\" />",
  "count": 1
}
```

2.修改.../platforms/android/AndroidManifest.xml里把对应的冲突删掉

``` xml
<!-- 下面三个是插件的 -->
<!-- <uses-permission android:name="android.permission.CAMERA"/> 删掉-->
<uses-permission android:name="android.permission.CAMERA" android:required="false" />
<uses-feature android:name="android.hardware.camera" android:required="false" />
<uses-feature android:name="android.hardware.camera.front" android:required="false" />
```

### 使用

``` javascript
QRScanner.prepare(onDone); 
QRScanner.scan(callback);//扫描二维码
QRScanner.show();
// Make the webview transparent so the video preview is visible behind it.
// Be sure to make any opaque HTML elements transparent here to avoid
// covering the video.
<!-- 划重点transparent，原来插件都告诉了，搞得我调试了很久没反应，
后来脑洞大开自己调试发现了要transparent，可把我给厉害坏了，哈哈哈 -->

QRScanner.disableLight(callback);//关闭灯光
QRScanner.enableLight(callback);//开启灯光

QRScanner.useFrontCamera(callback);//前置摄像头
QRScanner.useBackCamera(callback);//后置摄像头

QRScanner.openSettings();//设置
```
具体使用请查看文档

### 模拟二维码扫描代码

需要一个半透明界面中间一个透明的扫描框，最终解决方案是用border

部分源码：

html:
``` html
<div class="qrcode-scan-div">
  <div class="text">对准二维码到框内即可扫描</div>
  <div class="arrow"></div>
  <div class="line"></div>
  <div class="lighting" ng-click="lighting()">
    <i class="icon" ng-class="{'ion-ios-lightbulb':isLighting,'ion-ios-lightbulb-outline':!isLighting}"></i>
  </div>
</div>
```

css:
``` css
<!-- % 是相对于父元素的大小设定的比率,vw (viewport width) vh (viewport height) 
    是视窗的大小,100vw = 100%视窗宽度 100vh = 100%视窗高度，用vw，vh设定的大小
    只和视窗大小有关,暂时没测兼容性如何,请自行百度 -->

<!-- 头部导航44px，250px的扫描区域 下面是计算方法，自己脑补 -->
.qrcode-scan-div{
    position: relative;
    width: 100%;
    height: 100%;
    border: solid rgba(0, 0, 0, 0.5);
    border-top-width: calc(50vh - 169px);
    border-bottom-width: calc(50vh - 125px);
    border-left-width: calc(50vw - 125px);
    border-right-width: calc(50vw - 125px);
}
//.arrow和.line的样式可参考下面（二维码扫描四个角+扫描线demo）的调整
```

二维码扫描四个角+扫描线demo：
可将整段代码复制到 http://www.runoob.com/try/try.php?filename=trycss3_animation2 运行一下
 

``` html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title> 
<style>
.rect {
    position: relative; 
    width: 100px; 
    height: 100px; 
    background: linear-gradient(to left, #f00, #f00) left top no-repeat, 
                linear-gradient(to bottom, #f00, #f00) left top no-repeat, 
                linear-gradient(to left, #f00, #f00) right top no-repeat,
                linear-gradient(to bottom, #f00, #f00) right top no-repeat, 
                linear-gradient(to left, #f00, #f00) left bottom no-repeat,
                linear-gradient(to bottom, #f00, #f00) left bottom no-repeat,
                linear-gradient(to left, #f00, #f00) right bottom no-repeat,
                linear-gradient(to left, #f00, #f00) right bottom no-repeat;
    background-size: 1px 20px, 20px 1px, 1px 20px, 20px 1px;  
}
.line{
    position: absolute;
    left: 0px;
    height: 2px; width: 100px;
    background: red; 
    -webkit-filter: blur(2px);
    -moz-filter: blur(2px);
    -ms-filter: blur(2px);    
    filter: blur(2px); 
    animation: myScan 2s infinite alternate;
    -webkit-animation: myScan 2s infinite alternate;
}
@keyframes  myScan{
    from { top:0px; }
    to { top: 100px; }
}
@-webkit-keyframes  myScan{
    from { top:0px; }
    to { top: 100px; }
}
</style>
</head>
<body>
<div class="rect">
  <div class="line"></div>
</div>
</body>
</html>
```
### 最终效果

{% qnimg 80eb106e-demo1.gif %}

## phonegap-plugin-barcodescanner（如果可以接受界面，也可以用）

插件地址：https://github.com/wildabeast/BarcodeScanner

### 安装

``` bash
cordova plugin add https://github.com/wildabeast/BarcodeScanner.git
```

### Execution failed for task ':mergeDebugResources' [color/transparent]

Execution failed for task ':mergeDebugResources'.
[color/transparent] .../platforms/android/res/values/colors.xml  [color/transparent] .../platforms/android/res/values/multi_image_chooser_colors.xml: Error: Duplicate resources

解决：把两个文件中相应冲突的注释掉一个


### 使用

``` javascript
//扫描二维码
$scope.scanCode = function(){
    cordova.plugins.barcodeScanner.scan(
        function (result) {
          alert("We got a barcode\n" +
            "Result: " + result.text + "\n" +
            "Format: " + result.format + "\n" +
            "Cancelled: " + result.cancelled);
        },
        function (error) {
          alert("Scanning failed: " + error);
        }
    );
}
```

效果(扫描二维码会有点拉长)：
{% qnimg 80eb106e-1.png?v=20180531 %}


