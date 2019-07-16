---
title: 一个前端开发者的Mac装机清单及常用操作
date: 2018-09-25
categories:
  - IT
abbrlink: a0de71c0
---

### 安装 Homebrew - macOS 平台的软件包管理器

中文主页：https://brew.sh/index_zh-cn.html

```bash
# 安装
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
# 卸载
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"
```

常用命令

```bash
brew install oclint  #安装软件
brew uninstall oclint  #卸载软件
brew search oclint  #搜索软件
brew upgrade oclint  #更新软件
brew list  #查看安装列表
brew update  #更新Homebrew
```
`brew` 是从下载源码解压然后`./configure && make install`,同时会包含相关依存库。并自动配置好各种环境变量，而且易于卸载。

`brew cask` 是已经编译好了的应用包(.dmg/.pkg).仅仅是下载解压，放在统一的目录中(/opt/homebrew-cask/Caskroom), 省掉了自己去下载、解压、拖拽(安装)等步骤，同样，卸载相当容易与干净。


### 安装iTerm2

`brew cask install iterm2`

### 安装nvm、Node、Git

- nvm -- 安装详见：https://github.com/creationix/nvm

nvm:command not found，touch ~/.bash_profile and run the install script again

- Node -- `nvm install stable`

- Git -- `brew install git`

### 安装 Mongodb
```bash
$ brew install mongodb
$ brew services start mongodb
$ mongo  #启动 
>show dbs #显示已经存在的数据库
>exit  #退出
```

### Android Studio and Android SDK

Android Studio 下载地址：http://www.android-studio.org/

安装后打开

<!-- Android SDK 下载地址：http://tools.android-studio.org/index.php/sdk -->

### Java 环境

下载jdk ： https://www.oracle.com/technetwork/java/javase/downloads/jdk11-downloads-5066655.html

选择要安装的版本下载安装

Mac 系统下查看 Java 安装目录，打开终端，输入：/usr/libexec/java_home -V

配置环境变量(未试过，备份下)：
```bash
$ sudo vim /etc/profile
#按下i，显示insert，进入输入模式。
#（注： 在终端输入  /usr/libexec/java_home  可以得到JAVA_HOME 的路径）
#输入如下配置：
JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home"
export JAVA_HOME
CLASS_PATH="$JAVA_HOME/lib"
PATH=".$PATH:$JAVA_HOME/bin"
#按ESC，进入保存
#输入  :wq!   保存
$ source /etc/profile #运行profile配置 马上生效
$ echo $JAVA_HOME #得到配置的路径，说明配置完毕。
```

### 安装maven

1、下载Maven
打开Maven官网下载页面：http://maven.apache.org/download.cgi
下载:apache-maven-3.5.0-bin.tar.gz

解压下载的安装包到某一目录，比如：/Users/xxx/Documents/maven

2、配置环境变量
打开terminel输入以下命令：
```bash
vim ~/.bash_profile #打开.bash_profile文件，在次文件中添加设置环境变量的命令
export M2_HOME=/Users/xxx/Documents/maven/apache-maven-3.5.0
export PATH=$PATH:$M2_HOME/bin
#添加之后保存并推出，执行以下命令使配置生效：
source ~/.bash_profile
```
3、查看配置是否生效
```bash
mvn -v #命令，输入如下：
Apache Maven 3.5.0 (ff8f5e7444045639af65f6095c62210b5713f426; 2017-04-04T03:39:06+08:00)
Maven home: /Users/xxx/Documents/maven/apache-maven-3.5.0
Java version: 1.8.0_121, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home/jre
Default locale: zh_CN, platform encoding: UTF-8
OS name: "mac os x", version: "10.12.6", arch: "x86_64", family: "mac"
#则配置成功。
```

### 编辑器

- Webstorm
- Sublime Text
- xcode
- VScode(Visual Studio Code)


### 其他软件

- iTools Pro -- 管理 iPhone 的工具，本地的.ipa 安装到苹果手机
- [Beyond Compare](http://www.scootersoftware.com/download.php) -- 文件对比工具
- LogRabbit -- 是Android开发人员必不可少的logcat viewer.
- Licecap -- 录屏转成gif的软件
- FileZilla -- FTP软件

还有很多可以参考：https://www.jianshu.com/p/ce3742275279


### 常用命令

* mac 显示 `.git` 等隐藏文件的快捷键： `Command + Shift + . `

* mac 关闭指定端口

```bash
lsof -i:9000  #端口号
kill -9 42624 #对应PID
```

* 快捷键 https://support.apple.com/zh-cn/HT201236

* 手势 https://support.apple.com/zh-cn/HT204895