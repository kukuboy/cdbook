---
title: iosH5输入框解决办法
date: 2020-08-26 11:00:58
tags: 
    - ios
    - H5
    - 输入框
    - vue
categories:
    - 前端
    - vue
    - input输入框
---

# ios输入框问题解决方法总结

> 以vue为框架，但与其他差距不大

## 键盘上部分按键为英文的解决办法

> 以下解决方法以此输入框为基准
> 外部加上form包含即可解决

        <form action="javascript:return true">
            <div
            v-html="chat_input"
            @click="stopPromot"
            @input="inputDiv"
            @blur="blur"
            tabindex="1"
            @focus="focusChange"
            class="chat_input"
            contenteditable="true"
            ref="chat_input"
            @keydown="keydown($event)"
            placeholder="我来说两句…"
            ></div>
        </form>

## 第一次点击任何任何输入框均会获取焦点的问题

> WebkitUserSelect = 'text'样式不再写在css中

    stopPromot () {
        // 优化ios点击体验，修复第一次点击任何地方弹出键盘和有时点击为及时获取焦点的问题
            if (myUtil.checkPlatform() === 'Ios') {
                this.$refs.chat_input.style.WebkitUserSelect = 'text'
                this.toLastDiv(this.$refs.chat_input)
            }
        },

## 在ios中页面整体可滑动的问题

        body {
        /* 防止ios页面滑动 */
        overflow: hidden;
        margin: 0;
        padding: 0;
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
        position: fixed;
        /* 页面主题限制在安全区域内 */
        padding-bottom: constant(safe-area-insert-bottom);
        padding-bottom: env(safe-area-insert-bottom);
        }

## (重点)下列方法可解决，软键盘弹出时头部被顶出，或者输入框被盖住的问题

>此方法写在输入框的组件中

    // 原来是光标不能放在某些位置
    focusChange () {
      // 关闭表情面板
      console.log('获得焦点', document.body.scrollTop)
      this.$emit('focusChange', 0, true)
      this.shutDown()
      if (myUtil.checkPlatform() === 'Ios') {
        clearInterval(focusTime)
        focusTime = setInterval(() => {
          console.log(document.body.scrollTop)
          if (document.body.scrollTop !== 0) { clearInterval(focusTime) }
          this.$emit('focusChange', document.body.scrollTop)
        }, 30)
      } else {
        this.$emit('focusChange', 0, false, false)
      }
    },

> 此方法写在包含输入组件的页面中
> this.scroll(true)为输入框获取焦点时其他处理的部分，如聊天内容需要滚动到最下方
> bodyTouch的值为当前页面是否被触摸，下列介绍获取方法

        focusChange (h, blur = false, isIos = true) {
      // 处理安卓
      if (!isIos) {
        setTimeout(() => {
          this.scroll(true)
        }, 100)
        return
      }
      // 处理ios
      if (h > 0 && !blur) {
        h += 'px'
        document.body.style.height = 'calc(100% - ' + h + ')'
        document.body.scrollTop = h
        console.log('focusChange', h)
        clearInterval(this.bodyTouchTime)
        this.bodyTouchTime = setInterval(() => {
          if (this.bodyTouch) {
            return
          }
          document.body.scrollTop = h
        }, 10)
        setTimeout(() => {
          this.scroll(true)
        }, 100)
      } else if (blur) {
        clearInterval(this.bodyTouchTime)
        document.body.style.height = '100%'
      }
    },

> bodyTouchh获取

    mounted () {
        this.init()
    },
    destroyed () {
        clearInterval(this.bodyTouchTime)
        // 清除事件
        let _this = this
        document.removeEventListener('touchstart', function (e) {
        _this.bodyTouch = true
        }, false)
        document.removeEventListener('touchend', function (e) {
        _this.bodyTouch = false
        }, false)
    },
    methods: {
        init () {
        chatContent = this.$refs.chat_content
        chatContent.addEventListener('scroll', this.scrollListen, false)
        // 判断检测当前屏幕是否被触摸
        let _this = this
        document.addEventListener('touchstart', function (e) {
            _this.bodyTouch = true
        }, false)
        document.addEventListener('touchend', function (e) {
            _this.bodyTouch = false
        }, false)
        },
    }

## ios输入框获取焦点，光标移到最后

> obj即为输入框对象，可使输入框获取焦点，也可使光标移到最后

    // 光标移到最后
    toLastDiv (obj) {
      let range
      if (window.getSelection) {
        // ie11 10 9 ff safari
        obj.focus() // 解决ff不获取焦点无法定位问题
        range = window.getSelection() // 创建range
        range.selectAllChildren(obj) // range 选择obj下所有子内容
        range.collapseToEnd() // 光标移至最后
      } else if (document.selection) {
        // ie10 9 8 7 6 5
        range = document.selection.createRange() // 创建选择对象
        // var range = document.body.createTextRange();
        range.moveToElementText(obj) // range定位到obj
        range.collapse(false) // 光标移至最后
        range.select()
      }
    },