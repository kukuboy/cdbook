---
title: canvas相关知识
date: 2020-09-22 11:05:47
tags: canvas
categories:
    - 前端
    - canvas
---

此文章做个人笔记，仅供参阅，来源：[Canvas](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API)

# 基本介绍


__Canvas API 提供了一个通过JavaScript 和 HTML的\<canvas\>元素来绘制图形的方式。它可以用于动画、游戏画面、数据可视化、图片编辑以及实时视频处理等方面。   
Canvas API主要聚焦于2D图形。而同样使用<\canvas\>元素的 WebGL API 则用于绘制硬件加速的2D和3D图形。__

---

# 基本使用



    <canvas id="canvas"></canvas>
    ---
    const canvas = document.getElementById('canvas');
    const ctx = canvas.getContext('2d');


---
# 检查支持性

    <canvas id="tutorial" width="150" height="150">
    canvas在此不被支持
    </canvas>

    var canvas = document.getElementById('tutorial');

    if (canvas.getContext){
    var ctx = canvas.getContext('2d');
    // drawing code here
    } else {
    // canvas-unsupported code here
    }

# 绘制矩形


__不同于 SVG,\<canvas\> 只支持两种形式的图形绘制：矩形和路径（由一系列点连成的线段）。所有其他类型的图形都是通过一条或者多条路径组合而成的。不过，我们拥有众多路径生成的方法让复杂图形的绘制成为了可能__

__canvas提供了三种方法绘制矩形__


        fillRect(x, y, width, height)
        绘制一个填充的矩形
        strokeRect(x, y, width, height)
        绘制一个矩形的边框
        clearRect(x, y, width, height)
        清除指定矩形区域，让清除部分完全透明。


# 绘制路径

__图形的基本元素是路径。路径是通过不同颜色和宽度的线段或曲线相连形成的不同形状的点的集合。一个路径，甚至一个子路径，都是闭合的。使用路径绘制图形需要一些额外的步骤。__

1. 首先，你需要创建路径起始点。
2. 然后你使用画图命令去画出路径。
3. 之后你把路径封闭。
4. 一旦路径生成，你就能通过描边或填充路径区域来渲染图形。

__以下是所要用到的函数：__

    beginPath()
    新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径。
    closePath()
    闭合路径之后图形绘制命令又重新指向到上下文中。
    stroke()
    通过线条来绘制图形轮廓。
    fill()
    通过填充路径的内容区域生成实心的图形。


1. 生成路径的第一步叫做beginPath()。本质上，路径是由很多子路径构成，这些子路径都是在一个列表中，所有的子路径（线、弧形、等等）构成图形。而每次这个方法调用之后，列表清空重置，然后我们就可以重新绘制新的图形。  
   >注意：当前路径为空，即调用beginPath()之后，或者canvas刚建的时候，第一条路径构造命令通常被视为是moveTo（），无论实际上是什么。出于这个原因，你几乎总是要在设置路径之后专门指定你的起始位置。
2. 调用函数指定绘制路径
3. 就是闭合路径closePath(),不是必需的。这个方法会通过绘制一条从当前点到开始点的直线来闭合图形。如果图形是已经闭合了的，即当前点为开始点，该函数什么也不做。

    绘制三角形的代码如下：

        function draw() {
        var canvas = document.getElementById('canvas');
        if (canvas.getContext) {
            var ctx = canvas.getContext('2d');

            ctx.beginPath();
            ctx.moveTo(75, 50);
            ctx.lineTo(100, 75);
            ctx.lineTo(100, 25);
            ctx.fill();
        }
        }


## 移动笔触

        moveTo(x, y)
        将笔触移动到指定的坐标x以及y上。
        当canvas初始化或者beginPath()调用后，你通常会使用moveTo()函数设置起点。我们也能够使用moveTo()绘制一些不连续的路径。看一下下面的笑脸例子。我将用到moveTo()方法（红线处）的地方标记了。

## 绘制直线

        lineTo(x, y)
        绘制一条从当前位置到指定x以及y位置的直线。
        该方法有两个参数：x以及y ，代表坐标系中直线结束的点。开始点和之前的绘制路径有关，之前路径的结束点就是接下来的开始点，等等。。。开始点也可以通过moveTo()函数改变。

## 圆弧

        绘制圆弧或者圆，我们使用arc()方法。当然可以使用arcTo()，不过这个的实现并不是那么的可靠，所以我们这里不作介绍。

        arc(x, y, radius, startAngle, endAngle, anticlockwise)
        画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle开始到endAngle结束，按照anticlockwise给定的方向（默认为顺时针）来生成。
        arcTo(x1, y1, x2, y2, radius)
        根据给定的控制点和半径画一段圆弧，再以直线连接两个控制点。
        这里详细介绍一下arc方法，该方法有六个参数：x,y为绘制圆弧所在圆上的圆心坐标。radius为半径。startAngle以及endAngle参数用弧度定义了开始以及结束的弧度。这些都是以x轴为基准。参数anticlockwise为一个布尔值。为true时，是逆时针方向，否则顺时针方向。

        注意：arc()函数中表示角的单位是弧度，不是角度。角度与弧度的js表达式:

        弧度=(Math.PI/180)*角度。

## 矩形

        直接在画布上绘制矩形的三个额外方法，正如我们开始所见的绘制矩形，同样，也有rect()方法，将一个矩形路径增加到当前路径上。

        rect(x, y, width, height)
        绘制一个左上角坐标为（x,y），宽高为width以及height的矩形。
        当该方法执行的时候，moveTo()方法自动设置坐标参数（0,0）。也就是说，当前笔触自动重置回默认坐标。


