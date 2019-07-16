---
title: Vue全家桶+Node+MongoDB 博客系统
date: 2018-10-01
categories:
  - IT
tags:
  - Vue
abbrlink: 8f32d5f4
---
纯练手项目，从零开始一点一点搭建，项目源码地址：https://github.com/itguliang/vue-node-mongodb-blog
## 项目环境
- node : v10.11.0
- mongo : v4.0.2
- vue : 3.0.5

## 创建项目

```bash
$ vue create vue-node-mongodb-blog

//选择记录
Vue CLI v3.0.5
? Please pick a preset: Manually select features
? Check the features needed for your project: 
  Babel, Router, CSS Pre-processors,Linter
? Use history mode for router? (Requires proper server setup for index fallback
in production) Yes
? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported
by default): 
  Sass/SCSS
? Pick a linter / formatter config: 
  Standard
? Pick additional lint features: 
  Lint on save
? Where do you prefer placing config for Babel, PostCSS, ESLint, etc.? 
  In dedicated config files
? Save this as a preset for future projects? No

$ cd vue-node-mongodb-blog
$ npm install express --save
$ npm install axios --save //用axios调用后端接口
$ npm run serve
```

在项目的根目录新建一个叫server的目录，用于放置Node的东西。进入server目录，再新建三个js文件： 
- index.js （入口文件） 
- db.js （设置数据库相关） 
- api.js （编写接口）

## 项目目录
```bash
├── server
│   ├── index.js     # 入口文件
│   ├── config.js    # 设置数据库相关
│   └── api.js       # 编写接口
└── src
    ├── components   # 组件
    ├── views        # 页面
    ├── router.js    # 路由
    ├── utils.js     # 工具
    └── store
        ├── index.js          # 我们组装模块并导出 store 的地方
        ├── actions.js        # 根级别的 action
        ├── mutations.js      # 根级别的 mutation
        └── modules
            ├── xxxx.js       # xxxx模块
            └── xxxx.js       # xxxx模块

```

## 记

- vue cli 3.x 解决跨域问题：
根目录创建 `vue.config.js` 详见：https://cli.vuejs.org/zh/config/#devserver-proxy
```js
module.exports = {
  devServer: {
    proxy: 'target'
  }
}
```

- concurrently--npm run 执行多个命令
```js
"scripts": {
    "serve": "concurrently \"node server/index\" \"vue-cli-service serve\" "
}
```

- vue路由传参的三种基本方式

- vuex

安装：
```bash
$ npm install vuex --save
```