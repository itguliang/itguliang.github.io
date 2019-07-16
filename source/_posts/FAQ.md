---
title: 解决问题集锦
date: 2018-05-24
toc: false
categories:
  - IT
tags:
  - FAQ
abbrlink: 5254fc55
---
### Q：vim 命令

https://www.cnblogs.com/libaoliang/articles/6961676.html


### Q：配置中的波浪号`~`和插入符号`^`

* "1.1.1" : 遵守“大版本.次要版本.小版本”的格式规定，即安装时只安装指定版本。

* "~1.2.2" : 安装1.2.x的最新版本（不低于1.2.2），但是不安装1.3.x，即安装时不改变大版本号和次要版本号。

* "^1.2.2" : 安装1.x.x的最新版本（不低于1.2.2），但是不安装2.x.x，即安装时不改变大版本号。需要注意的是，如果大版本号为0，则插入号的行为与波浪号相同，这是因为处于开发阶段，即使次要版本号变动，也可能带来程序的不兼容。

* latest : 安装最新版本。



### Q：Chrome 窗口 console调整到源码底部

在sources界面下，按下ESC按钮，即可！



### Q：nvm 设置默认 node 版本

nvm alias default v5.0.0



### Q：js使用正则实现ReplaceAll全部替换的方法

```javascript
JS 字符串有replace() 方法。但这个方法只会对匹配到的第一个字串替换

1. str.replace(/oldString/g,newString)
str.replace(/word/g,"Excel")
g 的意义是：执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）
	
2. str.replace(new RegExp(oldString,"gm"),newString)
str.replace(new RegExp("word","gm"),"Excel")

3. 增加String 对象原型方法 replaceAll
String.prototype.replaceAll = function(s1,s2){ 
   return this.replace(new RegExp(s1,"gm"),s2); 
}
```



### Q：手机号验证正则

```javascript
var myreg = /^1[34578]{1}\d{9}/;
	
if (!myreg.test($scope.feedback.mobile)) {
	alert("请输入正确的手机号码");
} 
```



### Q：runSequence 不管用

```javascript
安装了，也按语法写了，就是执行两句,然后不动了
[15:17:56] Using gulpfile D:\workspace\MobileLearn\gulpfile.js
[15:17:56] Starting 'default'...
[15:17:56] Starting 'sass'...

问题应该出现在 sass task上

//scss编译成css（gulp-sass）
gulp.task('sass', function(done) {
  gulp.src('./www/scss/style.scss')
    .pipe(plugins.sass())
    .pipe(gulp.dest('./www/css/'));
});

解决：
1.把那个done去掉
2.加上done();
```


**原因**：``gulp``是异步的，按顺序执行（用插件或者用依赖）的时候，在先执行的或者说被依赖的任务，加入一个提示，来告知什么时候它会完成：可以再完成时候返回一个 ``callback``，或者返回一个 ``promise`` 或 ``stream``，不然的话系统会一直等待它完成

详读官网文档描述：[http://www.gulpjs.com.cn/docs/api/](http://www.gulpjs.com.cn/docs/api/ "gulp")


### Q：Sublime text 中格式化Html的快捷键更改

Ctrl+Alt+F快捷键跟我的160WiFi的什么快捷键冲突了，更改如下：

	[Preferences]->[Key Bindings-User]中，添加如下：

	{ "keys": ["ctrl+shift+f"], "command": "reindent" }




### Q：Android SDK Manager国内无法更新的解决方案

1.Android SDK Manager——>Tools——>Options

	HTTP Proxy Server:mirrors.neusoft.edu.cn
	HTTP Proxy Port:80
	选中Force https://... sources to be fetched using http://...复选框

2.访问站长工具网站（http://tool.chinaz.com/ )，选择 其他工具/超级PING ，把域名dl-ssl.google.com粘贴进去，然后勾选海外的，点击查询，会列出一些可以ping通的IP地址。
使用cmd命令行ping 对应的ip地址，修改系统的host文件，具体位置在（C:\Windows\System32\drivers\etc），在最后一行增加域名解析记录。然后重新打开Android SDK Manager，试一下。如果不行，就换一个ip，重新修改host，总有可以的



### Q：Chrome跨域设置

在Chrome快捷方式，目标里如下设置
	
	"your path\chrome.exe" --disable-web-security



### Q：网页左上角小图标

	<link rel="shortcut icon" href="路径/图片.ico"/> //（图片16*16）



### Q：SublimeText3批量查找替换文件夹中多个文件包含的字符

  - 右击文件夹，Find in folder...
  - 底部出现的对话框，分别输入查找和替换的内容，点击右下角的Replace
  - 弹出Confirm提示，点击Replace后，会弹出很多改动的文件
  - File --> Save All

### ? 注意

ionic1 controller里有let声明变量造成页面加载不了;

数组类的赋值考虑为空的时候赋为 [];