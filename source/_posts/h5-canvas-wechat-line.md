---
title: HTML5+Canvas 模仿微信运动折线图代码
date: 2019-01-16
categories:
  - IT
abbrlink: 2d7df994
copyright: true
---
点击按钮查看效果（点击折线图试试效果）：
<button onclick="week()" style="display: inline-block;
    border: 1px solid grey;cursor: pointer;
    padding: 5px;
    border-radius: 5px;
    margin: 0 10px 0 0;outline:none;">按周显示</button><button onclick="month()" style="display: inline-block;
    border: 1px solid grey;cursor: pointer;
    padding: 5px;
    border-radius: 5px;
    margin: 0 10px 0 0;outline:none;">按月显示</button>
<canvas id='canvas-line' style="width:100%;"></canvas>
<script>
    var data1 = [9, 12, 10, 29, 33, 17, 6];
    var data2 = [9, 12, 14, 17, 10, 29, 33, 17, 40, 6,
                 9, 12, 14, 17, 10, 29, 33, 17, 40, 6,
                 9, 12, 14, 17, 10, 29, 33, 17, 40, 6];
    var cv = document.getElementById("canvas-line");
        cv.width = cv.offsetWidth*2;
        cv.height = 3 * cv.width / 5;
        // canvas坐标原点在左上角
        var padding = 16,  //边距
        n = 4,//上下距离n个padding
        x1 = padding,  //横线左边点x坐标
        x2 = cv.width - padding,  //横线右边边点x坐标
        y1 = n * padding,  //竖向上边点y坐标
        y2 = cv.height - n * padding,  //竖向下边点y坐标
        xLength = cv.width - 3 * padding,    //x轴的长度
        yLength = cv.height - 3 * n * padding;  //y轴的长度

    var maxNum;//求数组中的最大值
    var pointsWidth;//折线上每个点之间的距离

    var ctx = cv.getContext("2d");
        
    var data;

    function draw(rankdata){
        data=rankdata;
        ctx.globalAlpha = 1;
        ctx.lineWidth = 1;
        ctx.strokeStyle = "white";

        //填充背景颜色
        var grd = ctx.createLinearGradient(0, 0, cv.height, 0);
        grd.addColorStop(0.1, "#21BBB1");
        grd.addColorStop(0.9, "#0E8FA2");
        ctx.fillStyle = grd;
        ctx.fillRect(0, 0, cv.width, cv.height);

        //顶部line 
        ctx.moveTo(x1, y1);
        ctx.lineTo(x2, y1);
        ctx.stroke();

        //底部line
        ctx.moveTo(x1, y2);
        ctx.lineTo(x2, y2);
        ctx.stroke();

        //中断（坐标轴和折线的）连接
        ctx.beginPath();
        ctx.font = "30px Arial";
        ctx.fillStyle = "white";
        ctx.strokeStyle = "white";
        ctx.lineWidth = 3;

        maxNum = Math.max.apply(null, data);//求数组中的最大值
        pointsWidth = xLength / (data.length - 1);//折线上每个点之间的距离

        for (var i = 0; i < data.length; i++) {
            var pointX = padding + i * pointsWidth;
            var pointY = 2* n * padding + (1 - data[i] / maxNum) * yLength;

            ctx.lineTo(pointX, pointY);//折线

            if( (i==0) ||(data.length<=7)||(data.length>7 && (i+1)%5==0)){
                ctx.fillText(i + 1, pointX-5, y2 + 40); //横轴
                ctx.textAlign = "center";
            }
            ctx.stroke();

            // 圆点
            ctx.beginPath();
            ctx.arc(pointX, pointY, 6, 0, Math.PI * 2);
            ctx.fill();

            ctx.moveTo(pointX, pointY);
        }

        // 折线阴影
        ctx.beginPath();
        ctx.moveTo(x1, y2);
        for (var i = 0; i < data.length; i++) {
            var pointX = padding + i * pointsWidth;
            var pointY = 2*n* padding + (1 - data[i] / maxNum) * yLength;
            ctx.lineTo(pointX, pointY);//折线
            if(i==data.length-1){
                ctx.lineTo(pointX, y2);
            }
        }
        ctx.globalAlpha = 0; 
        ctx.lineTo(x1, y2);
        ctx.globalAlpha = 0.1;
        ctx.fillStyle = "white";
        ctx.fill();
    }

    function clearCanvas(){  
        cv=document.getElementById("canvas-line");  
        cxt=cv.getContext("2d");  
        cv.height=cv.height; 
        ctx.clearRect(0,0,cv.width,cv.height);  
    }  

    function week(){
        clearCanvas();
        draw(data1);
    }
    function month(){
        clearCanvas();
        draw(data2);
    }
    var animateX=0;
    var animateY=0;
    var animateText=0;
    cv.addEventListener('click', function (e) {
          console.log(e);
          var index=Math.round((data.length-1) * (e.layerX-padding/2) / (cv.offsetWidth -  2* padding/2));
          animateX = padding + index * pointsWidth;
          animateY = 2*n*padding + (1 - data[index] / maxNum) * yLength;
          animateText= data[index];
          console.log(animateX+"--"+animateY);
          animate();
    }, false);

        // 点击动画
    function animate() {
          clearCanvas();
          draw(data);
          animateY = animateY-10;
          ctx.fillStyle = "white";
          ctx.font = "35px Arial";
          ctx.globalAlpha = 0.5;
          ctx.fillText(animateText, animateX, animateY); //横轴
          if (animateY >= 7*padding) {
            window.requestAnimationFrame(animate,cv);
          }
    }
    week();
