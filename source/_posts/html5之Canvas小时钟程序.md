---
title: Html5之Canvas小时钟程序
date: 2016-12-16 20:23:12
updateed: 2016-12-16 20:23:12
tags: [Javascript, html5]
---

<center>
  闲来无事，研究了一下 JavaScript 插件的写法，今天就将自己写的一个小插件记录下来。
本文介绍了此款插件的基本用法，实现的功能以及代码。
<center>
</br>
</center>
  标签：#Javascript, #html5
</center>

<!-- more -->

这一段时间有空在看 Canvas，由于以前开发过 ActionScript 版本的时钟，想着 Canvas 也一定能够实现，所以花了几个小时来调了一下，最终出来了，很是开心，所以在这里记录一下：
首先奉上效果图，虽然比较丑，没有用 CSS 过多的去渲染，这里完成了基本功能
：![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMjE4MTAxMjEwOTg4)
下面是我的目录结构：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMjE2MjAxNzUwNzY1)
目录结构很简单，下面看看代码是怎么实现的：
myClock.html:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Canvas</title>
    <script type="text/javascript" src="../scripts/ClockFace.js"></script>
    <script type="text/javascript" src="../scripts/SimpleClock.js"></script>
</head>
<body>
    <canvas id="drawing" width="200" height="200">drgdgd</canvas>
</body>
</html>
```

模拟时钟类：

```
/**
 * Created by wulin on 2016/12/16.
 * 模拟时钟类。程序入口
 */
window.onload = init;

function init() {
    var simpleClock = new SimpleClock();
    simpleClock.initClock();
}

function SimpleClock() {
    this.face = null;

    this.initClock = function() {
        this.face = new ClockFace(200);

        /*手动初始化*/
        this.face.init();

        /*开始*/
        setInterval(function() {
            this.face.init();
        }.bind(this),1000);
    }
}
```

钟面实现类：

```
/**
 * Created by wulin on 2016/12/16.
 * 钟面实现类
 */

function ClockFace(w) {

    // 圆钟宽高等长
    this.w = w;

    /*设置半径*/
    this.radius = Math.round(this.w / 2);

    /*圆心坐标*/
    this.centerX = this.radius;
    this.centerY = this.radius;
    this.currentTime;

    this.drawing = document.getElementById("drawing");

    this.init = function() {

        if(!this.drawing.getContext) {
            return;
        }

        this.context = this.drawing.getContext("2d");

        //开始路径
        this.context.beginPath();

        //清楚canvas内容
        this.context.clearRect(0,0,200,200);

        /*绘制圆圈*/
        this.drawBorder();

        //变换原点
        this.context.translate(this.centerX, this.centerY);

        /*绘制小时文本*/
        this.drawHourLabels();

        //初始化时间
        this.draw();

        /*绘制 针*/
        this.createHands();

        //描边路径
        this.context.stroke();

        //变换原点
        this.context.translate(-100, -100);
        this.context.closePath();
    };

    this.draw = function() {

        this.currentTime = new Date();
        this.showTime(this.currentTime);

    };

    //绘制圆圈
    this.drawBorder = function() {
        //绘制外圆
        this.context.arc(this.centerX, this.centerY, this.radius, 0, 2 * Math.PI, false);

        //绘制内圆
        this.context.moveTo(this.w - 6, this.centerY);
        this.context.arc(this.centerX, this.centerY, this.radius - 6, 0, 2 * Math.PI, false);


    };

    //绘制小时文本
    this.drawHourLabels = function() {
        for(var i=1; i <= 12; i++) {

            /*夹角角度(弧度)*/
            var angleInRadians = i * 30 * (Math.PI/180);

            //显示位置
            var x =  0.8 * this.radius * Math.sin(angleInRadians);
            var y = -(0.8 * this.radius * Math.cos(angleInRadians));

            //设置文本样式
            this.context.font = "bold 14px Arial";
            this.context.textAlign = "center";
            this.context.textBaseline = "middle";
            this.context.fillText(i.toString(), x, y);
        }
    };

    this.createHands = function() {

        //时针
        var hourX =  0.5 * this.radius * Math.sin(this.hourHandRotation);
        var houurY = -(0.5 * this.radius * Math.cos(this.hourHandRotation));
        this.drawHand(hourX, houurY);

        /**
         * 分针*/
        var minuteX =  0.65 * this.radius * Math.sin(this.minuteHandRotation);
        var minuteY = -(0.65 * this.radius * Math.cos(this.minuteHandRotation));
        this.drawHand(minuteX, minuteY);

        /**
         * 秒针*/
        var secondX =  0.75 * this.radius * Math.sin(this.secondHandRotation);
        var secondY = -(0.75 * this.radius * Math.cos(this.secondHandRotation));
        this.drawHand(secondX, secondY);
    };

    this.drawHand = function(x, y) {

        console.log(x + "," +  y);
        this.context.moveTo(0, 0);
        this.context.lineTo(x, y);
    };

    /**
     * 显示时间；
     * @param time
     */
    this.showTime = function(time) {
        /**
         * 截取时间段*/
        var seconds = time.getSeconds();
        var minutes = time.getMinutes();
        var hours = time.getHours();

        /**
         * 钟面初始时，时针、分针以及秒针都指向6点；
         * 如果设置this.secondHandRotation = 90;就表示此时秒针顺时针转90度；*/
        this.secondHandRotation = (seconds*6) * (Math.PI/180);    // 可以算出1秒6度；
        this.minuteHandRotation = (minutes*6) * (Math.PI/180);    // 同上，1分6度；
        this.hourHandRotation = ((hours*30) + (minutes*0.5)) * (Math.PI/180);    //知道，1小时30度，那1分就是60分之30度，即1分时时钟转动0.5度；
    }
}

```

canvas 版本的时钟是参照前一篇文章[Flex 版本时钟](http://blog.csdn.net/u010323023/article/details/53697962)
来开发的。大致的思路和结构都差不多，在这里我没有用 canvas 的 roate()旋转方法实现，效率较 roate 方法相对来说比较低下一点，改日奉上 roate 的使用方法。还有些文章的时间比较仓促，代码写完也没来得及过多的优化，不足之处还请见谅。

<p style="text-align: center;"><span style="font-size:18px;"><strong><span style="color:#ff00;"><span style="color:#ff0000;">友情提示：</span></span>请尊重作者劳动成果，如需转载本博客文章请注明出处！谢谢合作！</strong></span></p>

<p align="center"><strong><span style="font-size:18px;">【作者：吴林&nbsp;&nbsp;</span></strong><a target="_blank" href="https://super-lin0.github.io/"><strong><span style="font-size:18px;">https://super-lin0.github.io/</span></strong></a><strong>】</span></strong></p>
