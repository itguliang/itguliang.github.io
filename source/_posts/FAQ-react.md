---
title: 解决问题集锦-React
date: 2018-11-03
categories:
  - IT
tags:
  - React
  - FAQ
abbrlink: 9ff6bdc8
---

### Q：React 对 state 内部对象属性进行单独的赋值
state数据：
```javascript
this.state = {
  loginData:{
    loginId: '',
    password: '',
  },
  loading:false,
};
```
修改loginId：
```javascript
setLoginId=(e)=>{
  let val=e.target.value;
  let data = Object.assign({}, this.state.loginData, { loginId: val })
  //或者用 ES6 的扩展运算符来替换 Object.assign
  let data = { ...this.state.loginData,loginId: val };
  this.setState({
    loginData: data
  })
}
```
[Object.assign](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)



### Q：ReactNative iOS运行出错：No bundle URL present

报错：
No bundle URL present

Make sure you’re running a packager server or have included a .jsbundle file in your application bundle

解决：
npm install 再运行


### Q：ReactNative iOS运行出错：ENOENT: no such file or directory

报错：
ENOENT: no such file or directory, lstat '/Users/gfishare/mark/ReactNativeDemo/ios/build/Build/Intermediates.noindex/React.build/Debug-iphonesimulator/React.build/Objects-normal/x86_64/RCTCxxUtils.o-4375e354'

解决：
react-native link 再运行

### Q：react-native run-ios Unexpected token

环境：
node v8.9.4
react-native-cli v2.0.1
react-native v0.55.4
npm v6.1.0

问题：
  /@babel/core/lib/transformation/file/file.js:63
    constructor(options, {
                         ^

  SyntaxError: Unexpected token {
      at exports.runInThisContext (vm.js:53:16)
      at Module._compile (module.js:373:25)
      at Module._extensions..js (module.js:416:10)

临时解决：react-native init demo --version 0.44.3//创建个低版本的

待解决


### Q：xcrun: error: unable to find utility "instruments", not a developer tool or in PATH

解决：$ sudo xcode-select -s /Applications/Xcode.app/Contents/Developer/


###  ？问题如下图
{% qnimg 9ff6bdc8-1.png %}

解决：cd ./node_modules/react-native/third-party/glog-0.3.4 && ../../scripts/ios-configure-glog.sh

### Q：:-1: Build input file cannot be found: '/Users/itguliang/workspace/react-native-demo/node_modules/react-native/Libraries/WebSocket/libfishhook.a'
解决：找到图片中标记的位置，删除再重新引入
{% qnimg 9ff6bdc8-2.png %}