</script>

源码（可直接保存到本地运行查看）：
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>HTML5+Canvas模仿微信运动折线图代码</title>
</head>
<style type="text/css">
button{
    display: inline-block;
    border: 1px solid grey;cursor: pointer;
    padding: 5px;
    border-radius: 5px;
    margin: 0 10px 10px 0;outline:none;
}
</style>
<body>
点击按钮查看效果：
<button onclick="week()">按周显示</button>
<button onclick="month()">按月显示</button>
<canvas id='canvas-line' style="width:100%;"></canvas>
</body>
<script>
    var data1 = [9, 12, 10, 29, 33, 17, 6];
    var data2 = [9, 12, 14, 17, 10, 29, 33, 17, 40, 6,
                 9, 12, 14, 17, 10, 29, 33, 17, 40, 6,
                 9, 12, 14, 17, 10, 29, 33, 17, 40, 6];
    var cv = document.getElementById("canvas-line");
        cv.width = cv.offsetWidth*2;
        cv.height = 3 * cv.width / 5;
        // canvas坐标原点在左上角
        var padding = 16,  //边距
        n = 4,//上下距离n个padding
        x1 = padding,  //横线左边点x坐标
        x2 = cv.width - padding,  //横线右边边点x坐标
        y1 = n * padding,  //竖向上边点y坐标
        y2 = cv.height - n * padding,  //竖向下边点y坐标
        xLength = cv.width - 3 * padding,    //x轴的长度
        yLength = cv.height - 3 * n * padding;  //y轴的长度

    var maxNum;//求数组中的最大值
    var pointsWidth;//折线上每个点之间的距离
    var ctx = cv.getContext("2d");
    var data;

    function draw(rankdata){
        data=rankdata;
        ctx.globalAlpha = 1;
        ctx.lineWidth = 1;
        ctx.strokeStyle = "white";

        //填充背景颜色
        var grd = ctx.createLinearGradient(0, 0, cv.height, 0);
        grd.addColorStop(0.1, "#21BBB1");
        grd.addColorStop(0.9, "#0E8FA2");
        ctx.fillStyle = grd;
        ctx.fillRect(0, 0, cv.width, cv.height);

        //顶部line 
        ctx.moveTo(x1, y1);
        ctx.lineTo(x2, y1);
        ctx.stroke();

        //底部line
        ctx.moveTo(x1, y2);
        ctx.lineTo(x2, y2);
        ctx.stroke();

        //中断（坐标轴和折线的）连接
        ctx.beginPath();
        ctx.font = "30px Arial";
        ctx.fillStyle = "white";
        ctx.strokeStyle = "white";
        ctx.lineWidth = 3;

        maxNum = Math.max.apply(null, data);//求数组中的最大值
        pointsWidth = xLength / (data.length - 1);//折线上每个点之间的距离

        for (var i = 0; i < data.length; i++) {
            var pointX = padding + i * pointsWidth;
            var pointY = 2* n * padding + (1 - data[i] / maxNum) * yLength;

            ctx.lineTo(pointX, pointY);//折线

            if( (i==0) ||(data.length<=7)||(data.length>7 && (i+1)%5==0)){
                ctx.fillText(i + 1, pointX-5, y2 + 40); //横轴
                ctx.textAlign = "center";
            }
            ctx.stroke();

            // 圆点
            ctx.beginPath();
            ctx.arc(pointX, pointY, 6, 0, Math.PI * 2);
            ctx.fill();

            ctx.moveTo(pointX, pointY);
        }

        // 折线阴影
        ctx.beginPath();
        ctx.moveTo(x1, y2);
        for (var i = 0; i < data.length; i++) {
            var pointX = padding + i * pointsWidth;
            var pointY = 2*n* padding + (1 - data[i] / maxNum) * yLength;
            ctx.lineTo(pointX, pointY);//折线
            if(i==data.length-1){
                ctx.lineTo(pointX, y2);
            }
        }
        ctx.globalAlpha = 0; 
        ctx.lineTo(x1, y2);
        ctx.globalAlpha = 0.1;
        ctx.fillStyle = "white";
        ctx.fill();
    }

    function clearCanvas(){  
        cv.height=cv.height; 
    }  

    function week(){
        clearCanvas();
        draw(data1);
    }
    function month(){
        clearCanvas();
        draw(data2);
    }
    
    // 点击事件
    var animateX=0;
    var animateY=0;
    var animateText=0;
    cv.addEventListener('click', function (e) {
          console.log(e);
          var index=Math.round((data.length-1) * (e.layerX-padding/2) / (cv.offsetWidth -  2* padding/2));
          animateX = padding + index * pointsWidth;
          animateY = 2*n*padding + (1 - data[index] / maxNum) * yLength;
          animateText= data[index];
          animate();
    }, false);

    // 点击动画
    function animate() {
          clearCanvas();
          draw(data);
          animateY = animateY-30;
          if (animateY >= 7*padding) {
            window.requestAnimationFrame(animate,cv);
          }else{
          	animateY=7*padding;
          }
          ctx.fillStyle = "white";
          ctx.font = "35px Arial";
          ctx.globalAlpha = 0.5;
          ctx.fillText(animateText, animateX, animateY); //横轴
          
    }
    draw(data1);
</script>
</html>
```