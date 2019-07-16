---
title: Xcode archive 打包ipa过程图解
date: 2018-06-04
categories:
  - IT
abbrlink: 550b8caa
---

导入iOS证书p12到钥匙串，双击p12文件，登录，导入证书

#### 1.选择Generic iOS Device，选择其他模拟器是不能Archive的

{% qnimg 550b8caa-1.png?2019 %}

#### 2.Xcode 工具条 Product 下点击 Archive

{% qnimg 550b8caa-2.png %}

#### 3.如果弹出下面框，输入本机密码，始终允许

{% qnimg 550b8caa-3.png %}

#### 4.点击Export

{% qnimg 550b8caa-4.png %}

#### 5.点击Enterprise

{% qnimg 550b8caa-5.png %}

#### 6.点击Next

{% qnimg 550b8caa-6.png %}

#### 7.选择证书,点击Next

{% qnimg 550b8caa-7.png %}

#### 8.最后点击导出，选择指定位置即可

{% qnimg 550b8caa-8.png %}

