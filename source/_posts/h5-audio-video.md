---
title: Html5 Audio 和 Video 属性事件汇总
date: 2018-07-01
categories:
  - IT
tags:
  - Html/Css
abbrlink: adf0756b
---
### audio
- autoplay：自动播放
- controls：显示控件
- loop：循环播放
- preload：音频在页面加载时进行加载
- src：音频链接

html:
``` html
<audio id="media" src="http://.../test.mp3" controls></audio>  
```
获取音频对象
``` javascript
//audio可以直接通过new创建对象
myMedia = new Audio("http://.../test.mp3");
//audio和video都可以通过标签获取对象
myMedia = document.getElementById("media");
```

### video
- src ：视频的属性，url地址
- poster：视频封面，没有播放时显示的图片
- preload：预加载|none|metadata（部分预加载）|auto。默认为auto
- autoplay：自动播放
- loop：循环播放
- controls：浏览器自带的控制条
- width：视频宽度
- height：视频高度

其中： src地址可控制播放区域（调取native播放器（qq,uc等浏览器）的除外） 
【示例】 http://xxxx.com/video.mp4#t=10,20   表示播放范围10s-20s;
t=,02:00:00 从开始到两小时;t=60 从60s开始;t=,10.5 开始到10.5s

html:
``` html
<video id="media" src="http://.../test.mp4" controls></video>  
```

获取video对象:
``` javascript
myMedia = document.getElementById("media"); 
```

## HTMLVideoElement和HTMLAudioElement 均继承自HTMLMediaElement

### Media 方法
```javascript
myMedia.canPlayType();//检测浏览器是否能播放指定的音频/视频类型。
// 'probable'：浏览器最可能支持该类型
// 'maybe'：可能支持
// ''：不支持
myMedia.load();      //重新加载，用于更改src之后使用，无参数，无返回值
myMedia.play();      //播放
myMedia.pause();     //暂停
```

### Media 属性

```javascript
myMedia.src = value; //返回或设置当前资源的URL
myMedia.currentSrc;  //返回当前资源的URL
myMedia.canPlayType(type);  //是否能播放某种格式的资源

myMedia.controls;    //是否有默认控制条
myMedia.autoPlay;    //是否自动播放
myMedia.loop;        //是否循环播放 默认为false
myMedia.ended;       //是否结束
myMedia.paused;      //是否暂停
myMedia.seeking;     //是否正在seeking

//只有 Internet Explorer 9 支持 error 属性。
myMedia.error;       //null:正常
myMedia.error.code;  //1.用户终止 2.网络错误 3.解码错误 4.URL无效

myMedia.preload; 
// auto：一旦页面加载，则开始加载音频或视频
// metadata：当页面加载后仅加载音频或视频的元数据
// none：页面加载后不加载音频或视频

myMedia.networkState;
// 0 = NETWORK_EMPTY - 音频/视频尚未初始化
// 1 = NETWORK_IDLE - 音频/视频是活动的且已选取资源，但并未使用网络
// 2 = NETWORK_LOADING - 浏览器正在下载数据
// 3 = NETWORK_NO_SOURCE - 未找到音频/视频来源

myMedia.readyState;
// 0 = HAVE_NOTHING - 没有关于音频/视频是否就绪的信息
// 1 = HAVE_METADATA - 关于音频/视频就绪的元数据
// 2 = HAVE_CURRENT_DATA - 关于当前播放位置的数据是可用的，但没有足够的数据来播放下一帧/毫秒
// 3 = HAVE_FUTURE_DATA - 当前及至少下一帧的数据是可用的
// 4 = HAVE_ENOUGH_DATA - 可用数据足以开始播放

myMedia.playbackRate = value;        //当前播放速度，设置后马上改变
myMedia.defaultPlaybackRate = value; //默认的回放速度，可以设置
// 1：正常速度
// 0.5：半速
// 2：倍速
// -1：向后正常速度
// -0.5：向后半速

myMedia.currentTime = value; //当前播放的位置，赋值可改变位置
myMedia.startTime;           //一般为0，如果为流媒体或者不从0开始的资源，则不为0
myMedia.duration;            //当前资源长度 流返回无限
myMedia.volume = value;      //音量
myMedia.defaultMuted;        //初始是否静音，默认为false
myMedia.muted = value;       //静音 默认为false

myMedia.seekable;   //返回可以seek的区域 TimeRanges
myMedia.played;     //返回已经播放的区域，TimeRanges，关于此对象见下文
myMedia.buffered;   //返回已缓冲区域，TimeRanges
//TimeRanges(区域)对象
TimeRanges.length;       //区域段数
TimeRanges.start(index); //第index段区域的开始位置
TimeRanges.end(index);   //第index段区域的结束位置 
```

### Media 事件

```javascript
var eventTester = function(eventName){
   myMedia.addEventListener(eventName,function(){
       console.log(eventName)
   },false);
}
eventTester("loadstart");       //客户端开始请求数据
eventTester("progress");        //客户端正在下载指定的音频或视频
eventTester("loadedmetadata");  //音频或视频的元数据已加载
eventTester("loadeddata");      //音频或视频的当前帧已加载，但没有足够数据播放下一帧
eventTester("canplay");         //可以播放，但中途可能因为加载而暂停 浏览器能够开始播放指定的音频或视频
eventTester("canplaythrough");  //音频或视频能够不停顿地一直播放

eventTester("suspend");         //延迟下载 在音频或视频数据被阻止加载时触发(可以是完成加载后触发，或者因为被暂停)
eventTester("abort");           //在音频或视频终止加载时触发（不是因为错误引起）
eventTester("error");           //请求数据时遇到错误
eventTester("stalled");         //在浏览器尝试获取媒体数据，但数据不可用时触发。
eventTester("empted");          //在发生故障并且文件突然不可用时触发
eventTester("play");            //play()和autoplay开始播放时触发
eventTester("pause");           //pause()触发

eventTester("waiting");         //等待数据，并非错误 需要缓冲下一帧而停止时触发
eventTester("playing");         //音频或视频已开始播放时触发

eventTester("seeking");         //正在进行指示定位时触发
eventTester("seeked");          //指示定位已结束时触发
eventTester("timeupdate");      //播放时间改变 播放位置改变时触发(播放和调整指示定位时都会触发)
eventTester("ended");           //音频或视频文件播放完毕后触发
eventTester("ratechange");      //播放速度改变进触发
eventTester("durationchange");  //资源长度改变
eventTester("volumechange");    //音量改变时触发
```

当音频/视频处于加载过程中时，会依次发生以下事件：
1、loadstart
2、durationchange
3、loadedmetadata
4、loadeddata
5、progress
6、canplay
7、canplaythrough

## vue 实现视频播放

1.安装插件
```bash
npm install vue-video-player -S
```
2.配置插件
在main.js里
```javascript
import VideoPlayer from 'vue-video-player'
require('video.js/dist/video-js.css')
require('vue-video-player/src/custom-theme.css')

Vue.use(VideoPlayer)
```

3.使用插件

https://github.com/surmon-china/vue-video-player

----------
参考：
http://www.jianshu.com/p/404d01b8e713

https://github.com/robinma/videoplayer

https://docs.microsoft.com/en-us/previous-versions/windows/internet-explorer/ie-developer/samples/hh924822(v=vs.85)

https://developer.mozilla.org/en-US/docs/Web/API/HTMLVideoElement
https://segmentfault.com/a/1190000009395289
http://www.runoob.com/tags/ref-av-dom.html