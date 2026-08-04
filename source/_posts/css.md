---
title: css
date: 2020-07-22 17:00:21
tags: 
    - css
    - html居中的方法
    - css画三角形或者平行四边形
categories:
    - 前端
    - css
---
# html居中的方法

>可做对应修改值居中水平方向或者竖直方向
```css
{
    max-width: 100%;
    max-height: 100%;
    position: absolute;
    left: 50%;
    top: 50%;
    -webkit-transform: translate(-50%, -50%);
    transform: translate(-50%, -50%);
}
```

# css画三角形或者平行四边形

> 主要利用的边框拥有一定的厚度后，在转角处是以div的中心到角的线平分的，这个时候利用透明即可实现

```css
/* 三角形 */
div{
    height: 0;
    width: 0;
    border-left: 50px solid transparent;
    border-right: 50px solid transparent;
    border-bottom: 100px solid;
}

/* 平行四边形 */
div{
    height: 50px;
    width: 50px;
    border-left: 50px solid transparent;
    border-right: 50px solid transparent;
    border-bottom: 100px solid;
}
```

