---
title: 纯CSS制作箭头(空心、实心、任意大小、任意方向)
date: 2018-07-17
categories:
  - IT
tags:
  - Html/Css
abbrlink: c3ec74bc
---

项目中要实现个角度没那么尖尖的箭头，不想用图标，试着纯CSS实现下，transform matrix不是很懂这个语法，后期再看吧哈哈，反正先实现效果。

效果图：
{% qnimg c3ec74bc-1.png %}

源码：
可复制到 http://www.runoob.com/try/try.php?filename=trycss3_transform_rotate 查看运行结果

也可用伪元素 before after 实现

```Html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>css制作空心的上下左右的箭头</title>
  <style type="text/css">
    div{
      margin-top:20px;
    }
    span{
    	display:inline-block;
    	width: 15px;
    	height: 15px;
    	margin-right:40px;
    }
    .border{
    	border-top: 2px solid red;
    	border-right: 2px solid red;
    }
    .up{
    	transform: rotate(-45deg);
    }
    .down{
    	transform: rotate(135deg);
    }
    .left{
    	transform: rotate(-135deg);
    }
    .right{
    	transform: rotate(45deg);
    }
    .up1{
    	transform: rotate(-45deg);
    }
    .up1 span{
    	transform: matrix(1,0.3,0.3,1,4,-5);
    }
    .down1{
    	transform: rotate(135deg);
    }
    .down1 span{
    	transform: matrix(1,0.3,0.3,1,-5,1);
    }
    .left1{
    	transform: rotate(-135deg);
    }
    .left1 span{
    	transform: matrix(1,0.3,0.3,1,-4,4);
    }
    .right1{
    	transform: rotate(45deg);
    }
    .right1 span{
    	transform: matrix(1,0.3,0.3,1,4,-6)
    }
  </style>
</head>
<body>
  <div>
     <span class="border up"></span>
     <span class="border down"></span>
     <span class="border left"></span>
     <span class="border right"></span>
  </div>
  <div>
     <span class="up1"><span class="border"></span></span>
     <span class="down1"><span class="border"></span></span>
     <span class="left1"><span class="border"></span></span>
     <span class="right1"><span class="border"></span></span>
  </div>
</body>
</html>
```
