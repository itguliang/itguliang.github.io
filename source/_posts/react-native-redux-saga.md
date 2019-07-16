---
title: react-native开发(三)--redux、redux-saga
date: 2018-05-12 00:00:00
categories:
  - IT
tags:
  - React
  - Redux
abbrlink: cc4b8fb1
---
demo地址:https://github.com/itguliang/react-native-demo

效果图：

## 实现功能：

计时器,点击开始按钮就开始跑秒,点击停止按钮停止跑秒, 点击重置按钮时间就清零. 跑秒状态下数字为蓝色,停止状态下为黑色.

## 实现过程：

### 安装

	npm install --save redux-saga

### 项目目录结构
	
```
|-- src 
    |-- constants 
        |-- timerActionsType.js 
    |-- actions 
        |-- timerAction.js 
    |-- reducers 
        |-- timerReducer.js
        |-- allReducer.js
    |-- sagas 
    |-- store 
    |-- view 
    |-- route.js
```

### 部分代码

创建 `timerActionsTypes.js` :

```javascript
export const START = 'START';
export const STOP = 'STOP';
export const RESET = 'RESET';
export const RUN_TIMER = 'RUN_TIMER';
```

创建 `timerAction.js` , 创建对应的三个action

```javascript
import { START, STOP, RESET, RUN_TIMER } from './actionsTypes';

const start = () => ({ type: START });
const stop = () => ({ type: STOP });
const reset = () => ({ type: RESET });
const runTime = () => ({ type: RUN_TIMER });

export {
    start,
    stop,
    reset,
    runTime
}
```

创建 `timerReducer.js` , 根据需要在收到相关的action时操作项目的state:

```javascript
import { combineReducers } from 'redux';
import { START, STOP, RESET, RUN_TIMER} from '../constants/timerActionsTypes';

// 原始默认state
const timerDefaultState = {
  seconds: 0,
  runStatus: false
}

export default function timer(state = timerDefaultState, action) {
  switch (action.type) {
    case START:
      return { ...state, runStatus: true };
    case STOP:
      return { ...state, runStatus: false };
    case RESET:
      return { ...state, seconds: 0 };
    case RUN_TIMER:
      return { ...state, seconds: state.seconds + 1 };
    default:
      return state;
  }
}
```

修改 `allReducers.js`：
```javascript
import { combineReducers } from 'redux';
import counter from './counterReducer';
import timer from './timerReducer';

const allReducers = combineReducers({
  timer: timer,
  counter: counter,
});

export default allReducers;
```

sagas文件夹下创建 `timerSagas.js`文件, 处理业务逻辑:

```javascript
import { takeEvery, delay, END } from 'redux-saga';
import { put, call, take, fork, cancel, cancelled } from 'redux-saga/effects';
import { START, STOP, RESET, RUN_TIMER} from '../constants/timerActionsTypes';
import { stop, runTime } from '../actions/timerAction';

function* watchStart() {
  // 一般用while循环替代 takeEvery
  while (true) {
    // take: 等待 dispatch 匹配某个 action
    yield take(START);
    // 通常fork 和 cancel配合使用，实现非阻塞任务，take是阻塞状态，也就是实现执行take时候，无法向下继续执行，fork是非阻塞的，同样可以使用cancel取消一个fork 任务
    var runTimeTask = yield fork(timer);
    yield take(STOP);
    // cancel: 取消一个fork任务
    yield cancel(runTimeTask);
  }
}

function* watchReset() {
  while (true) {
    yield take(RESET)
    yield put(stop());
  }
}

function* timer() {
  try {
    while(true) {
      // call: 有阻塞地调用 saga 或者返回 promise 的函数，只在触发某个动作
      yield call(delay, 1000);
      // put: 触发某个action， 作用和dispatch相同
      yield put(runTime());
    }
  } finally {
    if (yield cancelled()) {
      console.log('取消了runTimeTask任务');
    }
  }
}

export default function* rootSaga() {
    yield fork(watchStart);
    yield fork(watchReset)
}
```

修改 `appStore.js`,添加 `timerReducer`, 把saga作为中间件添加进store:

```javascript
import { createStore, applyMiddleware, compose } from 'redux';
import createSagaMiddleware, { END } from 'redux-saga';
// import createLogger from 'redux-logger';
import counterReducer from '../reducers/counterReducer';
import timerReducer from '../reducers/timerReducer';
import sagas from './sagas';

const configureStore = preloadedState => {
	const sagaMiddleware = createSagaMiddleware();
	const store = createStore(
        counterReducer,
        timerReducer,
        preloadedState,
        compose (
            // applyMiddleware(sagaMiddleware, createLogger())
            applyMiddleware(sagaMiddleware)
        )
    )
    sagaMiddleware.run(sagas);
    store.close = () => store.dispatch(END);
    return store;
}

const store = configureStore();
export default store;
```

接下来引入到项目中：

TimerView.js

```javascript
import React , { Component } from 'react';
import {
  Text,
  View,
  StyleSheet,
  TouchableOpacity,
} from 'react-native';
import { connect } from 'react-redux';
import HeaderBar from '../components/HeaderBar';

import { reset, start, stop } from '../actions/timerAction';

class TimerView extends Component {
  static navigationOptions = {
    title: 'Timer demo'
  };
  _onPressReset() {
    this.props.dispatch(reset());
  }

  _onPressInc() {
    this.props.dispatch(start());
  }

  _onPressDec() {
    this.props.dispatch(stop());
  }

  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.counter}>{this.props.timer.seconds}</Text>
        <TouchableOpacity style={styles.reset} onPress={()=>this._onPressReset()}>
          <Text>重置</Text>
        </TouchableOpacity>
        <TouchableOpacity style={styles.start} onPress={()=>this._onPressInc()}>
          <Text>开始</Text>
        </TouchableOpacity>
        <TouchableOpacity style={styles.stop} onPress={()=>this._onPressDec()}>
          <Text>停止</Text>
        </TouchableOpacity>
      </View>
    );
  }
}
const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'flex-start'
  },
  counter: {
    marginTop:40,
    color:'blue',
    fontSize:30,
  },
  reset: {
    marginTop:20,
    backgroundColor: 'yellow',
  },
  start: {
    marginTop:20,
    backgroundColor: 'yellow',
  },
  stop: {
    marginTop:20,
    backgroundColor: 'yellow',
  },
})
const mapStateToProps = state => ({
    timer: state.timer
})

export default connect(mapStateToProps)(TimerView);
```