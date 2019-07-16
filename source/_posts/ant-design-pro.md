---
title: Ant Design Pro
date: 2019-02-01
categories:
  - IT
abbrlink: 9ad5068d
---

https://www.yuque.com/ant-design/course/abl3ad

<!-- 
React onClick点击事件传参三种写法
写法一
<Button onClick={this.delFolder.bind(this,"abc")}></Button>

定义delFolder方法
delFolder = (name,e)=>{
alert(name)
}

用bind绑定，调用是作为第二个参数传递，不用显示传递事件对象，定义方法时，事件对象作为最后一个参数传入

写法二
<Button onClick={this.delFolder("abc")}></Button>

定义delFolder方法

delFolder = (name)=>{return (e)=>{    
console.log(e);    
console.log(key);}}

返回一个函数，事件对象在返回的函数中

第三种写法
<Button onClick={（e）=>this.delFolder("abc",e)}></Button>delFolder = (name,e)=>{}事件对象作为第二个参数传递 -->


### React 如何正常渲染一段HTML字符串

```html
<div dangerouslySetInnerHTML = {{__html:data.content}} ></div>
```

### 图片加载失败显示默认图片
```html
import defaultImg from '../../assets/defaultImg.jpg';

const ImgComponent = function (props) {
  function onError(e) {
    e.target.onerror = null;
    e.target.src = defaultImg;
  }
  return <img src={props.imgPath} onError={onError}/>
}

export default ImgComponent;
```

### 时间格式化

```html
import moment from 'moment';
<div>{moment(data.date).format('YYYY-MM-DD')}</div>
```
### 在react项目中，我一个标签需要用到多个样式时 <div className="title1 title2 title3"> 转换成 <div className={style.title1} 这种形式，怎么写？

直接用es6的 `${}` 链接就好了

```css
className={ isTitle1 ? styles.title1 : ''}

className={`${styles.title1} ${styles.title2} ${styles.title3}`}

className={`${isTitle1 ? styles.title1 : ''} ${isTitle2 ? styles.title2 : ''}`}
```


### ？effects 名称不能和reducers名称相同
会造成死循环

### ? Ant Design Pro 页面的跳转、传参数、接收参数

```javascript
import {routerRedux} from 'dva/router';
//在页面时跳转使用
dispatch(routerRedux.push('/home'));
//在model里面跳转
yield put(routerRedux.push('/home')); 
//link方式跳转
<Link to={{pathname:'/user',query:id}}>链接</Link>
<Link to={'/home/' + data.id}>链接</Link> 
// 在页面中获取参数 this.props.match.params.id 

// routerRedux 传参
dispatch(routerRedux.push({
  pathname: '/home',
  query:{id:"11111",name:"222222"}
}))

this.props.location.query 

const params  = location.query; //在model中获取参数
```