## Path2D 对象

        Path2D()会返回一个新初始化的Path2D对象（可能将某一个路径作为变量——创建一个它的副本，或者将一个包含SVG path数据的字符串作为变量）。
        new Path2D();     // 空的Path对象
        new Path2D(path); // 克隆Path对象
        new Path2D(d);    // 从SVG建立Path对象
        在这个例子中，我们创造了一个矩形和一个圆。它们都被存为Path2D对象，后面再派上用场。随着新的Path2D API产生，几种方法也相应地被更新来使用Path2D对象而不是当前路径。在这里，带路径参数的stroke和fill可以把对象画在画布上。
        function draw() {
        var canvas = document.getElementById('canvas');
        if (canvas.getContext){
            var ctx = canvas.getContext('2d');

            var rectangle = new Path2D();
            rectangle.rect(10, 10, 50, 50);

            var circle = new Path2D();
            circle.moveTo(125, 35);
            circle.arc(100, 35, 25, 0, 2 * Math.PI);

            ctx.stroke(rectangle);
            ctx.fill(circle);
        }
        }


# 色彩 Colors


        fillStyle = color
        设置图形的填充颜色。
        strokeStyle = color
        设置图形轮廓的颜色。

__注意: 一旦您设置了 strokeStyle 或者 fillStyle 的值，那么这个新值就会成为新绘制的图形的默认值。如果你要给每个图形上不同的颜色，你需要重新设置 fillStyle 或 strokeStyle 的值。__

## 颜色标准

        // 这些 fillStyle 的值均为 '橙色'
        ctx.fillStyle = "orange";
        ctx.fillStyle = "#FFA500";
        ctx.fillStyle = "rgb(255,165,0)";
        ctx.fillStyle = "rgba(255,165,0,1)";


# 透明度 globalAlpha

__globalAlpha = transparencyValue  
这个属性影响到 canvas 里所有图形的透明度，有效的值范围是 0.0 （完全透明）到 1.0（完全不透明），默认是 1.0。__


# 线型 Line styles

>可以通过一系列属性来设置线的样式。  

1. lineWidth 属性  
这个属性设置当前绘线的粗细。属性值必须为正数。默认值是1.0。

>较为准确的情况下，由于机制为由中心向两边延伸，宽度为1时可坐标可多加0.5

2. lineCap 属性  
属性 lineCap 的值决定了线段端点显示的样子。它可以为下面的三种的其中之一：butt，round 和 square。默认是 butt。square默认比butt长出宽度的一半

3. lineJoin 属性  
lineJoin 的属性值决定了图形中两线段连接处所显示的样子。它可以是这三种之一：round, bevel 和 miter。默认是 miter。拐角的连接样式

4. miterLimit 属性  
miterLimit 属性就是用来设定外延交点与连接点的最大距离，如果交点距离大于此值，连接效果会变成了 bevel。

5. 虚线  
用 setLineDash 方法和 lineDashOffset 属性来制定虚线样式. setLineDash 方法接受一个数组，来指定线段与间隙的交替；lineDashOffset 属性设置起始偏移量.

    ctx.setLineDash([4, 2]);
    ctx.lineDashOffset = -offset;

# 渐变 Gradients

就好像一般的绘图软件一样，我们可以用线性或者径向的渐变来填充或描边。我们用下面的方法新建一个 canvasGradient 对象，并且赋给图形的 fillStyle 或 strokeStyle 属性。

    createLinearGradient(x1, y1, x2, y2)
    createLinearGradient 方法接受 4 个参数，表示渐变的起点 (x1,y1) 与终点 (x2,y2)。
    createRadialGradient(x1, y1, r1, x2, y2, r2)
    createRadialGradient 方法接受 6 个参数，前三个定义一个以 (x1,y1) 为原点，半径为 r1 的圆，后三个参数则定义另一个以 (x2,y2) 为原点，半径为 r2 的圆。

    var lineargradient = ctx.createLinearGradient(0,0,150,150);
    var radialgradient = ctx.createRadialGradient(75,75,0,75,75,100);

    创建出 canvasGradient 对象后，我们就可以用 addColorStop 方法给它上色了。

    gradient.addColorStop(position, color)
    addColorStop 方法接受 2 个参数，position 参数必须是一个 0.0 与 1.0 之间的数值，表示渐变中颜色所在的相对位置。例如，0.5 表示颜色会出现在正中间。color 参数必须是一个有效的 CSS 颜色值（如 #FFF， rgba(0,0,0,1)，等等）。
    你可以根据需要添加任意多个色标（color stops）。下面是最简单的线性黑白渐变的例子。

    var lineargradient = ctx.createLinearGradient(0,0,150,150);
    lineargradient.addColorStop(0,'white');
    lineargradient.addColorStop(1,'black');


    例子
    function draw() {
    var ctx = document.getElementById('canvas').getContext('2d');

    // 创建渐变
    var radgrad = ctx.createRadialGradient(45,45,10,52,50,30);
    radgrad.addColorStop(0, '#A7D30C');
    radgrad.addColorStop(0.9, '#019F62');
    radgrad.addColorStop(1, 'rgba(1,159,98,0)');
    
    var radgrad2 = ctx.createRadialGradient(105,105,20,112,120,50);
    radgrad2.addColorStop(0, '#FF5F98');
    radgrad2.addColorStop(0.75, '#FF0188');
    radgrad2.addColorStop(1, 'rgba(255,1,136,0)');

    var radgrad3 = ctx.createRadialGradient(95,15,15,102,20,40);
    radgrad3.addColorStop(0, '#00C9FF');
    radgrad3.addColorStop(0.8, '#00B5E2');
    radgrad3.addColorStop(1, 'rgba(0,201,255,0)');

    var radgrad4 = ctx.createRadialGradient(0,150,50,0,140,90);
    radgrad4.addColorStop(0, '#F4F201');
    radgrad4.addColorStop(0.8, '#E4C700');
    radgrad4.addColorStop(1, 'rgba(228,199,0,0)');
    
    // 画图形
    ctx.fillStyle = radgrad4;
    ctx.fillRect(0,0,150,150);
    ctx.fillStyle = radgrad3;
    ctx.fillRect(0,0,150,150);
    ctx.fillStyle = radgrad2;
    ctx.fillRect(0,0,150,150);
    ctx.fillStyle = radgrad;
    ctx.fillRect(0,0,150,150);
    }


