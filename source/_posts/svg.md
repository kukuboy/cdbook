---
title: svg
date: 2021-04-13 10:18:25
tags:
- svg
categories:
- 前端
- svg
---

# svg路径

## path

> <path> 标签用来定义路径。

### d

> 该属性定义了一个路径

+ 下面的命令可用于路径数据：

- M = moveto 
  + 移动到点对应位置，与上个点之间没有路径
  +  M x,y
- L = lineto
  + 与上个点之间绘制一条直线
  + L x, y
- H = horizontal lineto
  + 与上个点之间绘制一条水平的线
  + H x, y
- V = vertical lineto
  + 与上个点之间绘制一条垂直的线
  + V x, y
- C = curveto
  + Curvto命令指定了一个贝塞尔曲线。有两种类型的贝塞尔曲线：立方曲线和二次方曲线。
  + 二次方贝塞尔曲线是一种特殊的立方贝塞尔曲线，在这里，控制点的两端是相同的。二次方贝塞尔曲线的句法是”Q cx, cy  x, y“或”q dcx, dcy dx, dy“。cx和cy都是控制点的绝对坐标，而dcx和dcy分别是控制点在x和y方向上的距离。 
  + 立方贝赛尔曲线遵守二次方贝赛尔曲线同样的概念，但是它需要考虑两个控制点。立方贝塞尔曲线的句法是：”C c1x,c1y c2x,c2y x,y“或者”c dc1x,dc1y dc2x,dc2y dx,dy“，在这里，c1x、c1y和c2x、c2y是分别是初始点和结束点的控制点的绝对坐标。dc1x、dc1y和dc2x、dc2y都是相对于初始点，而不是相对于结束点的。dx和dy分别是向右和向下的距离。 
  + 最后，所有的贝塞尔曲线命令可以制作出一个多边贝塞尔图形，先初始化命令，然后多次指定所有的参数，就可以制作出一个多边贝赛尔图形。因此，下面的两个命令可以创建完全相同的路径：
    
        <path d="c 50,0 50,100 100,100 50,0 50,-100 100,-100" />
        <path d="c 50,0 50,100 100,100 c 50,0 50,-100 100,-100" />

- S = smooth curveto
  + 因为它假定第一个控制点是从前一个控制点关于前一个点的反射，或者说如果没有前一个控制点的话它实际上就是前一个点。
  + S用来创建立方贝塞尔曲线，语法是”S cx,cy x,y“或者”s dcx,dcy dx,dy“，在这里(d)cx指定第二个控制点。
- Q = quadratic Belzier curve
  + 二次方贝塞尔曲线的句法是”Q cx, cy  x, y“或”q dcx, dcy dx, dy“。cx和cy都是控制点的绝对坐标，而dcx和dcy分别是控制点在x和y方向上的距离。
- T = smooth quadratic Belzier curveto
  + 因为它假定第一个控制点是从前一个控制点关于前一个点的反射，或者说如果没有前一个控制点的话它实际上就是前一个点。
  + T的句法是”T x,y“或者”t dx,dy“，分别对应于绝对坐标和相对距离，用来创建二次方贝塞尔曲线
- A = elliptical Arc
  + ”A rx,ry xAxisRotate LargeArcFlag,SweepFlag x,y“。解构它，rx和ry分别是x和y方向的半径，而LargeArcFlag的值要到是0要么是1，用来确定是要画小弧（0）还是画大弧（1）。SweepFlag也要么是0要么是1，用来确定弧是顺时针方向（1）还是逆时针方向（0）。x和y是目的地的坐标。虽然xAxisRotate支持改变x轴相对于当前引用框架的方向，但是在Gecko 7中，这个参数看起来没什么效果。
- Z = closepath
  + ClosePath命令将在当前路径从，从当前点到第一个点简单画一条直线。它是最简单的命令

>注释：以上所有命令均允许小写字母。大写表示绝对定位，小写表示相对定位。

