---
title: Flutter 学习
date: 2019-01-08
categories:
  - IT
abbrlink: 2d7e6751
---

## 安装

1、下载SDK https://flutter.io/docs/development/tools/sdk/archive?tab=macos#macos

2、解压安装包到你想安装的目录
```bash
$ cd ~/Documents
$ unzip ~/Downloads/flutter_macos_v0.5.1-beta.zip
```

3、添加flutter相关工具到path中：
```bash
$ export PATH=`pwd`/flutter/bin:$PATH  #临时
$ echo $PATH
```

## 运行 flutter doctor

运行以下命令查看是否需要安装其它依赖项来完成安装：
```bash
$ flutter doctor
```
## 安装过程问题

https://www.jianshu.com/p/603649a02956

```bash
[!] Android toolchain - develop for Android devices (Android SDK 26.0.2)
    ✗ Android license status unknown.
```
运行 flutter doctor --android-licenses
```bash
$ flutter doctor --android-licenses
  A newer version of the Android SDK is required. To update, run:
  /Users/xxx/xxx/sdk/tools/bin/sdkmanager --update
# 根据提示运行
$ /Users/xxx/xxx/sdk/tools/bin/sdkmanager --update
```
然后根据提示一直y，y到结束为止。

```bash
[!] iOS toolchain - develop for iOS devices (Xcode 10.1)
    ✗ libimobiledevice and ideviceinstaller are not installed. To install with
      Brew, run:
        brew update
        brew install --HEAD usbmuxd
        brew link usbmuxd
        brew install --HEAD libimobiledevice
        brew install ideviceinstaller
    ✗ CocoaPods not installed.
        CocoaPods is used to retrieve the iOS platform side's plugin code that
        responds to your plugin usage on the Dart side.
        Without resolving iOS dependencies with CocoaPods, plugins will not work
        on iOS.
        For more info, see https://flutter.io/platform-plugins
      To install:
        brew install cocoapods
        pod setup
```
按提示执行上面的命令

```bash
[✓] Android Studio (version 2.3)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[!] IntelliJ IDEA Ultimate Edition (version 2018.3.2)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
```
上面提示是编辑器没有安装插件，安装即可

```bash
[!] VS Code (version 1.30.1)
[!] Connected device
    ! No devices available
```