# 阴影 Shadows

    shadowOffsetX = float
    shadowOffsetX 和 shadowOffsetY 用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，它们默认都为 0。

    shadowOffsetY = float
    shadowOffsetX 和 shadowOffsetY 用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，它们默认都为 0。
    shadowBlur = float
    shadowBlur 用于设定阴影的模糊程度，其数值并不跟像素数量挂钩，也不受变换矩阵的影响，默认为 0。
    shadowColor = color
    shadowColor 是标准的 CSS 颜色值，用于设定阴影颜色效果，默认是全透明的黑色

    个例子绘制了带阴影效果的文字。

    function draw() {
    var ctx = document.getElementById('canvas').getContext('2d');

    ctx.shadowOffsetX = 2;
    ctx.shadowOffsetY = 2;
    ctx.shadowBlur = 2;
    ctx.shadowColor = "rgba(0, 0, 0, 0.5)";
    
    ctx.font = "20px Times New Roman";
    ctx.fillStyle = "Black";
    ctx.fillText("Sample String", 5, 30);
    }

# Canvas 填充规则

当我们用到 fill（或者 clip和isPointinPath ）你可以选择一个填充规则，该填充规则根据某处在路径的外面或者里面来决定该处是否被填充，这对于自己与自己路径相交或者路径被嵌套的时候是有用的。

两个可能的值：

 "nonzero": non-zero winding rule, 默认值.
 "evenodd":  even-odd winding rule.

    function draw() {
    var ctx = document.getElementById('canvas').getContext('2d'); 
    ctx.beginPath(); 
    ctx.arc(50, 50, 30, 0, Math.PI*2, true);
    ctx.arc(50, 50, 15, 0, Math.PI*2, true);
    ctx.fill("evenodd");
    }

# 绘制文本

## 绘制文本

        canvas 提供了两种方法来渲染文本:蛀牙是实心空心的区别

        fillText(text, x, y [, maxWidth])
        在指定的(x,y)位置填充指定的文本，绘制的最大宽度是可选的.
        strokeText(text, x, y [, maxWidth])
        在指定的(x,y)位置绘制文本边框，绘制的最大宽度是可选的.

## 有样式的文本
在上面的例子用我们已经使用了 font 来使文本比默认尺寸大一些. 还有更多的属性可以让你改变canvas显示文本的方式：

    font = value
    当前我们用来绘制文本的样式. 这个字符串使用和 CSS font 属性相同的语法. 默认的字体是 10px sans-serif。
    textAlign = value
    文本对齐选项. 可选的值包括：start, end, left, right or center. 默认值是 start。
    textBaseline = value
    基线对齐选项. 可选的值包括：top, hanging, middle, alphabetic, ideographic, bottom。默认值是 alphabetic。
    direction = value
    文本方向。可能的值包括：ltr, rtl, inherit。默认值是 inherit。

## 预测量文本宽度

measureText()
将返回一个 TextMetrics对象的宽度、所在像素，这些体现文本特性的属性。
下面的代码段将展示如何测量文本来获得它的宽度：

    function draw() {
    var ctx = document.getElementById('canvas').getContext('2d');
    var text = ctx.measureText("foo"); // TextMetrics object
    text.width; // 16;
    }

# 绘制图片

    function draw() {
        var ctx = document.getElementById('canvas').getContext('2d');
        var img = new Image();
        img.onload = function(){
        ctx.drawImage(img,0,0);
        ctx.beginPath();
        ctx.moveTo(30,96);
        ctx.lineTo(70,66);
        ctx.lineTo(103,76);
        ctx.lineTo(170,15);
        ctx.stroke();
        }
        img.src = 'images/backdrop.png';
    }

## 缩放 Scaling

    drawImage(image, x, y, width, height)
    这个方法多了2个参数：width 和 height，这两个参数用来控制 当向canvas画入时应该缩放的大小

## 切片 Slicing

    drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)
    第一个参数和其它的是相同的，都是一个图像或者另一个 canvas 的引用。其它8个参数最好是参照右边的图解，前4个是定义图像源的切片位置和大小，后4个则是定义切片的目标显示位置和大小。

    思考：可用画框和图片实现多个画廊

