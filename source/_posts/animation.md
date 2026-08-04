---
title: animation
date: 2020-07-23 10:05:22
tags: css
categories:
    - 前端
    - css
---

# Animation
## 作用:实现简单动画。
## 语法

    animation: name duration timing-function delay iteration-count direction fill-mode play-state;

其中，
1. name: animation-name简写，需要绑定的keyframe名称（其中规定了位置从哪儿到哪儿）
2. duration: animation-duration简写，规定整个过程花费时间。（单位：s/ms）
3. timing-function: animation-timing-function简写，整个过程的速度曲线（先快后慢ease-out、先慢后快ease-in、匀速linear等）
4. delay: animation-delay简写，动画开始之前的延迟。（单位：s/ms）
5. iteration-count： animation-iteration-count简写，动画循环次数。（无限infinite、自定义次数n）
6. direction： animation-direction：简写，是否轮流反方向播放。（normal正常播放、alternate轮流反方向播放）
7. fill-mode：animation-fill-mode简写，CSS动画在执行之前和之后如何将样式应用于其目标（none当动画未执行时，动画将不会将任何样式应用于目标，forwards目标将保留由执行期间遇到的最后一个关键帧计算值，backwards动画将在应用于目标时立即应用第一个关键帧中定义的值，both动画将遵循forwards和backwards的规则）
8. play-state：animation-play-state简写， CSS 属性定义一个动画是否运行或者暂停。（running当前动画正在运行，paused当前动画已被停止。）兼容性较差，尚在实验-2020-12

## 举个例子
   
    // html部分
     <div class="move"></div>
    // css部分
    @keyframes mymove{
        from{left: 0;}
        to{left: 80%;}
    }
    .move{
        height: 100px;
        width: 100px;
        position: relative;
        background: red;
        animation: mymove 5s infinite alternate linear;
    }

其中，
1. 注意@keyframes的用法，mymove就是animation中的name值，from规定起始位置，to规定结束位置。
2. 注意一定要设置相对定位。
3. 对于其中，参数位置的要求并没有十分严格，也就是说animation中顺序可以不按照语法中的顺序来写。