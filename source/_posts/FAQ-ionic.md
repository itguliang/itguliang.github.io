---
title: 解决问题集锦-Ionic
date: 2018-11-28
categories:
  - IT
tags:
  - Ionic
  - FAQ
abbrlink: e44b2a33
---

### Q：注意

ionic1 controller里有let声明变量造成页面加载不了;

数组类的赋值考虑为空的时候赋为 [];


### Q：app打包成apk文件以后，如何查看VersionCode、VersionName等版本信息

使用aapt工具，aapt.exe工具 即 Android Asset Packaging Tool，在SDK的build-tools目录下。 

操作流程：
``` bash
# 1、首先找到aapt工具，在Android SDK文件夹下的build-tools包里，如下
cd xxx路径/SDK/build-tools/23.0.0_rc3

# 2、然后使用aapt dump badging XXX.apk
aapt dump badging XXX路径/XXX.apk
# 如果不管用，前面加 ./
./aapt dump badging XXX路径/XXX.apk
# 就能看到PackageName，VersionCode，LauncherActivity等信息
```

### Q：angular 页面滚动到指定位置

``` javascript
$location.hash('category');
$anchorScroll();
```


### Q：angularjs用div contenteditable 模拟输入框自动变化高度，ng-model双向数据绑定，附带placeholder效果

用textarea标签来做高度自适应输入功能有点麻烦，用div加个h5属性 `contenteditable="true"` 即可使用 `ng-model` 实现双向绑定，和高度自适应的功能。
注意：苹果手机不支持h5属性`contenteditable="true"`，需添加样式`-webkit-user-select:text`

html:
``` html
<div placeholder="请输入" contenteditable="true" ng-model="model"></div>
```
CSS:
``` CSS
div{
  -webkit-user-select:text;　
}
div:empty:before{
  content: attr(placeholder);  
  color:#bbb;
}
```
directive:
``` javascript
.directive('contenteditable', function() {
    return {
      restrict: 'A',
      require: '?ngModel',
      link: function(scope, element, attrs,ngModel) {
        if (!ngModel) {
          return;
        }
        ngModel.$render = function() {
          element.html(ngModel.$viewValue || '');
        };
        element.bind('keyup', function() {
          scope.$apply(function() {
            var html=element.html();
            ngModel.$setViewValue(html);
          });
        });
      }
    };
});
```
有个问题是光标总是定位在开头，自行百度



### Q：div 包裹 input checkbox点击问题

``` html
<!-- 点击div即checkbox或者文字都可以实现是否同意的操作 -->
<!-- 结果点击checkbox无效 -->
<div  ng-click="agree()">
  <span><input type="checkbox" ng-model="isAgree" /></span>
  <span>我同意并自愿作出以上承诺</span>
</div>
```

➣解决：

``` html
<!-- 点击div即checkbox或者文字都可以实现是否同意的操作 -->
<!-- 结果点击checkbox无效 -->
<div  ng-click="agree()">
  <span ng-click="checkboxAgree($event)">
    <input type="checkbox" ng-model="isAgree" />
    </span>
  <span>我同意并自愿作出以上承诺</span>
</div>
```
停止事件冒泡,当点击的是checkbox时,就不执行父div的click

```javascript
$scope.checkboxAgree = function(e) {
  e.stopPropagation();
};
```



### Q：$cordovaFileTransfer下载图片后系统相册无法显示图片

➣解决：
android 下载到 cordova.file.externalRootDirectory Download：
```bash
# 插件地址：https://github.com/lotterfriends/refreshgallery
$ cordova plugin add cordova-plugin-refresh-gallery

# 下载成功js中添加下面代码,相当于通知系统相册进行刷新
refreshMedia.refresh(targetPath); # Refresh the image gallery
```


### Q：You don't have write permissions for the /Library/Ruby/Gems/2.0.0 directory.

只需要在命令前加上sudo

例如:sudo gem install cocoapods

sudo是获取管理员权限,现在按照步骤输入管理员密码即可.



### Q：cordova-android@6.3.0编译遇到问题，可能需要要重新安装cordova-plugin-compat

```javascript
What went wrong:
Execution failed for task ':transformClassesWithDexForDebug'.
> 
com.android.build.api.transform.TransformException: 
com.android.ide.common.process.ProcessException: 
java.util.concurrent.ExecutionException: com.android.dex.DexException: 
Multiple dex files define Lorg/apache/cordova/PermissionHelper;
```

➣解决：