## 状态的保存和恢复

    save()
    保存画布(canvas)的所有状态
    restore()
    save 和 restore 方法是用来保存和恢复 canvas 状态的，都没有参数。Canvas 的状态就是当前画面应用的所有样式和变形的一个快照。

Canvas状态存储在栈中，每当save()方法被调用后，当前的状态就被推送到栈中保存。一个绘画状态包括：
* 当前应用的变形（即移动，旋转和缩放，见下）
* 以及下面这些属性：strokeStyle, fillStyle, globalAlpha, lineWidth, lineCap, lineJoin, miterLimit, lineDashOffset, shadowOffsetX, shadowOffsetY, shadowBlur, shadowColor, globalCompositeOperation, font, textAlign, textBaseline, direction, imageSmoothingEnabled
* 当前的裁切路径（clipping path），会在下一节介绍  
你可以调用任意多次 save方法。每一次调用 restore 方法，上一个保存的状态就从栈中弹出，所有设定都恢复。

## 移动 Translating

    translate(x, y)
    translate 方法接受两个参数。x 是左右偏移量，y 是上下偏移量，如右图所示。

## 旋转 Rotating

它用于以原点为中心旋转 canvas。

    rotate(angle)  
    这个方法只接受一个参数：旋转的角度(angle)，它是顺时针方向的，以弧度为单位的值。  
    旋转的中心点始终是 canvas 的原点，如果要改变它，我们需要用到 translate 方法。

## 缩放 Scaling

我们用它来增减图形在 canvas 中的像素数目，对形状，位图进行缩小或者放大。  

    scale(x, y)  
    scale  方法可以缩放画布的水平和垂直的单位。两个参数都是实数，可以为负数，x 为水平缩放因子，y 为垂直缩放因子，如果比1小，会比缩放图形， 如果比1大会放大图形。默认值为1， 为实际大小。  
    画布初始情况下， 是以左上角坐标为原点的第一象限。如果参数为负实数， 相当于以x 或 y轴作为对称轴镜像反转（例如， 使用translate(0,canvas.height); scale(1,-1); 以y轴作为对称轴镜像反转， 就可得到著名的笛卡尔坐标系，左下角为原点）。  

    默认情况下，canvas 的 1 个单位为 1 个像素。举例说，如果我们设置缩放因子是 0.5，1 个单位就变成对应 0.5 个像素，这样绘制出来的形状就会是原先的一半。同理，设置为 2.0 时，1 个单位就对应变成了 2 像素，绘制的结果就是图形放大了 2 倍。

## 变形 Transforms

允许对变形矩阵直接修改。

    transform(a, b, c, d, e, f)
    这个方法是将当前的变形矩阵乘上一个基于自身参数的矩阵，如下面的矩阵所示：[ a	c	e	b	d	f	0	0	1 ]
    如果任意一个参数是Infinity，变形矩阵也必须被标记为无限大，否则会抛出异常。
    这个函数的参数各自代表如下：

    a (m11)
    水平方向的缩放
    b(m12)
    水平方向的倾斜偏移
    c(m21)
    竖直方向的倾斜偏移
    d(m22)
    竖直方向的缩放
    e(dx)
    水平方向的移动
    f(dy)
    竖直方向的移动
    setTransform(a, b, c, d, e, f)
    这个方法会将当前的变形矩阵重置为单位矩阵，然后用相同的参数调用 transform 方法。如果任意一个参数是无限大，那么变形矩阵也必须被标记为无限大，否则会抛出异常。从根本上来说，该方法是取消了当前变形,然后设置为指定的变形,一步完成。
    resetTransform()
    重置当前变形为单位矩阵，它和调用以下语句是一样的：ctx.setTransform(1, 0, 0, 1, 0, 0);

## 新图案覆盖原则

__globalCompositeOperation__
    globalCompositeOperation = type
    这个属性设定了在画新图形时采用的遮盖策略，其值是一个标识12种遮盖方式的字符串。

    source-over
    这是默认设置，并在现有画布上下文之上绘制新图形。

    
    source-in
    新图形只在新图形和目标画布重叠的地方绘制。其他的都是透明的。

    source-out
    在不与现有画布内容重叠的地方绘制新图形。

    source-atop
    新图形只在与现有画布内容重叠的地方绘制。

    destination-over
    在现有的画布内容后面绘制新的图形。

    destination-in
    现有的画布内容保持在新图形和现有画布内容重叠的位置。其他的都是透明的。

    destination-out
    现有内容保持在新图形不重叠的地方。

    destination-atop
    现有的画布只保留与新图形重叠的部分，新的图形是在画布内容后面绘制的

    lighter
    两个重叠图形的颜色是通过颜色值相加来确定的。

    copy
    只显示新图形。

    xor
    图像中，那些重叠和正常绘制之外的其他地方是透明的。

    multiply
    将顶层像素与底层相应像素相乘，结果是一幅更黑暗的图片。

    screen
    像素被倒转，相乘，再倒转，结果是一幅更明亮的图片。

    overlay
    multiply和screen的结合，原本暗的地方更暗，原本亮的地方更亮。

    darken
    保留两个图层中最暗的像素。

    lighten
    保留两个图层中最亮的像素。

    color-dodge
    将底层除以顶层的反置。

    color-burn
    将反置的底层除以顶层，然后将结果反过来。

    hard-light
    屏幕相乘（A combination of multiply and screen）类似于叠加，但上下图层互换了

    soft-light
    用顶层减去底层或者相反来得到一个正值。

    difference
    一个柔和版本的强光（hard-light）。纯黑或纯白不会导致纯黑或纯白。

    exclusion
    和difference相似，但对比度较低。

    hue
    保留了底层的亮度（luma）和色度（chroma），同时采用了顶层的色调（hue）。

    saturation
    保留底层的亮度（luma）和色调（hue），同时采用顶层的色度（chroma）。

    color
    保留了底层的亮度（luma），同时采用了顶层的色调(hue)和色度(chroma)。

    luminosity
    保持底层的色调（hue）和色度（chroma），同时采用顶层的亮度（luma）。