>请把下面的代码拷贝到记事本，然后把文件保存为 "path1.svg"。把此文件放入您的 web 目录：

    <?xml version="1.0" standalone="no"?>
    <!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" 
    "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
    
    <svg width="100%" height="100%" version="1.1"
    xmlns="http://www.w3.org/2000/svg">
    
    <path d="M250 150 L150 350 L350 350 Z" />
    
    </svg>
>上面的例子定义了一条路径，它开始于位置 250 150，到达位置 150 350，然后从那里开始到 350 350，最后在 250 150 关闭路径。


>下面的例子创建了一个螺旋：

    <?xml version="1.0" standalone="no"?>
    <!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" 
    "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
    
    <svg width="100%" height="100%" version="1.1"
    xmlns="http://www.w3.org/2000/svg">
    
    <path d="M153 334
    C153 334 151 334 151 334
    C151 339 153 344 156 344
    C164 344 171 339 171 334
    C171 322 164 314 156 314
    C142 314 131 322 131 334
    C131 350 142 364 156 364
    C175 364 191 350 191 334
    C191 311 175 294 156 294
    C131 294 111 311 111 334
    C111 361 131 384 156 384
    C186 384 211 361 211 334
    C211 300 186 274 156 274"
    style="fill:white;stroke:red;stroke-width:2"/>
    
    </svg>



## Stroke属性
### Stroke
>Stroke属性定义一条线，文本或元素轮廓颜色：

  <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <g fill="none">
  <path stroke="red" d="M5 20 l215 0" />
  <path stroke="blue" d="M5 40 l215 0" />
  <path stroke="black" d="M5 60 l215 0" />
  </g>
  </svg>

    下面是SVG代码：
    实例
    <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
    <g fill="none">
    <path stroke="red" d="M5 20 l215 0" />
    <path stroke="blue" d="M5 40 l215 0" />
    <path stroke="black" d="M5 60 l215 0" />
    </g>
    </svg>


### stroke-width 
>stroke- width属性定义了一条线，文本或元素轮廓厚度：

  <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <g fill="none" stroke="black">
  <path stroke-width="2" d="M5 20 l215 0" />
  <path stroke-width="4" d="M5 40 l215 0" />
  <path stroke-width="6" d="M5 60 l215 0" />
  </g>
  </svg>


下面是SVG代码：
    
    <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
    <g fill="none" stroke="black">
    <path stroke-width="2" d="M5 20 l215 0" />
    <path stroke-width="4" d="M5 40 l215 0" />
    <path stroke-width="6" d="M5 60 l215 0" />
    </g>
    </svg>

### stroke-linecap 
>strokelinecap属性定义不同类型的开放路径的终结：

实例

  <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <g fill="none" stroke="black" stroke-width="6">
  <path stroke-linecap="butt" d="M5 20 l215 0" />
  <path stroke-linecap="round" d="M5 40 l215 0" />
  <path stroke-linecap="square" d="M5 60 l215 0" />
  </g>
  </svg>

下面是SVG代码：

    <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
    <g fill="none" stroke="black" stroke-width="6">
    <path stroke-linecap="butt" d="M5 20 l215 0" />
    <path stroke-linecap="round" d="M5 40 l215 0" />
    <path stroke-linecap="square" d="M5 60 l215 0" />
    </g>
    </svg>

### stroke-dasharray 
>strokedasharray属性用于创建虚线：

实例

  <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <g fill="none" stroke="black" stroke-width="4">
  <path stroke-dasharray="5,5" d="M5 20 l215 0" />
  <path stroke-dasharray="10,10" d="M5 40 l215 0" />
  <path stroke-dasharray="20,10,5,5,5,10" d="M5 60 l215 0" />
  </g>
  </svg>

下面是SVG代码：

    <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
    <g fill="none" stroke="black" stroke-width="4">
    <path stroke-dasharray="5,5" d="M5 20 l215 0" />
    <path stroke-dasharray="10,10" d="M5 40 l215 0" />
    <path stroke-dasharray="20,10,5,5,5,10" d="M5 60 l215 0" />
    </g>
    </svg>
