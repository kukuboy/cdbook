---
title: rem相关
date: 2020-08-20 16:27:48
tags: 
    - rem
    - vue
categories: 
    - 前端
    - rem
---

# 使用

      let clientWidth = document.documentElement.clientWidth;
      if (!clientWidth) return;
      let pxOneRem = clientWidth / 7.5;
      document.documentElement.style.fontSize = pxOneRem + "px";
      
> 个人理解:设置html样式的font-size为10px,则1rem为未10px
> 在设备的适配中，尤其是手机设备中，由于不同设备的尺寸大小，纵横比差距过大，所以会导致使用
> px往往不能满足这些需求。使用百分比在纵横比不同的手机上表现差距也会很大。故可以采用rem。

> 通过获取设备html的宽度，然后除于10,100,7.5,25这些你自定义的值，在此以7.5为例。
> 那么7.5rem便是100vw，即宽度的100%。如果除的值是100，那么rem和vw便会有很多的相似点，
> 但也会有不同的地方，如若手机字体方法，则同时rem也会改变。
## 问题修复
> 那么在一些改变字体的手机上，可能会出现实际的rem值比你想要的偏大，导致页面效果异常。
> 旧手机这个现象比较常见，则可以采用下列的方法进行修复，
> 这里放在vue的钩子中

      data () {
        return {
            resizeEvt: "orientationchange" in window ? "orientationchange" : "resize"
        }
       }
      destroyed() {
        window.removeEventListener(this.resizeEvt, this.adjustRem(), false);
       },
       mounted() {
         this.init();
       },
       methods: {
         init() {
           // 调整rem尺寸
           this.adjustRem();
           // 修复字体放大手机上rem尺寸错误的问题
           window.addEventListener(this.resizeEvt, this.adjustRem(), false);
         },
         adjustRem() {
           console.log("触发了尺寸检测变化");
           let clientWidth = document.documentElement.clientWidth;
           if (!clientWidth) return;
           let pxOneRem = clientWidth / 7.5;
           document.documentElement.style.fontSize = pxOneRem + "px";
           // 纠错函数
           function adapt() {
             let d = window.document.createElement("div");
             d.style.width = "1rem";
             d.style.display = "none";
             let head = window.document.getElementsByTagName("head")[0];
             head.appendChild(d);
             let defaultFontSize = parseFloat(
               window.getComputedStyle(d, null).getPropertyValue("width")
             );
             return defaultFontSize;
           }
           pxOneRem = (pxOneRem * pxOneRem) / adapt();
           document.documentElement.style.fontSize = pxOneRem + "px";
         },
        }

> 实测有用, 如有问题评论区留言即可