# 裁切路径

裁切路径和普通的 canvas 图形差不多，不同的是它的作用是遮罩，用来隐藏不需要的部分。如右图所示。红边五角星就是裁切路径，所有在路径以外的部分都不会在 canvas 上绘制出来。

如果和上面介绍的 globalCompositeOperation 属性作一比较，它可以实现与 source-in 和 source-atop差不多的效果。最重要的区别是裁切路径不会在 canvas 上绘制东西，而且它永远不受新图形的影响。这些特性使得它在特定区域里绘制图形时相当好用。

在 绘制图形 一章中，只介绍了 stroke 和 fill 方法，这里介绍第三个方法clip

    clip()
    将当前正在构建的路径转换为当前的裁剪路径。

    默认情况下，canvas 有一个与它自身一样大的裁切路径（也就是没有裁切效果）。

    这个例子，我会用一个圆形的裁切路径来限制随机星星的绘制区域。

    function draw() {
    var ctx = document.getElementById('canvas').getContext('2d');
    ctx.fillRect(0,0,150,150);
    ctx.translate(75,75);

    // Create a circular clipping path
    ctx.beginPath();
    ctx.arc(0,0,60,0,Math.PI*2,true);
    ctx.clip();

    // draw background
    var lingrad = ctx.createLinearGradient(0,-75,0,75);
    lingrad.addColorStop(0, '#232256');
    lingrad.addColorStop(1, '#143778');
    
    ctx.fillStyle = lingrad;
    ctx.fillRect(-75,-75,150,150);

    // draw stars
    for (var j=1;j<50;j++){
        ctx.save();
        ctx.fillStyle = '#fff';
        ctx.translate(75-Math.floor(Math.random()*150),
                    75-Math.floor(Math.random()*150));
        drawStar(ctx,Math.floor(Math.random()*4)+2);
        ctx.restore();
    }
    
    }
    function drawStar(ctx,r){
    ctx.save();
    ctx.beginPath()
    ctx.moveTo(r,0);
    for (var i=0;i<9;i++){
        ctx.rotate(Math.PI/5);
        if(i%2 == 0) {
        ctx.lineTo((r/0.525731)*0.200811,0);
        } else {
        ctx.lineTo(r,0);
        }
    }
    ctx.closePath();
    ctx.fill();
    ctx.restore();
    }

    首先，我画了一个与 canvas 一样大小的黑色方形作为背景，然后移动原点至中心点。然后用 clip 方法创建一个弧形的裁切路径。裁切路径也属于 canvas 状态的一部分，可以被保存起来。如果我们在创建新裁切路径时想保留原来的裁切路径，我们需要做的就是保存一下 canvas 的状态。

    裁切路径创建之后所有出现在它里面的东西才会画出来。在画线性渐变时我们就会注意到这点。然后会绘制出50 颗随机位置分布（经过缩放）的星星，当然也只有在裁切路径里面的星星才会绘制出来。

# 基本动画

## 动画的基本步骤
你可以通过以下的步骤来画出一帧:

1. 清空 canvas
除非接下来要画的内容会完全充满 canvas （例如背景图），否则你需要清空所有。最简单的做法就是用 clearRect 方法。
2. 保存 canvas 状态
如果你要改变一些会改变 canvas 状态的设置（样式，变形之类的），又要在每画一帧之时都是原始状态的话，你需要先保存一下。
3. 绘制动画图形（animated shapes）
这一步才是重绘动画帧。
4. 恢复 canvas 状态
如果已经保存了 canvas 的状态，可以先恢复它，然后重绘下一帧。

## 有安排的更新画布 Scheduled updates
首先，可以用window.setInterval(), window.setTimeout(),和window.requestAnimationFrame()来设定定期执行一个指定函数。

    setInterval(function, delay)
    当设定好间隔时间后，function会定期执行。
    setTimeout(function, delay)
    在设定好的时间之后执行函数
    requestAnimationFrame(callback)
    告诉浏览器你希望执行一个动画，并在重绘之前，请求浏览器执行一个特定的函数来更新动画。
    如果你并不需要与用户互动，你可以使用setInterval()方法，它就可以定期执行指定代码。如果我们需要做一个游戏，我们可以使用键盘或者鼠标事件配合上setTimeout()方法来实现。通过设置事件监听，我们可以捕捉用户的交互，并执行相应的动作。

下面的例子，采用 window.requestAnimationFrame()实现动画效果。这个方法提供了更加平缓并更加有效率的方式来执行动画，当系统准备好了重绘条件的时候，才调用绘制动画帧。一般每秒钟回调函数执行60次，也有可能会被降低。想要了解更多关于动画循环的信息，尤其是游戏，可以在Game development zone 参考这篇文章 Anatomy of a video game。

