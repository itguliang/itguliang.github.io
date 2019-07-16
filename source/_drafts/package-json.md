---
title: package.json文件详解
abbrlink: 5358f322
tags:
---

http://javascript.ruanyifeng.com/nodejs/packagejson.html
http://www.mujiang.info/translation/npmjs/files/package.json.html

## 概述
package.json文件，定义了这个项目所需要的各种模块，以及项目的配置信息（比如名称、版本、许可证等元数据）。
npm install命令根据这个配置文件，自动下载所需的模块，也就是配置项目所需的运行和开发环境。

在package.json中最重要的就是name和version字段。他们都是必须的，如果没有就无法install。name和version一起组成的标识在假设中是唯一的。改变包应该同时改变version。

* name：项目名称
* version：版本（遵守“大版本.次要版本.小版本”的格式）
* dependencies：指定了项目运行所依赖的模块
`npm install XXX --save` 表示将该模块写入`dependencies`属性
* devDependencies：指定项目开发所需要的模块。
`npm install XXX --save-dev` 表示将该模块写入`devDependencies`属性

## scripts

指定了运行脚本命令的npm命令行缩写，比如start指定了运行npm run start时，所要执行的命令。

	"scripts": {
	    "preinstall": "echo here it comes!",
	    "postinstall": "echo there it goes!",
	    "start": "node index.js",
	    "test": "tap test/*.js"
	}

## dependencies、devDependencies



==============

http://www.17sucai.com/pins/16875.html

h5 模仿微信运动




