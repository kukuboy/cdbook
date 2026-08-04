---
title: vue混入文件的几大用法
date: 2020-12-01 14:06:34
tags: 
    - mixin混入
    - vue
categories: 
    - 前端
    - vue
    - mixin
    - mixin.js
---

# 使用

__该用法未采用全局混入，采用引入混用__
__文件格式为正常的vue文件__

方法为：

    import mixins from "@/util/mixins";

    export default {
    name: "NewGift",
    components: {},
    mixins: [mixins],
    }

# 显示页面加载时间

    created() {
        console.time("页面加载时间：");
        document.getElementById("app").classList.add("viewFilter");
        mixin_time = setTimeout(() => {
        console.log("开始出现加载条");
        this.$Dialog.ViewLoading();
        }, 1000);
    },
    mounted() {
        console.timeEnd("页面加载时间：");
        // setTimeout(() => {
        document.getElementById("app").classList.remove("viewFilter");
        this.$Dialog.ViewLoading({
        state: "end"
        });
        clearTimeout(mixin_time);
        // }, 100000);
    },

# 存放一些通用变量，如放在vuex getter里的值

    import { mapGetters } from "vuex";
    computed: {
        //计算属性
        ...mapGetters(["loginSign"])
    },

# 页面加载过慢时增加等待

__this.$Dialog.ViewLoading();为显示一个遮罩层覆盖页面，出现加载条__

    let mixin_time;
    export default {
    destroyed() {
        clearTimeout(mixin_time);
    },
    created() {
        mixin_time = setTimeout(() => {
        console.log("开始出现加载条");
        this.$Dialog.ViewLoading();
        }, 1000);
    },
    mounted() {
        this.$Dialog.ViewLoading({
        state: "end"
        });
    },

# 页面加载完成前增加模糊

    export default {
    created() {
        document.getElementById("app").classList.add("viewFilter");
    },
    mounted() {
        document.getElementById("app").classList.remove("viewFilter");
    },

# 部分内容接口请求完成前模糊内容



__js__

    //array内容为ref名字的数字，如：
    //<div ref="answer1"></div>
    //<div ref="answer2"></div>
    //array=["answer1","answer2"]
    methods: {
        RequestInit(array) {
        try {
            for (let i of array) {
            if (this.$refs[i]._isVue) this.$refs[i].$el.classList.add("filter10");
            else this.$refs[i].classList.add("filter10");
            }
        } catch (e) {
            console.log("混入函数出错：", e);
        }
        },
        RequestSuccess(array) {
        try {
            this.$nextTick(() => {
            for (let i of array) {
                if (this.$refs[i]._isVue)
                this.$refs[i].$el.classList.remove("filter10");
                else this.$refs[i].classList.remove("filter10");
            }
            });
        } catch (e) {
            console.log("混入函数出错：", e);
        }
        },
        RequestFail(array) {
        try {
            for (let i of array) {
            if (this.$refs[i]._isVue) {
                this.$refs[i].$el.classList.add("filterFail");
                this.$refs[i].$el.addEventListener("click", () =>
                this.failClick(this.$refs[i].$el, i)
                );
            } else {
                this.$refs[i].classList.add("filterFail");
                this.$refs[i].addEventListener("click", () =>
                this.failClick(this.$refs[i], i)
                );
            }
            }
        } catch (e) {
            console.log("混入函数出错：", e);
        }
        },
        failClick(el, name) {
        if (el.classList.value.indexOf("filterFail") < 0) return;
        el.classList.remove("filterFail");
        el.removeEventListener("click", () => this.failClick(el, name));
        // 重新请求函数
        try {
            eval("this." + name + "()");
        } catch (e) {
            console.log("重复请求失败", e);
        }
        }
    }

__scss__

    .filter10{
    position: relative;
    *{
        filter: blur(10px);
        -webkit-filter: blur(10px);
        pointer-events: none;
    }
    &::after{
        content: "";
        position: absolute;
        left: 20%;
        top: 40%;
        width: 30%;
        height: 2px;
        border-radius: 1px;
        background-color: #e83030;
        animation: move 1s linear 0s infinite normal;
    }
    @keyframes move {
        0%{
        left: 20%;
        width: 0;
        }
        25%{
        left: 20%;
        width: 30%;
        }
        50%{
        left: 50%;
        width: 30%;
        }
        75%{
        left: 80%;
        width: 0%;
        }
        100%{
        left: 80%;
        width: 0%;
        }
    }
    &.filterFail::after{
        font-size: 0.3rem;
        line-height: 0.6rem;
        color: #FFFFFF;
        content: "点击重试";
        position: absolute;
        left: 30%;
        top: 40%;
        height: 0.6rem;
        width: 40%;
        border-radius: 0.3rem;
        box-shadow: 0 5px 20px #000000;
        animation:none;
    }
    }