## 鼠标追踪动画

    <!DOCTYPE html>
    <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <meta http-equiv="X-UA-Compatible" content="ie=edge">
            <title>Document</title>
            <script>
                var cn;
                //= document.getElementById('cw');
                var c;
                var u = 10;
                const m = {
                    x: innerWidth / 2,
                    y: innerHeight / 2
                };
                window.onmousemove = function(e) {
                    m.x = e.clientX;
                    m.y = e.clientY;

                }
                function gc() {
                    var s = "0123456789ABCDEF";
                    var c = "#";
                    for (var i = 0; i < 6; i++) {
                        c += s[Math.ceil(Math.random() * 15)]
                    }
                    return c
                }
                var a = [];
                window.onload = function myfunction() {
                    cn = document.getElementById('cw');
                    c = cn.getContext('2d');

                    for (var i = 0; i < 10; i++) {
                        var r = 30;
                        var x = Math.random() * (innerWidth - 2 * r) + r;
                        var y = Math.random() * (innerHeight - 2 * r) + r;
                        var t = new ob(innerWidth / 2,innerHeight / 2,5,"red",Math.random() * 200 + 20,2);
                        a.push(t);
                    }
                    //cn.style.backgroundColor = "#700bc8";

                    c.lineWidth = "2";
                    c.globalAlpha = 0.5;
                    resize();
                    anim()
                }
                window.onresize = function() {

                    resize();

                }
                function resize() {
                    cn.height = innerHeight;
                    cn.width = innerWidth;
                    for (var i = 0; i < 101; i++) {
                        var r = 30;
                        var x = Math.random() * (innerWidth - 2 * r) + r;
                        var y = Math.random() * (innerHeight - 2 * r) + r;
                        a[i] = new ob(innerWidth / 2,innerHeight / 2,4,gc(),Math.random() * 200 + 20,0.02);

                    }
                    //  a[0] = new ob(innerWidth / 2, innerHeight / 2, 40, "red", 0.05, 0.05);
                    //a[0].dr();
                }
                function ob(x, y, r, cc, o, s) {
                    this.x = x;
                    this.y = y;
                    this.r = r;
                    this.cc = cc;
                    this.theta = Math.random() * Math.PI * 2;
                    this.s = s;
                    this.o = o;
                    this.t = Math.random() * 150;

                    this.o = o;
                    this.dr = function() {
                        const ls = {
                            x: this.x,
                            y: this.y
                        };
                        this.theta += this.s;
                        this.x = m.x + Math.cos(this.theta) * this.t;
                        this.y = m.y + Math.sin(this.theta) * this.t;
                        c.beginPath();
                        c.lineWidth = this.r;
                        c.strokeStyle = this.cc;
                        c.moveTo(ls.x, ls.y);
                        c.lineTo(this.x, this.y);
                        c.stroke();
                        c.closePath();

                    }
                }
                function anim() {
                    requestAnimationFrame(anim);
                    c.fillStyle = "rgba(0,0,0,0.05)";
                    c.fillRect(0, 0, cn.width, cn.height);
                    a.forEach(function(e, i) {
                        e.dr();
                    });

                }
            </script>
            <style>
                #cw {
                    position: fixed;
                    z-index: -1;
                }

                body {
                    margin: 0;
                    padding: 0;
                    background-color: rgba(0,0,0,0.05);
                }
            </style>
        </head>
        <body>
            <canvas id="cw"></canvas>
            qwerewr    

        </body>
    </html>

# 像素操作

## ImageData 对象

ImageData对象中存储着canvas对象真实的像素数据，它包含以下几个只读属性：

    width

    图片宽度，单位是像素
    height
    图片高度，单位是像素
    data
    Uint8ClampedArray类型的一维数组，包含着RGBA格式的整型数据，范围在0至255之间（包括255）。

data属性返回一个 Uint8ClampedArray，它可以被使用作为查看初始像素数据。每个像素用4个1bytes值(按照红，绿，蓝和透明值的顺序; 这就是"RGBA"格式) 来代表。每个颜色值部份用0至255来代表。每个部份被分配到一个在数组内连续的索引，左上角像素的红色部份在数组的索引0位置。像素从左到右被处理，然后往下，遍历整个数组。

Uint8ClampedArray  包含高度 × 宽度 × 4 bytes数据，索引值从0到(高度×宽度×4)-1  

例如，要读取图片中位于第50行，第200列的像素的蓝色部份，你会写以下代码：

    blueComponent = imageData.data[((50 * (imageData.width * 4)) + (200 * 4)) + 2];

根据行、列读取某像素点的R/G/B/A值的公式：

    imageData.data[((50 * (imageData.width * 4)) + (200 * 4)) + 0/1/2/3];
你可能用会使用Uint8ClampedArray.length属性来读取像素数组的大小（以bytes为单位）：

    var numBytes = imageData.data.length;

## 创建一个ImageData对象
去创建一个新的，空白的ImageData对象，你应该会使用createImageData() 方法。有2个版本的createImageData()方法。

    var myImageData = ctx.createImageData(width, height);

上面代码创建了一个新的具体特定尺寸的ImageData对象。所有像素被预设为透明黑。

你也可以创建一个被anotherImageData对象指定的相同像素的ImageData对象。这个新的对象像素全部被预设为透明黑。这个并非复制了图片数据。

    var myImageData = ctx.createImageData(anotherImageData);

## 得到场景像素数据

