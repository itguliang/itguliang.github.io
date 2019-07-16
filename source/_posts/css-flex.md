---
title: 前端Flex弹性布局
categories:
  - IT
abbrlink: fcbc2b3
date: 2019-03-20 00:00:00
toc: true
---

每次用的时候有些属性总是忘记，写篇文章整理一下
## Flex弹性布局
```css
.box{
  display: flex;
  display: inline-flex;  /* 行内元素使用flex布局 */
  display: -webkit-flex; /* Webkit内核的浏览器，必须加上-webkit前缀  Safari */
}
```
采用`Flex`布局的元素，称为`Flex`容器（flex container），简称”容器”。设为Flex布局以后，子元素的float、clear和vertical-align属性将失效。

## 容器属性
### flex-direction 
表示：item的排列方向
```css
.box{
  display: flex;
  flex-direction:row; /* 默认 水平靠左 */
  flex-direction:row-reverse;  /* 水平靠右 */
  flex-direction:column; /* 垂直靠上 */
  flex-direction:column-reverse; /* 垂直靠下 */
}
```

### flex-wrap
表示：一条轴线排不下，如何换行
```css
.box{
  display: flex;
  flex-wrap:nowrap; /* 默认 不换行 */
  flex-wrap:wrap; /* 换行 第一行在上方 */
  flex-wrap:wrap-reverse; /* 换行 第一行在下方 */
}
```
### flex-flow
表示：`flex-direction` 属性和 `flex-wrap` 属性的简写形式，默认值为`row nowrap`
### justify-content
表示：item在主轴上的对齐方式
### align-items
表示：交叉轴（垂直）上如何对齐
### align-content
表示：多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用.

## item属性
<!-- {% qnimg fcbc2b3-1.png %} -->

<!-- - order
- flex-grow
- flex-shrink
- flex-basis
- flex
- align-self
### order
### flex-grow
### flex-shrink
### flex-basis
### flex
### align-self 


https://github.com/veedrin/horseshoe/blob/master/flex/flex.md
-->
参考：[Flex布局语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)、[Flex布局实例篇](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)