---
title: 微信小程序
date: 2019-06-20
categories:
  - IT
abbrlink: 834a9829
---

#### 小程序访问后端接口

首先配置服务器域名

不配置接口文件直接调用：
```bash
  onLoad: function(options) {
    var that = this;
    that.fetchData();
  },
  fetchData: function() {
    var that = this;
    wx.request({
      url: "接口", //小程序目前发起request请求，必须是https协议
      success: function(res) {
        console.log(res);
        that.setData({
          blogList: res.data.data
        })
      },
      fail: function(res) {
        console.log(res)
      }
    })
  },
```
配置接口文件，config.js


小程序传参数获取数据


小程序图标

#### 小程序解析html文本

https://github.com/icindy/wxParse 使用方法见Readme