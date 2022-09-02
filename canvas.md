[canvas教程-简书](https://www.jianshu.com/p/7593eb9e53c2)
[canvas教程-知乎](https://zhuanlan.zhihu.com/p/81863157)
[canvas教程-MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial)
[canvas实例](https://www.html5tricks.com/tag/html5-canvas/)

#### `<canvas>`标签
```html
<canvas id="canvas" width="800" height="600">
    <-- 不支持canvas的浏览器才会显示该文本 -->
    您的浏览器版本过低，请更新
</canvas>
```
`<canvas>`是行内块元素
`<canvas>`中的id是标签的属性之一，在JavaScript代码中用来指定特定的`<canvas>`，是唯一的。
`<canvas>`是位图，放大会失真

#### 获取canvas对象
```JavaScript
document.body.addEventListener('load' , draw())
function draw( ) {
    var canvas = document.getElementById('canvas')
    // JS判断浏览器是否支持canvas
    if ( canvas.getContext ) {
        // 获取画笔（2d环境）
        var context = canvas.getContext('2d')
    }
}
```
#### 线段
1. 移动画笔 `moveTo(x,y)`
- 以`canvas`画布的左上角为笛卡尔坐标系的原点，x轴的正方向向右，且y轴的正方向向下。
2. 绘制直线 `lineTo(x,y)`，单位px
- 从上一笔的停止点绘制到`(x,y)`
3. 选择画笔
- `context.lineWidth = 5`  画笔粗细为`5px`
- `context.strokeStyle = "#AA394C"`  画笔颜色为 #AA394C

#### 确定绘制
确定绘制只有两种方法，`fill( )`和`stroke( )`，填充和描边

#### 多线条组成图形
1. 绘制折线
复用`lineTo()`就行
```JavaScript
context.moveTo(100,100); 
context.lineTo(300,300);
context.lineTo(100,500);
context.strokeStyle = "black";
context.stroke();
```
2. 绘制多条折线
复用`moveTo()`和`lineTo()`
```JavaScript
context.moveTo(100,100);
context.lineTo(300,300);
context.lineTo(100,500);
context.lineWidth = 5;
context.strokeStyle = "red";
context.stroke();

context.moveTo(300,100);
context.lineTo(500,300);
context.lineTo(300,500);
context.lineWidth = 5;
context.strokeStyle = "blue";
context.stroke();

context.moveTo(500,100);
context.lineTo(700,300);
context.lineTo(500,500);
context.lineWidth = 5;
context.strokeStyle = "black";
context.stroke();
```
> `Canvas`是基于状态的绘制。这段代码每次使用`stroke()`时，它都会把之前设置的状态再绘制一遍。第一次`stroke()`时，绘制一条红色的折线；第二次`stroke()`时，会再重新绘制之前的那条红色的折线，但是这个时候的画笔已经被更换成蓝色的了，所以画出的折线全是蓝色的。换言之，`strokeStyle`属性被覆盖了。同理，第三次绘制的时候，画笔颜色是最后的黑色，所以会重新绘制三条黑色的折线。所以，这里看到的三条折线，其实绘制了3次，一共绘制了6条折线。

#### 使用beginPath()开始绘制
为了让绘制方法不重复绘制，我们可以在每次绘制之前加上`beginPath()`，代表下次绘制的起始之处为`beginPath()`之后的代码。我们在三次绘制之前分别加上`context.beginPath()`。对于只规划路径未确定绘制的方法才需要`beginPath()`。
```JavaScript
context.beginPath();
context.moveTo(100,100);
context.lineTo(300,300);
context.lineTo(100,500);
context.lineWidth = 5;
context.strokeStyle = "red";
context.stroke();

context.beginPath();
context.moveTo(300,100);
context.lineTo(500,300);
context.lineTo(300,500);
context.lineWidth = 5;
context.strokeStyle = "blue";
context.stroke();

context.beginPath();
context.moveTo(500,100);
context.lineTo(700,300);
context.lineTo(500,500);
context.lineWidth = 5;
context.strokeStyle = "black";
context.stroke();
```
#### 绘制矩形
1. 使用closePath()闭合图形
```
context.beginPath();
context.moveTo(150,50);
context.lineTo(650,50);
context.lineTo(650,550);
context.lineTo(150,550);
context.lineTo(150,50);     //绘制最后一笔使图像闭合
context.lineWidth = 5;
context.strokeStyle = "black";
context.stroke();
```
> 用`lineTo()`方法闭合最后一点，闭合点会出现缺口。这种情况是设置了`lineWidth`导致的。如果默认1笔触的话，是没有问题的。但是笔触越大，线条越宽，这种缺口就越明显。使用使用`closePath()`闭合图形来避免这种情况。
![缺口](https://upload-images.jianshu.io/upload_images/7016617-87aad4166b1e97be.png )
<p align="center">左上角有缺口</p>
```JavaScript
context.beginPath();
context.moveTo(150,50);
context.lineTo(650,50);
context.lineTo(650,550);
context.lineTo(150,550);
// context.lineTo(150,50);     //最后一笔可以不画
context.closePath();        //使用closePath()闭合图形

context.lineWidth = 5;
context.strokeStyle = "black";
context.stroke();
```
-  每次开始绘制前都务必要使用`beginPath()`，如果需要闭合图形，每次绘制结束后使用`closePath()`。

2. 给矩形上色
- 先用`fillStyle`属性选择要填充的颜色
- 再用`context.fill()`确定填充

3. 封装矩形绘制方法
- 一个矩形可以由它的左上角坐标和其长宽唯一确定。
- 此外，还需要知道线条的颜色，宽度，描边颜色，填充颜色。
```JavaScript
function drawRect (context,x,y,width,height,color,lineWidth,borderColor,fillColor){
    context.beginPath()
    context.moveTo(x,y)
    context.lineTo(x+width,y)
    context.lineTo(x+width,y+height)
    context.lineTo(x,y+height)
    context.closePath()
    
    context.lineWidth = lineWidth
    context.strokeStyle = borderColor
    context.fillStyle = fillColor
    
    context.fill()
    context.stroke()
}
```
#### 使用rect()方法规划矩形边框
由于绘制矩形是常用的方法，所以在`Canvas API`中已经帮我们封装好了一个绘制矩形的方法 `rect(x, y, width, height)`，`lineWidth`默认为1px，`strokeStyle`默认是黑色。
> `rect()`方法只是规划路径，还没有用`fill()`或`stroke()`确定绘制

#### 清除矩形（清空canvas）
`clearRect(x, y, width, height)`：清除指定矩形区域，让清除部分完全透明。



#### 裁剪路径
`clip()`将当前正在构建的路径转换为当前的裁剪路径。
- 只有裁剪路径区域内的图形才会显示出来。
- 如果和上面介绍的 `globalCompositeOperation` 属性作一比较，它可以实现与 `source-in` 和 `source-atop`差不多的遮罩效果。最重要的区别是裁切路径不会在 canvas 上绘制东西，而且它永远不受新图形的影响。
- 如果我们在创建新裁切路径时想保留原来的裁切路径，可以`context.save()`和`context.restore()`。



#### 线条属性
1. 线条的帽子（端点）`lineCap`，三个属性值
-  `butt`：默认值，端点是垂直于线段边缘的平直边缘（与边缘平直）。
-  `round`：端点是在线段边缘处以线宽为直径的半圆（超出边缘）。
-  `square`：端点是在选段边缘处以线宽为长、以一半线宽为宽的矩形（超出边缘）。

![lineCap](https://upload-images.jianshu.io/upload_images/7016617-7f62cdae933a4070.png )

2. 线条的连接点`lineJoin`，三个属性值
- `miter`，默认值，直接连接，创建尖角。
- `bevel`，连接处为斜角（切去尖角）。
- `round`，连接处为圆角。

![lineJoin](https://upload-images.jianshu.io/upload_images/7016617-59186380227af538.png )

3. 尖角最大长度 `miterLimit`属性
   限制当两条线相交时交接处最大长度；所谓交接处长度（斜接长度）是指线条交接处内角顶点到外角顶点的长度`miterLength`。
   
   ```JavaScript
   context.miterLimit = value;
   ```
   > `value`：number类型，斜接面限制比例的的数字。
   > `miterLimit =  miterLength / lineWidth`  。0、负数、Infinity 和 NaN 都会被忽略。
> 只有当 lineJoin 显示为 `miter`尖角 时，miterLimit 才有效。边角的角度越小，斜接长度就会越大。为了避免斜接长度过长，我们可以使用 miterLimit 属性。如果斜接长度超过 miterLimit 的值，边角会以 lineJoin 的 `bevel`斜角 类型来显示

4. 线宽`lineWidth`，默认为1，数值类型

5. 笔触属性`strokeStyle` 和填充属性`fillStyle`
    `strokeStyle` 设置或返回用于笔触的颜色、渐变或模式
    `fillStyle` 属性设置或返回用于填充绘画的颜色、渐变或模式
    两者都具有三个属性值：
- `color`：笔触颜色
- `gradient`：用于填充绘图的渐变对象（线性 或 径向）
    - `context.createLinearGradient()`创建线性渐变对象
    - `createRadialGradient()`创建径向渐变对象
- `pattern`：用于在指定的方向内填充指定的元素的对象
    - `context.createPattern(要使用的图片，画布或者视频等元素，“重复方式”)`创建pattern对象
- 下面有详细说明

#### 填充颜色和渐变形状
填充颜色主要分两种：
1. 基本颜色 `fillStyle属性`
2. 渐变颜色（线性和径向）
    - 线性渐变创建一个水平、垂直或者对角线的填充图案。
    
    - 径向渐变自中心点创建一个放射状填充。

    - 填充渐变形状分为三步：添加渐变线，为渐变线添加关键色和断点，应用渐变。
    
    2.1 线性渐变
    
    ​	2.1.1 添加渐变线
```JavaScript
var grd = context.createLinearGradient(xstart,ystart,xend,yend);
```
​			2.1.2 添加颜色和断点

```JavaScript
grd.addColorStop(0,"black");
grd.addColorStop(1,"white");
```
> stop传递的是 0 ~ 1 的浮点数，代表断点到(xstart,ystart)的距离占整个渐变色长度是比例。 

​			2.1.2 应用渐变
```JavaScripit
context.fillStyle = grd;
context.strokeStyle = grd;
```
​		2.2 径向渐变
 			2.2.1 添加渐变圆 

```JavaScript
// x0,y0是开始圆圆心，r0是开始圆半径；x1,y1是结束圆圆心，r1是结束圆圆半径
var grd = context.createRadialGradient(x0,y0,r0,x1,y1,r1);
```
​		2.2.2 添加颜色和断点	

```JavaScript
grd.addColorStop(stop,color);
```
> stop传递的是 0 ~ 1 的浮点数，代表断点半径占总半径的比例。	

​		2.2.3 应用渐变

```JavaScript
context.fillStyle = grd;
context.strokeStyle = grd;
```

#### 绘制矩形的快捷方法（描边或填充）

`fillRect(x,y,width,height)`、`strokeRect(x,y,width,height)`这两个函数可以分别看做`rect()`与`fill()`以及`rect()`与`stroke()`的组合。
因为`rect()`仅仅只是规划路径而已，而这两个方法实实在在的绘制。

#### 透明度 
- `globalAlpha`属性影响到 canvas 里所有图形的透明度，有效的值范围是 0.0 （完全透明）到 1.0（完全不透明），默认是 1.0。
> 1. `globalAlpha`只会影响之后的代码，不会影响之前的代码。
> 2. 设置了`globalAlpha`的canvas图形若交叠在一起，会叠加`globalAlpha`值
- 因为 strokeStyle 和 fillStyle 属性接受符合 CSS 3 规范的颜色值，那我们可以用下面的写法来设置具有透明度的颜色。
```JavaScript
// 指定透明颜色，用于描边和填充样式
ctx.strokeStyle = "rgba(255,0,0,0.5)";
ctx.fillStyle = "rgba(255,0,0,0.5)";
```

#### 绘制文本
`fillText(text, x, y [, maxWidth])`
 fillText() 方法在画布上绘制填色的文本。文本的默认颜色是黑色 ，绘制的最大文本宽度是可选的
`strokeText(text, x, y [, maxWidth])`
 strokeText() 方法在画布上绘制文本（没有填色）。文本的默认颜色是黑色，绘制的最大文本宽度是可选的`font = value`： 当前我们用来绘制文本的样式。这个字符串使用和`CSS font` 属性相同的语法（包括复合写法），默认的字体是 `10px sans-serif`。 
`textAlign = value`： 基线对齐。 可选的值包括：top, hanging, middle, alphabetic, ideographic, bottom。 
`direction = value`： 文本方向。可能的值包括：ltr, rtl, inherit。 
`measureText()`： 将返回一个 `TextMetrics`对象 （包含文本属性的对象）。

#### 填充图案
1. `createPattern(img|element|canvas|video等对象,repeat-style)`
repeat-style：
    - 平面上重复：repeat;
    - x轴上重复：repeat-x;
    - y轴上重复：repeat-y;
    - 不使用重复：no-repeat;
2. 创建并填充图案
创建Image对象，为Image对象指定图片源
```JavaScript
var img = new Image();    //创建Image对象
img.src = "8-1.jpg";    //为Image对象指定图片源
```
填充图案
```JavaScript
var pattern = context.createPattern(img,"repeat");
context.fillStyle = pattern;
```

demo部分代码：

```javascript
var img = new Image();
img.src = "https://upload-images.jianshu.io/upload_images/7016617-395eecefc4dd4c5a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240";
img.onload = function(){
    var pattern = context.createPattern(img, "repeat");
    context.fillStyle = pattern;
    context.fillRect(0,0,800,600);
}
```

> 这里使用了`Image`的`onload`事件，它的作用是对图片进行预加载处理，即在图片加载完成后才执行其后`function`的代码体。这个是必须的，如果不写的话，画布将会显示黑屏。因为没有等待图片加载完成就填充纹理，导致浏览器找不到图片。



#### 合成效果
`globalCompositeOperation`属性

- 源图像（新图像） = 您打算放置到画布上的绘图。
- 目标图像（旧图像） = 您已经放置在画布上的绘图。
- 属性值：
`source-over`  默认，在目标图像上显示源图像。
`source-atop`   在目标图像顶部显示源图像。源图像位于目标图像之外的部分是不可见的。
`source-in`   在目标图像中显示源图像。只有目标图像内的源图像部分会显示，目标图像是透明的。
`source-out`   在目标图像之外显示源图像。只会显示目标图像之外源图像部分，目标图像是透明的。
`destination-over`   在源图像上方显示目标图像。
`destination-atop`   在源图像顶部显示目标图像。源图像之外的目标图像部分不会被显示。
`destination-in`   在源图像中显示目标图像。只有源图像内的目标图像部分会被显示，源图像是透明的。
`destination-out`   在源图像外显示目标图像。只有源图像外的目标图像部分会被显示，源图像是透明的。
`lighter`   显示源图像 + 目标图像，即相交部分图形先后填充来增加亮度
`copy`    显示源图像。忽略目标图像，即只显示源图像
`xor`   使用异或操作对源图像与目标图像进行组合，即相交部分为透明
[更多属性值](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial/Compositing)



#### 规划虚线
`setLineDash(arr)`  
在画布上规划一条虚线参数：arr 表示的是一个数组集合，里面的参数可以有多个。仅是规划，还没绘制，需搭配`stroke()`使用。
例如：
[5,10]：5（实线），10（间隔），5（实线），10（间隔）......
[5,10,15]：5（实线），10（间隔），15（实线）......
[5,10,15,20]：5（实线），10（间隔），15（实线），20（间隔）......

#### 获取当前虚线的样式
`getLineDash()`  
没有参数，它得到的结果就是设置虚线线宽的数组arr。

#### 虚线偏移量（向左）
```JavaScript
contextx.lineDashOffset = value
```
- value：float类型，初始值为 0.0。

#### 绘制标准圆弧

`arc()`：规划路径，还没绘制

```js
context.arc(x,y,radius,startAngle,endAngle,anticlockwise)
```

- 前面三个参数，分别是圆心坐标与圆半径。

- `startAngle`、`endAngle`使用的是弧度值，不是角度值。

- anticlockwise表示绘制的方法，是顺时针还是逆时针绘制。它传入布尔值，true表示逆时针绘制，false表示顺时针绘制，缺省值为false。

- 弧度的规定是绝对的，如下图：2*Math.PI ...

![弧度值](https://upload-images.jianshu.io/upload_images/7016617-627ec3e2a285124d.png)

#### 绘制圆角矩形
圆角矩形是由四段线条和四个1/4圆弧组成，拆解如下
![圆角矩形](https://upload-images.jianshu.io/upload_images/7016617-0de40d2f8e9c7b38.jpg)
```JavaScript
// x，y是变成圆角之前的直角点的坐标，width和height是圆角矩形的宽高
function drawRoundRect(cxt, x, y, width, height, radius){
    cxt.beginPath();
    cxt.arc(x + radius, y + radius, radius, Math.PI, Math.PI * 3 / 2);
    cxt.lineTo(x + width - radius , y);
    cxt.arc(x + width - radius , radius + y, radius, Math.PI * 3 / 2, Math.PI * 2);
    cxt.lineTo(x + width , y + height - radius);
    cxt.arc(x + width  - radius , y + height - radius , radius, 0, Math.PI * 1 / 2);
    cxt.lineTo(radius + x, y + height);
    cxt.arc(radius + x, y + height - radius , radius, Math.PI * 1 / 2, Math.PI);
    cxt.closePath();
}
```

#### 使用切点绘制圆弧

-  `arcTo()`方法接收5个参数，分别是控制点的坐标`(x1, y1)`，终点的坐标`(x2, y2)`和圆弧半径。这个方法由两个切线确定一条圆弧，仅规划路径，没有绘制。 具体如下。 
- `arcTo(x1,y1,x2,y2,radius)`：`moveTo(x,y)`或`lineTo(x,y)`与`(x1, y1)`之间必须先规划出一条切线（当前点到切点的线段），`(x1, y1)`和`(x2, y2)`连接成一条不会被绘制的切线，以这两条切线规划出半径为radius的弧线。`(x2, y2)`是圆弧终点的切点。

demo

```JavaScript
window.onload = function(){
    var canvas = document.getElementById("canvas");
    canvas.width = 800;
    canvas.height = 600;
    var context = canvas.getContext("2d");
    context.fillStyle = "#FFF";
    context.fillRect(0,0,800,600);

    drawArcTo(context, 200, 200, 600, 200, 600, 400, 100);
};

function drawArcTo(cxt, x0, y0, x1, y1, x2, y2, r){
    cxt.beginPath();
    cxt.moveTo(x0, y0);
    cxt.arcTo(x1, y1, x2, y2, r);

    cxt.lineWidth = 6;
    cxt.strokeStyle = "red";
    cxt.stroke();
    
	// 蓝色线条，辅助理解
    cxt.beginPath();
    cxt.moveTo(x0, y0);
    cxt.lineTo(x1, y1);
    cxt.lineTo(x2, y2);

    cxt.lineWidth = 1;
    cxt.strokeStyle = "#0088AA";
    cxt.stroke();
}
```

![](https://upload-images.jianshu.io/upload_images/7016617-2c30602c0f395ab7.png )

#### 二次贝塞尔曲线

![示意图](https://upload-images.jianshu.io/upload_images/7016617-162f77a1f200fae8.gif )

```javascript
context.quadraticCurveTo(cpx,cpy,x,y);
```

-  `P0`是起始点，由`moveTo()`或`lineTo()`控制。 

-  `P1(cpx, cpy)`是控制点，`P2(x, y)`是终止点。

-  一个工具可以简单调试直至得到你想要的效果。[在线转换器](http://tinyurl.com/html5quadratic)。 

  

#### 三次贝塞尔曲线

```javascript
context.bezierCurveTo(cp1x,cp1y,cp2x,cp2y,x,y);
```

- 这个方法可谓是绘制波浪线的神器。
- 根据之前的结论，n阶贝塞尔曲线就有n-1个控制点，所以三次贝塞尔曲线有1个起始点、1个终止点、2个控制点。
- 因此传入的6个参数分别为控制点`cp1 (cp1x, cp1y)`，控制点`cp2 (cp2x, cp2y)`，与终止点 `(x, y)`。
- 这个方法也是不用大家去掌握参数具体是怎么填的，只要知道参数的意义就行。
- 和`quadraticCurveTo()`方法一样，`bezierCurveTo()`的三次贝塞尔曲线网上也能找到互动的网页工具。
   这里提供一个网页：[Canvas Bézier Curve Example](http://tinyurl.com/html5bezier)，大家可以动手试一下。

demo

```html
<!DOCTYPE html>
<html lang="zh">
  <head>
    <meta charset="UTF-8" />
    <title>天空草原</title>
    <style>
      #canvas {
        display: block;
        margin: 50px auto;
        border: 1px solid #aaaaaa;
      }
    </style>
  </head>
  <body>
    <div id="canvas-warp">
      <canvas id="canvas">
        你的浏览器居然不支持Canvas？！赶快换一个吧！！
      </canvas>
    </div>

    <script>
      window.addEventListener('load', draw)
      function draw() {
        var canvas = document.getElementById('canvas')
        canvas.width = 800
        canvas.height = 600
        var context = canvas.getContext('2d')
        context.fillStyle = '#FFF'
        context.fillRect(0, 0, 800, 600)

        drawPrairie(context)
        drawSky(context)
        for (var i = 0; i < 5; i++) {
          var x = 500 * Math.random() + 50
          var y = 200 * Math.random() + 50
          var width = 100 * Math.random() + 50
          drawCloud(context, x, y, width)
        }
      }
      // 天空
      function drawSky(cxt) {
        cxt.save()

        cxt.beginPath()
        cxt.moveTo(0, 420)
        cxt.bezierCurveTo(250, 300, 350, 550, 800, 400)
        cxt.lineTo(800, 0)
        cxt.lineTo(0, 0)
        cxt.closePath()

        // 阳光
        var lineStyle = cxt.createRadialGradient(400, 0, 50, 400, 0, 200)
        lineStyle.addColorStop(0, '#42A9AA')
        lineStyle.addColorStop(1, '#2491AA')

        cxt.fillStyle = lineStyle

        cxt.fill()

        cxt.restore()
      }

      // 草原
      function drawPrairie(cxt) {
        cxt.save()

        cxt.beginPath()
        cxt.moveTo(0, 420)
        cxt.bezierCurveTo(250, 300, 350, 550, 800, 400)
        cxt.lineTo(800, 600)
        cxt.lineTo(0, 600)
        cxt.closePath()

        var lineStyle = cxt.createLinearGradient(0, 600, 600, 0)
        lineStyle.addColorStop(0, '#00AA58')
        lineStyle.addColorStop(0.3, '#63AA7B')
        lineStyle.addColorStop(1, '#04AA00')

        cxt.fillStyle = lineStyle
        cxt.fill()

        cxt.restore()
      }

      /*渲染单个云朵
     context:  canvas.getContext("2d")对象
     cx: 云朵X轴位置
     cy: 云朵Y轴位置
     cw: 云朵宽度
     */
      function drawCloud(cxt, cx, cy, cw) {
        //云朵移动范围即画布宽度
        var maxWidth = 800
        //如果超过边界从头开始绘制
        cx = cx % maxWidth
        //云朵高度为宽度的60%
        var ch = cw * 0.6
        //开始绘制云朵

        cxt.beginPath()
        //创建渐变
        var grd = cxt.createLinearGradient(0, 0, 0, cy)
        grd.addColorStop(0, 'rgba(255,255,255,0.8)')
        grd.addColorStop(1, 'rgba(255,255,255,0.5)')
        cxt.fillStyle = grd

        //在不同位置创建5个圆拼接成云朵现状
        cxt.arc(cx, cy, cw * 0.19, 0, 360, false)
        cxt.arc(cx + cw * 0.08, cy - ch * 0.3, cw * 0.11, 0, 360, false)
        cxt.arc(cx + cw * 0.3, cy - ch * 0.25, cw * 0.25, 0, 360, false)
        cxt.arc(cx + cw * 0.6, cy, cw * 0.21, 0, 360, false)
        cxt.arc(cx + cw * 0.3, cy - ch * 0.1, cw * 0.28, 0, 360, false)

        cxt.fill()
      }
    </script>
  </body>
</html>
```

![云朵](https://upload-images.jianshu.io/upload_images/7016617-96590b10993d7f78.png )

<p align="center">其实每次都不一样</p>
#### 指定点是否在路径区域内（包括路径）
`isPointInPath([path,] x , y [,fillRule])`  
如果在则返回true，不在则返回false
> `path`：可选，`Path2D`应用的路径
> `fillRule`：可选，用来决定点在路径内还是在路径外的算法。
> 允许的值：
> - `nonzero`: 非零环绕规则 ，默认值。
> - `evenodd`: 奇偶环绕原则 。 
> `nonzero`: 判断p点是否在一个区域内，从点p向外做一条射线（可以任意方向），多边形的边从左到右经过射线时环绕数-1，多边形的边从右向左经过时环绕数+1，最后环绕数不为0，即表示在多边形内部。
> ![nonzero规则](https://img-blog.csdn.net/20180915161304975?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p5ejAwMDAwMDAw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
- `isPointInPath()`不支持两个方法`fillRect()`，`strokeRect()`，必须先用`rect()`、`arc()`等方法先规划路径
demo
```JavaScript
var canvas = document.getElementById("canvas");
    var ctx = canvas.getContext("2d");

    ctx.beginPath();
    ctx.fillStyle = 'red';
    ctx.rect(50,50,100,100);
    ctx.fill();

    canvas.onclick = function(ev){
        var ev = ev || event;    
        ctx.clearRect(200,0,200,200);
        var l = ev.clientX - canvas.offsetLeft; 
        var t = ev.clientY - canvas.offsetTop;
        if(ctx.isPointInPath(l,t)){
            ctx.font = "40px Arial";
            ctx.fillText((l+','+t),200,120);
        }
    }
```
![demo](https://images2015.cnblogs.com/blog/600165/201604/600165-20160429160236769-2146051009.gif)



#### 指定点是否在路径的描边上
`isPointInStroke([path], x, y)`
在返回`true`，不在返回`false`

> `path`：可选，`Path2D`应用的路径



#### 保存和恢复canvas

 这里还使用到了两个新方法`save()`和`restore()`。之前说过了canvas是基于状态的绘制，保存（推送）当前状态到堆栈（先进先出），调用以下函数： 

```javascript
context.save();
```

 调出最后存储的堆栈恢复画布，使用以下函数： 

```javascript
context.restore();
```

#### 图形变换

图形变换是指用数学方法调整所绘形状的物理属性，其实质是坐标变形。所有的变换都依赖于后台的数学矩阵运算，所以我们只要使用变换的功能即可，无需去理解这些运算。谈到图形变换，不得不得说的三个基本变换方法就是：

- 平移变换：translate(x,y)
- 旋转变换：rotate(deg)
- 缩放变换：scale(sx,sy)
- 矩形变换：transform(a, b, c, d, e, f)

##### 1. 平移变换translate()

 平移变换，故名思议，就是一般的图形位移。 translate() 方法重新映射画布上的坐标轴`原点`位置。当您在 translate() 之后调用诸如 fillRect() 之类的方法时，原点的值会添加到 x 和 y 坐标值上。 
比如这里我想将位于（100，100）的矩形平移至（200，200）点。
那么我只要在绘制矩形之前加上context.translate(100,100)即可。 

demo

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>平移变换</title>
    <style>
        #canvas { border: 1px solid #aaaaaa; display: block; margin: 50px auto; }
    </style>
</head>
<body>
<div id="canvas-warp">
    <canvas id="canvas">
        你的浏览器居然不支持Canvas？！赶快换一个吧！！
    </canvas>
</div>

<script>
    window.onload = function(){
        var canvas = document.getElementById("canvas");
        canvas.width = 800;
        canvas.height = 600;
        var context = canvas.getContext("2d");
        context.fillStyle = "#FFF";
        context.fillRect(0,0,800,600);

        context.fillStyle = "#00AAAA";
        context.fillRect(100,100,200,100);

        context.fillStyle = "red";
        context.translate(100,100); // 原点发生变化
        // translate() 之后调用如 fillRect() 之类的方法时，原点的值会添加到 x 和 y 坐标值上
        context.fillRect(100,100,200,100);

    };
</script>
</body>
</html>
```

![](https://upload-images.jianshu.io/upload_images/7016617-3dda65e68d671444.png )

![](https://upload-images.jianshu.io/upload_images/7016617-0332f55319fcec6c.png )

- ##### 注意使用状态保存

  >  其实这里有一个坑，我们如果想把矩形平移至（300，300）怎么办呢？或许我们会想，直接调用context.translate(200,200)就可以了。好，我们看看效果。 

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>平移变换</title>
    <style>
        body { background: url("./images/bg3.jpg") repeat; }
        #canvas { border: 1px solid #aaaaaa; display: block; margin: 50px auto; }
    </style>
</head>
<body>
<div id="canvas-warp">
    <canvas id="canvas">
        你的浏览器居然不支持Canvas？！赶快换一个吧！！
    </canvas>
</div>

<script>
    window.onload = function(){
        var canvas = document.getElementById("canvas");
        canvas.width = 800;
        canvas.height = 600;
        var context = canvas.getContext("2d");
        context.fillStyle = "#FFF";
        context.fillRect(0,0,800,600);

        context.fillStyle = "#00AAAA";
        context.fillRect(100,100,200,100);

        context.fillStyle = "red";
        context.translate(100,100);
        context.fillRect(100,100,200,100);

        context.fillStyle = "green";
        context.translate(200,200);
        // 第一次平移后原点为(100,100)
        // 第二次平移基于新原点再平移后，原点为(300,300)
      	// (300,300) 加上 (100,100) 等于 (400,400)
        context.fillRect(100,100,200,100);

    };
</script>
</body>
</html>
```

![](https://upload-images.jianshu.io/upload_images/7016617-d46af85246327366.png )

这里的绿色矩形并没有如我们所愿在（300，300）位置处，而是跑到了（400，400）这里。为什么呢？想必大家已经知道了答案——Canvas是基于状态的绘制。在我们第一次平移之后，坐标系已经在（100，100）处了，所以如果继续平移，这个再基于新坐标系继续平移坐标系。那么要怎么去解决呢？很简单，有两个方法。

- 第一，在每次使用完变换之后，记得将坐标系平移回原点，即调用translate(-x,-y)。

- 第二，在每次平移之前使用context.save()，在每次绘制之后，使用context.restore()。

当我们调用`save()`方法，会保存当前Canvas的状态然后作为一个Layer(图层)，添加到Canvas栈中（先进先出）。

`save()`往栈压入一个Layer，`restore()`弹出栈顶的一个Layer，这个Layer代表Canvas的 状态。

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>平移变换</title>
    <style>
        body { background: url("./images/bg3.jpg") repeat; }
        #canvas { border: 1px solid #aaaaaa; display: block; margin: 50px auto; }
    </style>
</head>
<body>
<div id="canvas-warp">
    <canvas id="canvas">
        你的浏览器居然不支持Canvas？！赶快换一个吧！！
    </canvas>
</div>

<script>
    window.onload = function(){
        var canvas = document.getElementById("canvas");
        canvas.width = 800;
        canvas.height = 600;
        var context = canvas.getContext("2d");
        context.fillStyle = "#FFF";
        context.fillRect(0,0,800,600);

        context.fillStyle = "#00AAAA";
        context.fillRect(100,100,200,100);

        context.save();
        context.fillStyle = "red";
        context.translate(100,100);
        context.fillRect(100,100,200,100);
        context.restore();

        context.save();
        context.fillStyle = "green";
        context.translate(200,200);
        context.fillRect(100,100,200,100);
        context.restore();

    };
</script>
</body>
</html>
```

![](https://upload-images.jianshu.io/upload_images/7016617-96ffce2617a04f67.png )

##### 2. 旋转变换rotate()

- 同画圆弧一样，这里的rotate(rad)传入的参数是弧度，不是角度，顺时针旋转。

- 同时需要注意的是，这个的旋转是以坐标系的原点（0，0）为圆心进行的顺时针旋转。

- 所以，在使用rotate()之前，通常需要配合使用translate()平移坐标系，确定旋转的圆心。
   即，旋转变换通常搭配平移变换使用的。确定旋转圆心后将坐标系（原点）平移一个半径，则该坐标系也会绕着旋转圆心旋转。

- 最后一点需要注意的是，Canvas是基于状态的绘制，所以每次旋转都是接着上次旋转的基础上继续旋转，所以在使用图形变换的时候必须搭配save()与restore()方法，一方面重置旋转角度，另一方面重置坐标系原点。

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>旋转变换</title>
    <style>
        #canvas { border: 1px solid #aaaaaa; display: block; margin: 50px auto; }
    </style>
</head>
<body>
<div id="canvas-warp">
    <canvas id="canvas">
        你的浏览器居然不支持Canvas？！赶快换一个吧！！
    </canvas>
</div>

<script>
    window.onload = function(){
        var canvas = document.getElementById("canvas");
        canvas.width = 800;
        canvas.height = 600;
        var context = canvas.getContext("2d");
        context.fillStyle = "#FFF";
        context.fillRect(0,0,800,600);


        for(var i = 0; i <= 12; i++){
            context.save();
          	// 旋转前的正方形
            context.translate(70 + i * 50, 50 + i * 40);
            context.fillStyle = "#00AAAA";
            context.fillRect(0,0,20,20);
            context.restore();

            context.save();
          	// 旋转后的正方形
            context.translate(70 + i * 50, 50 + i * 40);
          	// 顺时针旋转30°
            context.rotate(i * 30 * Math.PI / 180);
            context.fillStyle = "red";
            context.fillRect(0,0,20,20);
            context.restore();
        }

    };
</script>
</body>
</html>
```

![](https://upload-images.jianshu.io/upload_images/7016617-ed334e5588b77f08.png )

> 这里用for循环绘制了14对正方形，其中蓝色是旋转前的正方形，红色是旋转后的正方形。每次旋转都以正方形左上角顶点为原点进行旋转。每次绘制都被save()与restore()包裹起来，每次旋转前都移动了坐标系。

##### 3. 缩放变换scale()

缩放变换`scale(sx,sy)`传入两个参数，分别是水平方向和垂直方向上对象的缩放倍数。例如`context.scale(2,2)`就是对图像放大两倍。其实，看上去简单，实际用起来还是有一些问题的。我们来看一段代码。

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>缩放变换</title>
    <style>
        #canvas { border: 1px solid #aaaaaa; display: block; margin: 50px auto; }
    </style>
</head>
<body>
<div id="canvas-warp">
    <canvas id="canvas">
        你的浏览器居然不支持Canvas？！赶快换一个吧！！
    </canvas>
</div>

<script>
    window.onload = function(){
        var canvas = document.getElementById("canvas");
        canvas.width = 800;
        canvas.height = 600;
        var context = canvas.getContext("2d");
        context.fillStyle = "#FFF";
        context.fillRect(0,0,800,600);

        context.strokeStyle = "red";
        context.lineWidth = 5;
        for(var i = 1; i < 4; i++){
            context.save();
            context.scale(i,i);
            context.strokeRect(50,50,150,100);
            context.restore();
        }
    };
</script>
</body>
</html>
```

![](https://upload-images.jianshu.io/upload_images/7016617-217dc735fffa8ff2.png )

看了上面的例子，大家一定对产生的结果有点奇怪。一是左上角顶点的坐标变了，而是线条的粗细也变了。因此，对于缩放变换有两点问题需要注意：

- 缩放时，图像左上角坐标的位置也会对应缩放。
- 缩放时，图像线条的粗细也会对应缩放。

##### 4. 矩形变换transform()
`transform(a, b, c, d, e, f)`: 这个方法是将当前的变形矩阵乘上一个基于自身参数的矩阵，在这里我们用下面的矩阵： 

```text
    a c e
  [ b d f ]
    0 0 1
```

如果任意一个参数是无限大，变形矩阵也必须被标记为无限大，否则会抛出异常。

这个函数的参数各自代表如下：

a(m11)：水平方向的缩放

b(m12)：水平方向的倾斜偏移

c(m21)：竖直方向的倾斜偏移

d(m22)：竖直方向的缩放

e(dx)：水平方向的移动

d(dy)：竖直方向的移动

- `setTransform(m11, m12, m21, m22, dx, dy)`
- `resetTransform`: 重置当前变形为单位矩阵，它和调用以下语句是一样的： `ctx.setTransform(1, 0, 0, 1, 0, 0)`



#### `Path2D` 对象

`Path2D()` 构造函数返回一个新的 `Path2D` 对象（
 用来声明路径），可以选择另一条路径作为参数（创建一个拷贝），或者选择 `SVG path` 数据构成的字符串。我们可以理解为将路径或者图形等保存在 `Path2D` 对象里托管
```JavaScript
// 创建空路径对象
new Path2D();   
// 复制另一个Path2D对象，path是一个 Path2D 对象
new Path2D(path); 
 // 根据SVG路径数据中的路径（d）创建一个 Path2D 对象
new Path2D(d); 
```
所有的路径方法比如`moveTo`, `rect`, `arc`或`quadraticCurveTo`等，如我们前面见过的，都可以在`Path2D`中使用。

##### 添加一条新路径到对当前路径
`Path2D.addPath(path [, transform])`
- `path`：需要添加的 `Path2D` 路径
- `transform`：变换矩阵，`DOMMatrix`对象 或 `SVGMatrix`对象
> [DOMMatrix](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMMatrix) 
> [SVGMatrix](https://developer.mozilla.org/zh-CN/docs/Web/API/SVGMatrix)（已被Web标准废弃）



#### 阴影

`shadowOffsetX` 和 `shadowOffsetY` 用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。值是`float`类型 
`shadowBlur` 用于设定阴影的模糊程度，其数值并不跟像素数量挂钩，也不受变换矩阵的影响，默认为 0，值是`float`类型 。
`shadowColor` 是标准的 CSS 颜色值，用于设定阴影颜色效果，默认是全透明的黑色。 

#### 图片

引入图像到canvas里需要以下两步基本操作：

1. 获得一个指向`HTMLImageElement`的对象或者另一个canvas元素的引用作为源，也可以通过提供一个URL的方式来使用图片（参见[例子](https://link.zhihu.com/?target=http%3A//www.html5canvastutorials.com/tutorials/html5-canvas-images/)）
2. 使用`drawImage()`函数将图片绘制到画布上


##### 1. 获得需要绘制的图片

- `HTMLImageElement`： 这些图片是由`Image()`函数构造出来的，或者任何的`<img>`元素
- `HTMLVideoElement`: 用一个HTML的 `<video>`元素作为你的图片源，可以从视频中抓取当前帧作为一个图像
- `ImageBitmap`: 这是一个高性能的位图，可以低延迟地绘制，它可以从上述的所有源以及其它几种源中生成。
  这些源统一由 `CanvasImageSource`类型来引用。
  有几种方式可以获取到我们需要在canvas上使用的图片。
  (1) 使用相同页面内的图片
- `document.images`集合
- `document.getElementsByTagName()`
- `document.getElementById()`
  (2) 使用其它域名下的图片
在 HTMLImageElement上使用其[crossOrigin](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement/crossOrigin)属性，你可以请求加载其它域名上的图片。如果图片的服务器允许跨域访问这个图片，那么你可以使用这个图片而不污染canvas，否则，使用这个图片将会[污染canvas](https://developer.mozilla.org/zh-CN/docs/Web/HTML/CORS_enabled_image#.E4.BB.80.E4.B9.88.E6.98.AF.22.E8.A2.AB.E6.B1.A1.E6.9F.93.22.E7.9A.84canvas)。
   (3) 使用其它 canvas 元素和引用页面内的图片
   (4) 由零开始创建图像 或者 我们可以用脚本创建一个新的 HTMLImageElement 对象。要实现这个方法，我们可以使用很方便的Image()构造函数。
```javascript
var img = new Image();   // 创建img元素
img.onload = function(){
  // 执行drawImage语句
}
img.src = 'myImage.png'; // 设置图片源地址
```
   (5) 通过 data: url 方式嵌入图像
我们还可以通过 data:url 方式来引用图像。Data urls 允许用一串 Base64 编码的字符串的方式来定义一个图片。

```javascript
img.src = 'data:image/gif;base64,R0lGODlhCwALAIAAAAAA3pn/ZiH5BAEAAAEALAAAAAALAAsAAAIUhA+hkcuO4lmNVindo7qyrIXiGBYAOw=='
```

   (6) 使用视频帧
```JavaScript
function getMyVideo() {
  var canvas = document.getElementById('canvas');
  if (canvas.getContext) {
    var ctx = canvas.getContext('2d');

    return document.getElementById('myvideo');
  }
}
```

 它将为这个视频返回[`HTMLVideoElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLVideoElement)对象，正如我们前面提到的，它可以作为我们的Canvas图片源（当前视频播放的帧）。 

##### 2. 绘制图片

顾名思义，该方法就是用于将图像绘制于Canvas画布当中，具体用法有三种：

> ① 在画布上定位图像：context.drawImage( img , x , y)
>  ② 在画布上定位图像，并规定图像的宽度和高度：context.drawImage( img , x , y , width , height )
>  ③ 剪切图像，并在画布上定位被剪切的部分：context.drawImage( img , sx , sy , swidth , sheight, x , y , width , height )

|参数|描述|
|---|---|
|img|规定要使用的图像、画布或视频|
|sx|可选，开始剪切的 x 坐标位置|
|sy|可选，开始剪切的 y 坐标位置|
|swidth|可选，裁剪框的宽度|
|sheight|可选，裁剪框的高度|
|dx|`image`的左上角在目标canvas上 X 坐标|
|dy|`image`的左上角在目标canvas上 Y 坐标|
|dwidth|可选，要使用的图像的宽度|
|dheight|可选，要使用的图像的高度|

>  SVG图像必须在 <svg> 根指定元素的宽度和高度。 

![](https://pic1.zhimg.com/80/v2-50bab101932466cc1517d75eafd589a8_720w.jpg )

##### 3. 保存图片
(1) `toDataURL()`

- 如果传入的类型非“`image/png`”，但是返回的值以“`data:image/png`”开头，那么该传入的类型是不支持的。
- Chrome支持“`image/webp`”类型。

这个方法与以上三种方法不同，它是Canvas对象的方法，该方法返回的是一个包含data URI的字符串。
该字符串可直接作为图片路径地址填入`<img>`标签的`src`属性 或 
可以将它放在一个有`download`属性的超链接里用于保存到本地。
该方法用于 `canvas` 转为 图片。
具体用法如下：

```javascript
canvas.toDataURL(type, encoderOptions);
```

- 如果画布的高度或宽度是0，那么会返回字符串`data:,。`
- `type`： 图片格式，默认为 `image/png` 
-  encoderOptions ： 在指定图片格式为 `image/jpeg 或` `image/webp的情况下，可以从 0 到 1 的区间内选择图片的质量`。如果超出取值范围，将会使用默认值 `0.92`。其他参数会被忽略。 
-  返回值：一个包含data URI的字符串。

(2) `canvas.toBlob(callback, type, encoderOptions)`

创造[`Blob`](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob)对象，用以展示canvas上的图片；这个图片文件可以被缓存或保存到本地，由用户代理端自行决定。如不特别指明，图片的类型默认为 image/png 。 `Blob` 对象表示一个不可变、原始数据的类文件对象。它的数据可以按文本或二进制的格式进行读取，也可以转换成 [`ReadableStream`](https://developer.mozilla.org/zh-CN/docs/Web/API/ReadableStream) 来用于数据操作。  

- `callback`：回调函数，可获得一个单独的[`Blob`](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob)对象参数。
- `type`： 图片格式，默认为 `image/png` 
- encoderOptions ： 在指定图片格式为 `image/jpeg 或` `image/webp的情况下，可以从 0 到 1 的区间内选择图片的质量`。如果超出取值范围，将会使用默认值 `0.92`。其他参数会被忽略。
- 返回值：无

#### 像素操作

1. [根据行、列读取某像素点的R/G/B/A值的公式](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial/Pixel_manipulation_with_canvas)：
```JavaScript
// R是+0，以此类推
imageData.data[((行数-1)*imageData.width + (列数-1))*4 + 0/1/2/3]
```
> 这里的A（Alpha）是用0-255表示，实际上A=0-255的值/255
2. 读取像素数组的大小（以bytes为单位）
```JavaScript
var numBytes = imageData.data.length;
```
3. 创建一个`ImageData`对象
```JavaScript
var myImageData = ctx.createImageData(width, height)
```
上面代码创建了一个新的具体特定尺寸的`ImageData`对象。所有像素被预设为透明黑。

你也可以传入另一个`Image`对象来创建一个新的`ImageData`对象。这个新的对象像素全部被预设为透明黑。这个并非复制了图片数据。
```JavaScript
var myImageData = ctx.createImageData(anotherImageData);
```
4. 得到场景像素数据
```JavaScript
var myImageData = ctx.getImageData(left, top, width, height);
```
这个方法会返回一个`ImageData`对象，它代表了画布区域的对象数据，此画布的四个角落分别表示为(left, top), (left + width, top), (left, top + height), 以及(left + width, top + height)四个点。这些坐标点被设定为画布坐标空间元素。`ImageData`对象的`data`属性是保存图像每个像素的`R/G/B/A`值的数组。

> 任何在画布以外的元素都会被返回成一个透明黑的`ImageData`对像。

5.  将图像数据重写至Canvas画布 

```javascript
context.putImageData(imgData,dx,dy,dirtyX,dirtyY,dirtyWidth,dirtyHeight)
```
|参数|描述|
|---|---|
|imgData|`ImageData`对象（包含像素值数组）|
|dx|放置在画布上的x坐标|
|dy|放置在画布上的y坐标|
|dirtyX|可选，将图像数据用矩形框选。矩形区域矩形区域左上角的x坐标。默认是整个图像数据的左上角x坐标。|
|dirtyY|可选，将图像数据用矩形框选。矩形区域矩形区域左上角的y坐标。默认是整个图像数据的左上角y坐标。|
|dirtyWidth|可选，将图像数据用矩形框选。矩形区域的宽度。默认是图像数据的宽度。|
|dirtyHeight|可选，将图像数据用矩形框选。矩形区域的高度。默认是图像数据的高度。|



#### 动画

##### 1. 动画的基本步骤

（1）清空 canvas
除非接下来要画的内容会完全充满 canvas （例如背景图），否则你需要清空所有。最简单的做法就是用 `clearRect` 方法。
（2）保存 canvas 状态
如果你要改变一些会改变 canvas 状态的设置（样式，变形之类的），又要在每画一帧之时都是原始状态的话，你需要先保存一下。
（3）绘制动画图形（animated shapes）
这一步才是重绘动画帧。
（4）恢复 canvas 状态
如果已经保存了 canvas 的状态，可以先恢复它，然后重绘下一帧。 

##### 2. 操作动画

在 canvas 上绘制内容是用 canvas 提供的或者自定义的方法，而通常，我们仅仅在脚本执行结束后才能看见结果，比如说，在 for 循环里面做完成动画是不太可能的。

因此， 为了实现动画，我们需要一些可以定时执行重绘的方法。有两种方法可以实现这样的动画操控。首先可以通过 setInterval 和 setTimeout 方法来控制在设定的时间点上执行重绘。
（1）有安排的更新画布 Scheduled updates

- `setInterval(function, delay)`：当设定好间隔时间后，function会定期执行。
- `setTimeout(function, delay)`：在设定好的时间之后执行函数
- `requestAnimationFrame(callback)`：告诉浏览器你希望执行一个动画，并在重绘之前，请求浏览器执行一个特定的函数来更新动画。

##### 太阳系的动画

```js
<!DOCTYPE html>
<html lang="zh">
  <head>
    <meta charset="UTF-8" />
    <title>太阳系</title>
    <style>
      #canvas {
        display: block;
        margin: 50px auto;
      }
    </style>
  </head>
  <body>
    <div id="canvas-warp">
      <canvas id="canvas" width="800" height="600">
        你的浏览器居然不支持Canvas？！赶快换一个吧！！
      </canvas>
    </div>

    <script>
      var sun = new Image()
      var moon = new Image()
      var earth = new Image()
      window.addEventListener('load', init)

      function init() {
        var ctx = document.getElementById('canvas').getContext('2d')
        sun.src = 'https://mdn.mozillademos.org/files/1456/Canvas_sun.png'
        moon.src = 'https://mdn.mozillademos.org/files/1443/Canvas_moon.png'
        earth.src = 'https://mdn.mozillademos.org/files/1429/Canvas_earth.png'
        window.requestAnimationFrame(draw)
        function draw() {
          ctx.fillStyle = '#FFF'
          ctx.fillRect(0, 0, 300, 300)
          // 在新图像上方显示旧图像
          ctx.globalCompositeOperation = 'destination-over'
          ctx.clearRect(0, 0, 300, 300) // clear canvas

          ctx.fillStyle = 'rgba(0,0,0,0.4)'
          ctx.strokeStyle = 'rgba(0,153,255,0.4)'
          ctx.save()
          ctx.translate(150, 150)

          // Earth
          var time = new Date()
          // ctx绕(150, 150)旋转，60s转一圈
          // 看不懂时注释掉rotate的代码
          ctx.rotate(
            ((2 * Math.PI) / 60) * time.getSeconds() +
              ((2 * Math.PI) / 60000) * time.getMilliseconds(),
          )
          // 地球绕太阳的公转半径
          ctx.translate(105, 0)
          // 没被太阳照到的半球添加阴影效果
          ctx.fillRect(0, -12, 40, 24) // Shadow
          // 地球原图的大小为24*24
          ctx.drawImage(earth, -12, -12)

          // Moon
          ctx.save()
          // 6s走一圈
          ctx.rotate(
            ((2 * Math.PI) / 6) * time.getSeconds() +
              ((2 * Math.PI) / 6000) * time.getMilliseconds(),
          )
          // 月球绕地球的公转半径
          ctx.translate(0, 28.5)
          // 月亮原图的大小为7*7
          ctx.drawImage(moon, -3.5, -3.5)
          ctx.restore()

          ctx.restore()

          ctx.beginPath()
          // 地球轨道
          ctx.arc(150, 150, 105, 0, Math.PI * 2, false) // Earth orbit
          ctx.stroke()

          ctx.drawImage(sun, 0, 0, 300, 300)

          window.requestAnimationFrame(draw)
        }
      }
    </script>
  </body>
</html>
```

[更多demo]( https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial/Basic_animations )

##### [高级动画效果](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial/Advanced_animations)

#### 点击区域和无障碍访问
`<canvas>`标签只是一个位图，它并不提供任何已经绘制在上面的对象的信息。 canvas的内容不能像语义化的HTML一样暴露给一些协助工具。一般来说，你应该避免在交互型的网站或者App上使用canvas。接下来的内容能帮助你让canvas更加容易交互。

1. 内容兼容
<canvas> ... </canvas>标签里的内容被可以对一些不支持canvas的浏览器提供兼容。这对残疾用户设备也很有用（比如屏幕阅读器），这样它们就可以读取并解释DOM里的子节点。
2. `ARIA`规则
Accessible Rich Internet Applications (ARIA) 定义了让Web内容和Web应用更容易被有身体缺陷的人获取的办法。你可以用ARIA属性来描述canvas元素的行为和存在目的。详情见ARIA和 ARIA 技术。
```html
<canvas id="button" tabindex="0" role="button" aria-pressed="false" aria-label="Start game"></canvas>
```
3. 点击区域
判断鼠标坐标是否在canvas上一个特定区域里一直是个有待解决的问题。hit region API让你可以在canvas上定义一个区域，这让无障碍工具获取canvas上的交互内容成为可能。它能让你更容易地进行点击检测并把事件转发到DOM元素去。这个API有以下三个方法（都是实验性特性，请先在浏览器兼容表上确认再使用）。

[context.addHitRegion()](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/addHitRegion): 在canvas上添加一个点击区域。
[context.removeHitRegion()](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/removeHitRegion): 从canvas上移除指定id的点击区域。
[context.clearHitRegions()](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/clearHitRegions): 移除canvas上的所有点击区域。

你可以把一个点击区域添加到路径里并检测`MouseEvent.region`属性来测试你的鼠标有没有点击这个区域，例：
```html
<canvas id="canvas"></canvas>
<script>
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");

ctx.beginPath();
ctx.arc(70, 80, 10, 0, 2 * Math.PI, false);
ctx.fill();
ctx.addHitRegion({id: "circle"});

canvas.addEventListener("mousemove", function(event){
  if(event.region) {
    alert("hit region: " + event.region);
  }
});
</script>

// addHitRegion()方法也可以带一个control选项来指定把事件转发到哪个元素上（canvas里的元素）。
ctx.addHitRegion({control: element});
```
4. 焦点圈
- `context.drawFocusIfNeeded()`: 如果给定的元素获得了焦点，这个方法会沿着在当前的路径画个焦点圈。
- `context.scrollPathIntoView()`: 把当前的路径或者一个给定的路径滚动到显示区域内。
#### canvas优化性能
1. 在离屏canvas上预渲染相似的图形或重复的对象

- 将需要重复绘制的画布直接生成一个画布放到屏幕外（可以用`类`实现），需要时用`drawImage()`将其绘制到展示的画布上，这样就不必重复绘制相同的画布

2. 避免浮点数的坐标点，用整数取而代之
- 当你画一个没有整数坐标点的对象时会发生子像素渲染。
- 浏览器为了达到抗锯齿的效果会做额外的运算。为了避免这种情况，请保证在你调用`drawImage()`函数时，用`Math.floor()`函数对所有的坐标点取整。

3.  不要在用`drawImage`时缩放图像 

 在离屏canvas中缓存图片的不同尺寸，而不要用`drawImage()`去缩放它们。 

4.  使用多层画布去画一个复杂的场景 

- 在您的应用程序中，您可能会发现某些对象需要经常移动或更改，而其他对象则保持相对静态。 在这种情况下，可能的优化是使用多个`<canvas>`元素对您的项目进行分层。

- 例如，假设您有一个游戏，其UI位于顶部（z-index:3），中间是游戏性动作（z-index:2），底部是静态背景（z-index:1）。 在这种情况下，您可以将游戏分成三个`<canvas>`层。 UI将仅在用户输入时发生变化，游戏层随每个新框架发生变化，并且背景通常保持不变。

5.  用CSS设置大的背景图 

 如果像大多数游戏那样，你有一张静态的背景图，用一个静态的<div>元素，结合[`background`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background) 特性，以及将它置于画布元素之后。这么做可以避免在每一帧在画布上绘制大图。 

6.  用`CSS transforms`特性缩放画布 

`CSS transforms` 使用GPU，因此速度更快。 最好的情况是不直接缩放画布，或者具有较小的画布按比例放大，而不是较大的画布按比例缩小。 

7.  关闭透明度 

如果你的游戏使用画布而且不需要透明，当使用 `HTMLCanvasElement.getContext()` 创建一个绘图上下文时把 `alpha` 选项设置为 `false` 。这个选项可以帮助浏览器进行内部优化。 

```javascript
var ctx = canvas.getContext('2d', { alpha: false });
```

8. 更多Tips

- 将画布的函数调用集合到一起（例如，画一条折线，而不要画多条分开的直线）
- 避免不必要的画布状态改变
- 渲染画布中的不同点，而非整个新状态
- 尽可能避免 [`shadowBlur`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/shadowBlur)特性
- 尽可能避免[text rendering](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Drawing_text)
- 尝试不同的方法来清除画布([`clearRect()`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/clearRect) vs. [`fillRect()`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/fillRect) vs. 调整canvas大小)
-  有动画，请使用[`window.requestAnimationFrame()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/requestAnimationFrame) 而非[`window.setInterval()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/setInterval)
- 请谨慎使用大型物理库

#### 注意点

1. `Canvas` 的默认大小为300像素×150像素（宽×高，像素的单位是px）。但是，可以使用HTML的宽度和高度属性来自定义Canvas 的尺寸。

2. 如果你绘制出来的图像是扭曲的, 尝试用width和height属性为`<canvas>`明确规定宽高，而不是使用CSS。

3. `<canvas>`元素可以像任何一个普通的图像一样（有`margin`，`border`，`background`等等属性）被设计。然而，这些样式不会影响在`canvas`中的实际图像。我们将会在一个专门的章节里看到这是如何解决的。当开始时没有为`canvas`规定样式规则，其将会完全透明。

4. 与`<img> `元素不同， `<canvas>`元素需要结束标签(`</canvas>`)。如果结束标签不存在，则文档的其余部分会被认为是替代内容，将不会显示出来。