为了获得一个包含画布场景像素数据的ImageData对像，你可以用getImageData()方法：

    var myImageData = ctx.getImageData(left, top, width, height);
    这个方法会返回一个ImageData对象，它代表了画布区域的对象数据，此画布的四个角落分别表示为(left, top), (left + width, top), (left, top + height), 以及(left + width, top + height)四个点。这些坐标点被设定为画布坐标空间元素。

注：任何在画布以外的元素都会被返回成一个透明黑的ImageData对像。

## 在场景中写入像素数据

你可以用putImageData()方法去对场景进行像素数据的写入。

    ctx.putImageData(myImageData, dx, dy);
    dx和dy参数表示你希望在场景内左上角绘制的像素数据所得到的设备坐标。

## 缩放和反锯齿

CanvasRenderingContext2D.imageSmoothingEnabled 是 Canvas 2D API 用来设置图片是否平滑的属性，true表示图片平滑（默认值），false表示图片不平滑。当我们获取 imageSmoothingEnabled 属性值时， 它会返回最新设置的值。

 以缩放画布为例，这个属性对像素为主的游戏很有用。默认的改变大小的算法会造成图片模糊并且破坏图片原有的像素。 如果那样的话，设置属性值为false。 参见 CSS image-rendering 属性。

注意:您可以使用imageSmoothingQuality属性来调整平滑质量。

    语法
    ctx.imageSmoothingEnabled = value;
    选项
    value
    一个Boolean 类型的值，表示图片是否平滑。


        <canvas id="zoom" width="300" height="227"></canvas>
        <div>
        <label for="smoothbtn">
        <input type="checkbox" name="smoothbtn" checked="checked" id="smoothbtn">
        Enable image smoothing
        </label>
        </div>
        var img = new Image();
        img.src = 'https://mdn.mozillademos.org/files/5397/rhino.jpg';
        img.onload = function() {
        draw(this);
        };

        function draw(img) {
        var canvas = document.getElementById('canvas');
        var ctx = canvas.getContext('2d');
        ctx.drawImage(img, 0, 0);
        img.style.display = 'none';
        var zoomctx = document.getElementById('zoom').getContext('2d');
        
        var smoothbtn = document.getElementById('smoothbtn');
        var toggleSmoothing = function(event) {
            zoomctx.imageSmoothingEnabled = this.checked;
            zoomctx.mozImageSmoothingEnabled = this.checked;
            zoomctx.webkitImageSmoothingEnabled = this.checked;
            zoomctx.msImageSmoothingEnabled = this.checked;
        };
        smoothbtn.addEventListener('change', toggleSmoothing);

        var zoom = function(event) {
            var x = event.layerX;
            var y = event.layerY;
            zoomctx.drawImage(canvas,
                            Math.abs(x - 5),
                            Math.abs(y - 5),
                            10, 10,
                            0, 0,
                            200, 200);
        };

        canvas.addEventListener('mousemove', zoom);
        }

## 保存图片

HTMLCanvasElement  提供一个toDataURL()方法，此方法在保存图片的时候非常有用。它返回一个包含被类型参数规定的图像表现格式的数据链接。返回的图片分辨率是96dpi。

    canvas.toDataURL('image/png')
    默认设定。创建一个PNG图片。
    Default setting. Creates a PNG image.
    canvas.toDataURL('image/jpeg', quality)
    创建一个JPG图片。你可以有选择地提供从0到1的品质量，1表示最好品质，0基本不被辨析但有比较小的文件大小。
    当你从画布中生成了一个数据链接，例如，你可以将它用于任何<image>元素，或者将它放在一个有download属性的超链接里用于保存到本地。

    你也可以从画布中创建一个Blob对像。

    canvas.toBlob(callback, type, encoderOptions)


# 点击区域和无障碍访问

## 点击区域

点击区域（hit region）
判断鼠标坐标是否在canvas上一个特定区域里一直是个有待解决的问题。hit region API让你可以在canvas上定义一个区域，这让无障碍工具获取canvas上的交互内容成为可能。它能让你更容易地进行点击检测并把事件转发到DOM元素去。这个API有以下三个方法（都是实验性特性，请先在浏览器兼容表上确认再使用）。

    CanvasRenderingContext2D.addHitRegion() 
    在canvas上添加一个点击区域。
    CanvasRenderingContext2D.removeHitRegion() 
    从canvas上移除指定id的点击区域。
    CanvasRenderingContext2D.clearHitRegions() 
    移除canvas上的所有点击区域。

你可以把一个点击区域添加到路径里并检测MouseEvent.region属性来测试你的鼠标有没有点击这个区域，例：

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
    addHitRegion()方法也可以带一个control选项来指定把事件转发到哪个元素上（canvas里的元素）。

    ctx.addHitRegion({control: element});
    把这个特性用在<input>元素上会很有用。

## 焦点圈

当用键盘控制时，焦点圈是一个能帮我们在页面上快速导航的标记。要在canvas上绘制焦点圈，可以使用drawFocusIfNeeded 属性。

    CanvasRenderingContext2D.drawFocusIfNeeded() 
    如果给定的元素获得了焦点，这个方法会沿着在当前的路径画个焦点圈。
    另外，scrollPathIntoView()方法可以让一个元素获得焦点的时候在屏幕上可见(滚动到元素所在的区域)。

    CanvasRenderingContext2D.scrollPathIntoView() 
    把当前的路径或者一个给定的路径滚动到显示区域内。


## 内容兼容

