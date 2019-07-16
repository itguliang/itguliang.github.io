---
title: react-native开发(一)--基本组件
date: 2018-05-10 00:00:00
categories:
  - IT
tags:
  - React
abbrlink: 5e934b3b
---
看了一点点文档，看到导航部分
写个小 demo

## 想实现的效果

{% qnimg 5e934b3b-1.png %}

创建项目  react-native init ReacNativeDemo
在初始结构下添加了src文件夹放置源码，目录结构如下

{% qnimg 5e934b3b-2.png %}


## 步骤

### 安装组件

安装导航组件React Navigation

yarn add react-navigation

安装第三方图标组件react-native-vector-icons

yarn add  react-native-vector-icons

### 项目目录结构
  
```
|-- src //文件夹存放所有js文件
    |-- constants //设定类型type 
    |-- actions //设定预处理消息过程
    |-- components //纯组件
    |-- reducers //设定消息的具体处理过程
    |-- store //管理应用的state.可以理解为一个存放APP内所有组件的state的仓库
    |-- view //界面
    |-- route.js //路由
```

### 创建tab页面

HomeView.js

```javascript
import React, {Component} from 'react';
import {
   Text,
} from 'react-native';
export default class Home extends Component {
   render() {
       return (
           <Text>首页</Text>
       )
   }
}
```

`MineView.js`和 `HomeView.js` 几乎一样，只是把 `<Text>首页</Text>` 中的文字改成相应的。

### 配置导航路由 `route.js`

先导入需要的组件，包括上面自己写的tab

{% qnimg 5e934b3b-3.png %}

注册tabs

```javascript
// 注册tabs
const AppTabNavigator = TabNavigator({
   Home: {
       screen: HomeView,
       navigationOptions: {
           tabBarLabel: '首页',
           tabBarIcon: ({tintColor}) => {
             return <Entypo name="home" size={25} color={tintColor} />;
           },
       },
   },
   Mine: {
       screen: MineView,
       navigationOptions: {
           tabBarLabel: '我的',
           tabBarIcon: ({tintColor}) => {
             return <Entypo name="user" size={25} color={tintColor} />;
           },
       }
   }
}, {
   animationEnabled: false, // 切换页面时是否有动画效果
   tabBarPosition: 'bottom', // 显示在底端，android 默认是显示在页面顶端的
   swipeEnabled: true, // 是否可以左右滑动切换tab
   backBehavior: 'none', // 按 back 键是否跳转到第一个Tab(首页)， none 为不跳转
   tabBarOptions: {
       activeTintColor: '#1890ff', // 文字和图片选中颜色
       inactiveTintColor: 'gray', // 文字和图片未选中颜色
       showIcon: true, // android 默认不显示 icon, 需要设置为 true 才会显示
       indicatorStyle: {
           height: 0  // 如TabBar下面显示有一条线，可以设高度为0后隐藏
       }, 
       pressOpacity: 0.8,
       style: {  
               height: 46,  
               backgroundColor: '#ffffff',  
               zIndex: 0,  
               position: 'relative'  
       },  
       labelStyle: {  
               fontSize: 11,  
               paddingVertical: 0,  
               marginTop: -3,
       },  
       iconStyle: {  
               marginTop: -2  
       },  
   },
});
```

配置路由

```javascript
export const AppNavigator = StackNavigator(
   {
       Home  : { screen: AppTabNavigator, navigationOptions: { header: null }},
       Mine    : { screen: MineView, navigationOptions: { header: null }},
   },
   {
       headerMode        : 'screen',
       initialRouteName  :'Home',
   }
);
```

### 修改APP.js

在render中引用定义的StackNavigator--AppNavigator

```javascript
import React, { Component } from 'react';
import {
 StyleSheet,
 Text,
 View
} from 'react-native';
import { AppNavigator } from './src/route';


export default class App extends Component {
 render() {
   return (
     <AppNavigator/>
   );
 }
}
```


### 运行 react-native run-ios

{% qnimg 5e934b3b-4.png %}


### 图标react-native-vector-icons 踩坑

地址：https://github.com/oblador/react-native-vector-icons

可查询图标地址：https://oblador.github.io/react-native-vector-icons/

安装：yarn add  react-native-vector-icons

Ios:

{% qnimg 5e934b3b-5.png %}

安装了好几次，运行都有问题
react-native开发踩坑之 ios上react-native-vector-icons 的error：unRecognized font family 'FontAwesome'
把node_modules/react-native-vector-icons下的Fonts文件添整个加到工程中
{% qnimg 5e934b3b-6.png %}
{% qnimg 5e934b3b-7.png %}

Finish，然后编辑Info.plist文件

```xml
<key>UIAppFonts</key>
<array>
	<string>Zocial.ttf</string>
	<string>SimpleLineIcons.ttf</string>
	<string>Octicons.ttf</string>
	<string>MaterialIcons.ttf</string>
	<string>MaterialCommunityIcons.ttf</string>
	<string>Ionicons.ttf</string>
	<string>Foundation.ttf</string>
	<string>FontAwesome.ttf</string>
	<string>Ferther.ttf</string>
	<string>Entypo.ttf</string>
	<string>Evilcons.ttf</string>
</array>
```

Xcode的Info.plist出现这种

{% qnimg 5e934b3b-8.png %}

编辑完重新运行才有效