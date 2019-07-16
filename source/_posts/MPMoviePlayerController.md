---
title: 优化MPMoviePlayerController 全屏播放 退出后 cordova 项目上移
date: 2018-10-26
categories:
  - IT
abbrlink: e8c41f51
copyright: true
---

IOS 横竖屏切换，头部与statusbar重叠，`config.xml` 设置如下：

```xml
<preference name="StatusBarOverlaysWebView" value="false" />
<preference name="StatusBarBackgroundColor" value="#0190dc" /> //颜色自定义
```

插件 `cordova-plugin-streaming-media` `v1.0.0`版本

（自我感觉原因是因为全屏播放退出后，整个其实还在全屏状态的感觉，以下为目前的解决办法，思路：样式同全屏一样，并不是全屏播放）

修改 `StreamingMedia.m` 文件

 `startPlayer` 方法，增改如下：

```JavaScript
//控制面板风格，枚举类型：
//MPMovieControlStyleNone：无控制面板
//MPMovieControlStyleEmbedded：嵌入视频风格
//MPMovieControlStyleFullscreen：全屏
//MPMovieControlStyleDefault：默认风格
//--改: 控制面板风格改为全屏风格
moviePlayer.controlStyle = MPMovieControlStyleFullscreen;

//--增：addSubview:moviePlayer.view之前 
[moviePlayer.view setFrame:CGRectMake(0, 0,
 self.viewController.view.frame.size.width, 
 self.viewController.view.frame.size.height)];
 
//--改: 禁用全屏  
[moviePlayer setFullscreen:NO animated:NO];
```

`orientationChanged` 方法，添加代码：

```
[moviePlayer.view setFrame:CGRectMake(0, 0,
 self.viewController.view.frame.size.width, 
 self.viewController.view.frame.size.height)];
```

