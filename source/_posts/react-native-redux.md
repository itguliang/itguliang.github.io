---
title: react-native开发(二)--redux、react-redux
date: 2018-05-11
categories:
  - IT
tags:
  - React
  - Redux
abbrlink: f6b7d227
---
demo地址:https://github.com/itguliang/react-native-demo

效果图：

## 实现功能：

计数器,点击加1按钮数字值就会+1, 点击减1按钮数字值就会-1, 点击归零按钮则数字值置为0

## 实现过程：

### 当前项目下安装redux相关文件

	npm install --save redux
	npm install --save react-redux

### 项目目录结构
	
```
|-- src
    |-- constants 
        |-- counterActionsType.js 
    |-- actions 
        |-- counterAction.js 
    |-- components 
    |-- reducers 
        |-- counterReducer.js 
        |-- allReducer.js 
    |-- store 
    |-- view 
    |-- route.js
```

### 部分代码

创建 `counterActionsTypes.js` ,用来定义计数器action名称, 定义三个action, 一个增加, 一个减小, 一个重置

```javascript
export const INCREASE = 'INCREASE';
export const DECREASE = 'DECREASE';
export const RESET = 'RESET';
```

创建 `counterAction.js` , 创建对应的三个action

```javascript
import { INCREASE, DECREASE, RESET } from '../constants/counterActionsTypes';

const increase = () => ({ type: INCREASE });
const decrease = () => ({ type: DECREASE });
const reset = () => ({ type: RESET });

export {
    increase,
    decrease,
    reset
}
```

创建 `counterReducer.js` , 根据需要在收到相关的action时操作项目的state:

```javascript
import { combineReducers } from 'redux';
import { INCREASE, DECREASE, RESET} from '../constants/counterActionsTypes';

// 原始默认state
const countDefaultState = {
  count: 5,
  factor: 1
}

export default function counter(state=countDefaultState, action) {
  switch (action.type) {
    case INCREASE:
      return { ...state, count: state.count + state.factor };
    case DECREASE:
      return { ...state, count: state.count - state.factor };
    case RESET:
      return { ...state, count: 0 };
    default:
      return state;
  }
}
```
因为以后可能会有其他的reducer,所以创建个统一入口allReducers.js：

```javascript
import { combineReducers } from 'redux';
import counter from './counterReducer';

const allReducers = combineReducers({
  counter: counter,
});

export default allReducers;
```

创建appStore.js:

```javascript
import { createStore, applyMiddleware, compose } from 'redux';
// import createLogger from 'redux-logger';
import allReducer from '../reducers/allReducer';

const configureStore = preloadedState => {
    return createStore (
        allReducer,
        preloadedState,
        // compose (
        //     applyMiddleware(createLogger())
        // )
    );
}
const store = configureStore();
export default store;
```


接下来引入到项目中：

CounterView.js

```javascript
import React , { Component } from 'react';
import {Text,View,StyleSheet,TouchableOpacity,} from 'react-native';
import { connect } from 'react-redux';
import HeaderBar from '../components/HeaderBar';
import { increase, decrease, reset } from '../actions/counterAction';

class CounterView extends Component {
  static navigationOptions = {
    title: 'Counter demo'
  };
  _onPressReset() {
    this.props.dispatch(reset());
  }
  _onPressInc() {
    this.props.dispatch(increase());
  }
  _onPressDec() {
    this.props.dispatch(decrease());
  }

  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.counter}>{this.props.counter.count}</Text>
        <TouchableOpacity style={styles.reset} onPress={()=>this._onPressReset()}>
          <Text>归零</Text>
        </TouchableOpacity>
        <TouchableOpacity style={styles.start} onPress={()=>this._onPressInc()}>
          <Text>加1</Text>
        </TouchableOpacity>
        <TouchableOpacity style={styles.stop} onPress={()=>this._onPressDec()}>
          <Text>减1</Text>
        </TouchableOpacity>
      </View>
    );
  }
}
const styles = StyleSheet.create({
  ...
})
const mapStateToProps = state => ({
    counter: state.counter
})
export default connect(mapStateToProps)(CounterView);

```

App.js:

```javascript
import React, { Component } from 'react';
import {
  StyleSheet,
  Text,
  View
} from 'react-native';
import { AppNavigator } from './src/route';

import { Provider } from 'react-redux';
import store from './src/store/appStore';

export default class App extends Component {
  render() {
    return (
      <Provider store={store}>
          <AppNavigator/>
      </Provider>
    );
  }
}
```

本文参考:
https://blog.csdn.net/xiangzhihong8/article/details/71512756