```bash
$ cordova plugin rm cordova-plugin-compat --force
$ cordova plugin add cordova-plugin-compat@1.2
$ cordova platform rm android
$ cordova platform add android@6.3.0 
```



### Q：Android端视频播放集成预览插件

Android视频播放插件:Vitamio-Cordova-Plugin

直播发送图片预览插件:cordova-plugin-ImagePicker

报错： LOAD FFMPEG ERROR: dlopen failed: /data/data/xxx.xx.com.cn/libs/libffmpeg.so: has text relocations ....

➣解决：

需要配置AndroidManifest.xml targetSdk低于23

<uses-sdk android:minSdkVersion="17" android:targetSdkVersion="22" />



### Q：angular5 使用angular-cli搭建项目ng build后，failed to load resource 文件路径问题

打包时执行：

```javascript
//--base-href后面为打包后的base路径
ng build --base-href ./  
```

或者，在package.json文件的scripts中添加命令：

```javascript
//--base-href后面为打包后的base路径
"build":"ng build --base-href ./"
```

打包的时候，执行npm run build 即可



### Q：cordova 监听后台运行切换

[后台运行 pause](http://cordova.apache.org/docs/en/latest/cordova/events/events.html#pause "pause")

[前台运行 resume](http://cordova.apache.org/docs/en/latest/cordova/events/events.html#resume "resume")



### Q：a href tel和sms ios失效问题

document.location.href = 'tel:' + mobile;

```javascript
config.xml
<access launch-external="yes" origin="tel:*" />
<access launch-external="yes" origin="sms:*" />
ios添加
<allow-navigation href="sms:*" />
<allow-navigation href="tel:*" />
```


### Q：cordova-plugin-inappbrowser插件 IOS 打开链接（jsp）loading不消失

ios-webview加载进度问题，debug看了下，正常情况应该是 `webViewDidStartLoad` --> `self.spinner startAnimating` --> `webViewDidFinishLoad` --> `self.spinner stopAnimating`。

但是项目中有个链接，打开后有跳转，不会执行`webViewDidFinishLoad`，执行了`didFailLoadWithError` --> `self.spinner stopAnimating`又执行了`webViewDidStartLoad` --> `self.spinner startAnimating`然后没反应了。

经过排除，在jsp页面onload后再跳转即可解决loading不消失问题



### Q：IOS8用户无法升级和下载或者闪退

4.4.0以上不兼容ios8

ionic platform add ios@4.3.1 



### Q：IOS8兼容问题

transform、flex-wrap需要加-webkit-前缀



### Q：Cordova升级打包后版本号versioncode由6位变为5位,导致升级后版本号低于之前版本不能安装

修改platforms>android>cordova>lib>prepare.js中function default_versionCode计算versionCode的代码

```javascript
function default_versionCode (version) {
   var nums = version.split('-')[0].split('.');
   var versionCode = 0;
   if (+nums[0]) {
     versionCode += +nums[0] * 100000;
   }
   if (+nums[1]) {
     versionCode += +nums[1] * 100;
   }
   if (+nums[2]) {
     versionCode += +nums[2] * 10;
   }
   events.emit('verbose', 'android-versionCode not found in config.xml. Generating a code based on version in config.xml (' + version + '): ' + versionCode);
   return versionCode;
}
```


### Q： ionic-v1 $ionicScrollDelegate scroll methods don’t work on iOS

登录界面，键盘弹起，界面向上滚动`scrollTop()`，键盘收起，界面恢复`scrollTo()`。结果ios不起作用，Android可以，ios下看了效果，`transform: translate3d(0px, valuepx, 0px) scale(1)` 中value根本不是自己设置的值

➣解决：
向上滚动的css：
``` CSS
<!-- 值自己设置 -->
.login-scroll .scroll{
  transform: translate3d(0px, -157px, 0px) scale(1) !important;
}
```
html:
``` html
<ion-content ng-class="{'login-scroll':loginScroll}"></ion-content>
```
监听键盘弹起与收起：
``` JavaScript
$scope.loginScroll=false;//默认为false

function keyboardshow(e) {
  $scope.loginScroll=true;
  // $ionicScrollDelegate.scrollTo(0, 155, true);//ios不管用
}
function keyboardhide(e) {
  $scope.loginScroll=false;
  // $ionicScrollDelegate.scrollTop(true);//ios不管用
}
window.addEventListener('native.keyboardshow', keyboardshow);
window.addEventListener('native.keyboardhide', keyboardhide);
```


### Q：ionic项目中锚点跳转

➣解决：

``` JavaScript
//<div id="itguliang" >
$location.hash('itguliang');//id
$anchorScroll();
```

### Q：ionic项目中$location.hash、$anchorscroll锚点跳转后，ios下页面无法向上滑动，只允许向下滑动

➣解决：

```html
<!-- 加上 overflow-scroll="true" -->
<ion-content overflow-scroll="true">
```

### Q：Ionic 调用键盘搜索

html:

```html
<form action="#">
  <input type="search" placeholder="输入关键字" 
	   ng-model="search.key" ng-focus="searchInput()" 
	   ng-enter="doSearch(search.key)">
</form>
```

directive:

```javascript
.directive('ngEnter', function() {
  return function(scope, element, attrs) {
    element.bind("keydown keypress", function(event) {
      if(event.which === 13) {
        scope.$apply(function(){
          scope.$eval(attrs.ngEnter);
		});
		event.preventDefault();
	  }
	});
  };
});
```

### Q：angularjs中ui-sref传递参数

```html
<div class="col" ui-sref="app.home-search({flag:'course'})"></div>
<div class="col" ui-sref="app.home-search({type: 1, role: 2})"></div>
```

### Q：angularjs中$http模块POST请求request payload转form data

对接后台接口，用post不成功，后台人员截图过来又说他那边可以，对比过后发现angular post请求表单参数在Request payload
他的在Form data,搜了解决方法有两种

解决1配置$httpProvider：

```javascript
var myApp = angular.module('app',[]);  
myApp.config(function($httpProvider){  

  $httpProvider.defaults.transformRequest = function(obj){  
    var str = [];  
    for(var p in obj){
      str.push(encodeURIComponent(p) + "=" + encodeURIComponent(obj[p]));  
    }  
    return str.join("&");  
  }  

  $httpProvider.defaults.headers.post = {
    'Content-Type': 'application/x-www-form-urlencoded'  
  }  

});
```


解决2在post中配置：

```javascript
var url = ApiService.getEndpoint() + "api/feedback/appSave.do";
return $http({
  url: url,
  method: 'POST',
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8'
  },
  transformRequest: function(obj) {
    var str = [];
    for (var p in obj) {
      str.push(encodeURIComponent(p) + "=" + encodeURIComponent(obj[p]));
	}
	return str.join("&");
  },
  data: data
});
```

ps:如果加载了jquery可以直接这样使用

```javascript
transformRequest : function(data){
  return $.param(data);
}
```


### Q：滑动到屏幕顶端不是内容顶端

```javascript
var scroll = document.getElementById(id).offsetTop - $ionicScrollDelegate.getScrollPosition().top;
$ionicScrollDelegate.resize();
$ionicScrollDelegate.scrollBy(0, scroll, true);
```

### Q：Ionic清除某一页面的cache

```javascript
$ionicHistory.clearCache(["tab.aa","tab.bb"]).then(function() {
  $state.go('tab.home');
});
```

### Q：angular video ng-src 不起作用

```javascript
app.filter('trusted', ['$sce', function ($sce) {
  return function(url) {
    return $sce.trustAsResourceUrl(url);
  };
}]);
```

```html
<video controls poster="img/poster.png">
  <source ng-src="{{object.src | trusted}}" type="video/mp4"/>
</video>
```

### Q：Ionic angular切换语言 国际化问题

移步至：[Ionic angular国际化问题](http://itguliang.github.io/blog/2016/04/01/ionic-language/ "国际化")


### Q：config.xml设置KeyboardDisplayRequiresUserAction为true，实现评论键盘自动弹出的功能(没试过)

```html
<preference name="KeyboardDisplayRequiresUserAction" value="false" />
```

### Q：网络情况$cordovaNetwork

```javascript
$rootScope.$on('$ionicView.afterEnter', function(scopes, states) {
  if (window.cordova) {
    var type = $cordovaNetwork.getNetwork();

    if (type == '2g' || type == 'none'|| type == 'unknown') {
      $cordovaToast.showLongCenter('当前网络慢,请在网络顺畅情况下使用');
    }
  }
});
```

### Q：ion-slide-box ng-repeat 刷新不显示问题

ion-slide-box ng-repeat 刷新或者点击其他再回到页面ion-slide-box会不见

``` html
<div class="slider-slides" ng-transclude="" style="width: 0px;">
```

就是这个width会变为0

http://forum.ionicframework.com/t/ion-slide-box-and-ng-repeat/9826