\<canvas\> ... \</canvas\>标签里的内容被可以对一些不支持canvas的浏览器提供兼容。这对残疾用户设备也很有用（比如屏幕阅读器），这样它们就可以读取并解释DOM里的子节点。在html5accessibility.com有个很好的例子来演示它如何运作。

    <canvas> 
    <h2>Shapes</h2> 
    <p>A rectangle with a black border. 
    In the background is a pink circle. 
    Partially overlaying the <a href="http://en.wikipedia.org/wiki/Circle" onfocus="drawCircle();" onblur="drawPicture();">circle</a>. 
    Partially overlaying the circle is a green 
    <a href="http://en.wikipedia.org/wiki/Square" onfocus="drawSquare();" onblur="drawPicture();">square</a> 
    and a purple <a href="http://en.wikipedia.org/wiki/Triangle" onfocus="drawTriangle();" onblur="drawPicture();">triangle</a>,
    both of which are semi-opaque, so the full circle can be seen underneath.</p> 
    </canvas>

## ARIA 规则

Accessible Rich Internet Applications (ARIA) 定义了让Web内容和Web应用更容易被有身体缺陷的人获取的办法。你可以用ARIA属性来描述canvas元素的行为和存在目的。详情见ARIA和 ARIA 技术。

    <canvas id="button" tabindex="0" role="button" aria-pressed="false" aria-label="Start game"></canvas>


# 性能优化

\<canvas\>元素是众多广泛使用的网络2D图像渲染标准之一。它被广泛用于游戏及复杂的图像可视化中。然而，随着网站和应用将canvas画布推至极限，性能开始成为问题。此文目标是给使用canvas画布元素的优化带来建议，去保证你的网站或者应用表现卓越。

性能贴士
下面是一些改善性能的建议

## 在离屏canvas上预渲染相似的图形或重复的对象

如果发现自己在每个动画帧上重复了一些相同的绘制操作，请考虑将其分流到屏幕外的画布上。 然后，您可以根据需要频繁地将屏幕外图像渲染到主画布上，而不必首先重复生成该图像的步骤。

    myEntity.offscreenCanvas = document.createElement("canvas");
    myEntity.offscreenCanvas.width = myEntity.width;
    myEntity.offscreenCanvas.height = myEntity.height;
    myEntity.offscreenContext = myEntity.offscreenCanvas.getContext("2d");

    myEntity.render(myEntity.offscreenContext);

## 避免浮点数的坐标点，用整数取而代之

当你画一个没有整数坐标点的对象时会发生子像素渲染。

    ctx.drawImage(myImage, 0.3, 0.5);
    
浏览器为了达到抗锯齿的效果会做额外的运算。为了避免这种情况，请保证在你调用drawImage()函数时，用Math.floor()函数对所有的坐标点取整。

## 不要在用drawImage时缩放图像_
在离屏canvas中缓存图片的不同尺寸，而不要用drawImage()去缩放它们。

## 使用多层画布去画一个复杂的场景
在您的应用程序中，您可能会发现某些对象需要经常移动或更改，而其他对象则保持相对静态。 在这种情况下，可能的优化是使用多个\<canvas\>元素对您的项目进行分层。

例如，假设您有一个游戏，其UI位于顶部，中间是游戏性动作，底部是静态背景。 在这种情况下，您可以将游戏分成三个\<canvas\>层。 UI将仅在用户输入时发生变化，游戏层随每个新框架发生变化，并且背景通常保持不变。

    <div id="stage">
    <canvas id="ui-layer" width="480" height="320"></canvas>
    <canvas id="game-layer" width="480" height="320"></canvas>
    <canvas id="background-layer" width="480" height="320"></canvas>
    </div>
    
    <style>
    #stage {
        width: 480px;
        height: 320px;
        position: relative;
        border: 2px solid black
    }
    canvas { position: absolute; }
    #ui-layer { z-index: 3 }
    #game-layer { z-index: 2 }
    #background-layer { z-index: 1 }
    </style>

## 用CSS设置大的背景图
如果像大多数游戏那样，你有一张静态的背景图，用一个静态的<div>元素，结合background 特性，以及将它置于画布元素之后。这么做可以避免在每一帧在画布上绘制大图。

## 用CSS transforms特性缩放画布
CSS transforms 使用GPU，因此速度更快。 最好的情况是不直接缩放画布，或者具有较小的画布并按比例放大，而不是较大的画布并按比例缩小。

    var scaleX = window.innerWidth / canvas.width;
    var scaleY = window.innerHeight / canvas.height;

    var scaleToFit = Math.min(scaleX, scaleY);
    var scaleToCover = Math.max(scaleX, scaleY);

    stage.style.transformOrigin = '0 0'; //scale from top left
    stage.style.transform = 'scale(' + scaleToFit + ')';

## 关闭透明度
如果你的游戏使用画布而且不需要透明，当使用 HTMLCanvasElement.getContext() 创建一个绘图上下文时把 alpha 选项设置为 false 。这个选项可以帮助浏览器进行内部优化。

    var ctx = canvas.getContext('2d', { alpha: false });

## 更多的贴士
- 将画布的函数调用集合到一起（例如，画一条折线，而不要画多条分开的直线）
- 避免不必要的画布状态改变
- 渲染画布中的不同点，而非整个新状态
- 尽可能避免 shadowBlur特性
- 尽可能避免text rendering
- 尝试不同的方法来清除画布(clearRect() vs. fillRect() vs. 调整   canvas大小)
- 有动画，请使用window.requestAnimationFrame() 而非window.setInterval()
- 请谨慎使用大型物理库
