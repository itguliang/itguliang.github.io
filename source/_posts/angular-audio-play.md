---
title: Ionic1 Angular1 audio 模仿微信公众号音频播放
date: 2018-09-30
categories:
  - IT
tags:
  - Ionic
  - Angular
copyright: true
abbrlink: e8054204
---
实现效果：
{% qnimg e8054204.gif %}

Html：
```html
<mp3-play x-source="data"></mp3-play>
```

mp3.html:
```html
<div class="audio-play">
    <span class="play-btn" ng-class="{playing:audioData.playing}" role="button" ng-click="play()"></span>
    <div class="right">
        <strong class="audio-title">{{source.title}}</strong>
        <div class="audio-desc">{{source.desc}}</div>
        <div class="audio-progress range">
            <input type="range" name="volume" value="0" min="0"
            max="{{source.voiceFileTime}}" ng-model="audioData.currentTime" ng-change="seeking()">
            <p class="rang-width"></p>
        </div>
        <div class="audio-time">
            <em class="current">{{audioData.currentTime | audioTime:source.voiceFileTime}}</em>
            <em class="total">{{source.voiceFileTime | audioTime:source.voiceFileTime}}</em>
        </div>
    </div>
    <audio controls ng-src="{{source.voiceFilePath}}"></audio>
</div>
```

对应指令代码：
```javascript
.filter('audioTime', function() {
  return function(second,chapterTime) {
    var t="";
    if(second > -1){
      var chaptHour = parseInt(chapterTime/3600);
      var hour = parseInt(second/3600);
      var min = parseInt(second/60) % 60;
      var sec = parseInt(second % 60);
      if(hour < 10 && chaptHour>0) {
        t = '0'+ hour + ":";
      } else if(hour>=10){
        t = hour + ":";
      }

      if(min < 10){t += "0";}
      t += min + ":";
      if(sec < 10){t += "0";}
      t += sec;
    }
    return t;
  }
})
.directive('mp3Play', function () {
  return {
    restrict: 'EA',
    templateUrl: 'templates/mp3.html',
    scope: {
      source: '='
    },
    link: function (scope, element) {

      // 初始化变量和数据成员
      var audio = element.find('audio')[0]; // 获取视频元素
      scope.audioData = {
        playing: false, // 控制是否播放中
        currentTime: 0, // 当前播放进度
        isPlayed: false
      };

      // 播放与暂停
      scope.play = function () {
        console.log("播放/暂停");
        if (!scope.audioData.playing) {
          audio.autoplay = scope.audioData.playing = true;
          audio.play();
        } else {
          audio.autoplay = scope.audioData.playing = false;
          audio.pause();
        }
      };

      // 滑动功能实现
      scope.seeking = function () {
        console.log("seaking");
        audio.currentTime = scope.audioData.currentTime;
      };

      // 音频播放位置发生改变时触发
      ionic.EventController.on('timeupdate', function () {
        scope.audioData.currentTime = audio.currentTime;
        var width = (100 * scope.audioData.currentTime / scope.source.voiceFileTime) + "%";
        document.querySelector('.rang-width').style.width = width;
        scope.$apply();
      }, audio);

      // 播放完成
      ionic.EventController.on('ended', function () {
        scope.audioData.playing = false;
        scope.audioData.currentTime = scope.source.voiceFileTime
        scope.$apply();
      }, audio);

    }
  }
})
```
audio监听其他事件可参考文章：[Html5 Audio 和 Video 属性事件汇总](https://itguliang.github.io/post/adf0756b.html)
