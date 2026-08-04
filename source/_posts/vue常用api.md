---
title: vue常用api
date: 2020-11-24 10:07:27
tags: 
    - vue
categories: 
    - 前端
    - vue
---

# 全局配置
__可卸载main.js中__

## silent
__控制使用__
- 类型：boolean

- 默认值：false

- 用法：


        Vue.config.silent = true

取消 Vue 所有的日志与警告。

## keyCodes
__控制按键绑定对应功能__
- 类型：{ [key: string]: number | Array<number> }

- 默认值：{}

- 用法：


        Vue.config.keyCodes = {
        v: 86,
        f1: 112,
        // camelCase 不可用
        mediaPlayPause: 179,
        // 取而代之的是 kebab-case 且用双引号括起来
        "media-play-pause": 179,
        up: [38, 87]
        }
        <input type="text" @keyup.media-play-pause="method">

给 v-on 自定义键位别名。


## performance
__查看性能__
- 2.2.0 新增

- 类型：boolean

- 默认值：false (自 2.2.3 起)

- 用法：

设置为 true 以在浏览器开发工具的性能/时间线面板中启用对组件初始化、编译、渲染和打补丁的性能追踪。只适用于开发模式和支持 performance.mark API 的浏览器上。


## productionTip

__该项配置可去除开发时当前路由跳转提示__
- 2.2.0 新增

- 类型：boolean

- 默认值：true

- 用法：

设置为 false 以阻止 vue 在启动时生成生产提示。

# api

## Vue.extend( options )
__可以理解为vue文件__
- 参数：

        {Object} options
- 用法：

使用基础 Vue 构造器，创建一个“子类”。参数是一个包含组件选项的对象。

data 选项是特例，需要注意 - 在 Vue.extend() 中它必须是函数

    <div id="mount-point"></div>
    // 创建构造器
    var Profile = Vue.extend({
    template: '<p>{{firstName}} {{lastName}} aka {{alias}}</p>',
    data: function () {
        return {
        firstName: 'Walter',
        lastName: 'White',
        alias: 'Heisenberg'
        }
    }
    })
    // 创建 Profile 实例，并挂载到一个元素上。
    new Profile().$mount('#mount-point')
结果如下：

    <p>Walter White aka Heisenberg</p>



## Vue.nextTick( [callback, context] )
__可用于等待页面渲染完成__
- 参数：

        {Function} [callback]
        {Object} [context]

- 用法：

在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

    // 修改数据
    vm.msg = 'Hello'
    // DOM 还没有更新
    Vue.nextTick(function () {
    // DOM 更新了
    })

    // 作为一个 Promise 使用 (2.1.0 起新增，详见接下来的提示)
    Vue.nextTick()
    .then(function () {
        // DOM 更新了
    })
2.1.0 起新增：如果没有提供回调且在支持 Promise 的环境中，则返回一个 Promise。请注意 Vue 不自带 Promise 的 polyfill，所以如果你的目标浏览器不原生支持 Promise (IE：你们都看我干嘛)，你得自己提供 polyfill。


## Vue.set( target, propertyName/index, value )

__可触发视图更新__
- 参数：

    {Object | Array} target
    {string | number} propertyName/index
    {any} value
返回值：设置的值。

- 用法：

向响应式对象中添加一个 property，并确保这个新 property 同样是响应式的，且触发视图更新。它必须用于向响应式对象上添加新 property，因为 Vue 无法探测普通的新增 property (比如 this.myObject.newProperty = 'hi')

注意对象不能是 Vue 实例，或者 Vue 实例的根数据对象。

## Vue.delete( target, propertyName/index )
__可触发视图更新__
- 参数：

    {Object | Array} target
    {string | number} propertyName/index
仅在 2.2.0+ 版本中支持 Array + index 用法。

- 用法：

删除对象的 property。如果对象是响应式的，确保删除能触发更新视图。这个方法主要用于避开 Vue 不能检测到 property 被删除的限制，但是你应该很少会使用它。

在 2.2.0+ 中同样支持在数组上工作。

目标对象不能是一个 Vue 实例或 Vue 实例的根数据对象。

# 生命周期钩子

>所有的生命周期钩子自动绑定 this 上下文到实例中，因此你可以访问数据，对 |property 和方法进行运算。这意味着你不能使用箭头函数来定义一个生命周期方法 (例如 created: () => this.fetchTodos())。这是因为箭头函数绑定了父上下文，因此 this 与你期待的 Vue 实例不同，this.fetchTodos 的行为未定义。

## 生命周期图

![生命周期图示](/img/vue生命周期图示.png)

## beforeCreate

- 类型：Function

- 详细：

在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用。

## created

- 类型：Function

- 详细：

在实例创建完成后被立即调用。在这一步，实例已完成以下的配置：数据观测 (data observer)，property 和方法的运算，watch/event 事件回调。然而，挂载阶段还没开始，$el property 目前尚不可用。

## beforeMount

- 类型：Function

- 详细：

在挂载开始之前被调用：相关的 render 函数首次被调用。

__该钩子在服务器端渲染期间不被调用。__


## mounted

- 类型：Function

- 详细：

实例被挂载后调用，这时 el 被新创建的 vm.$el 替换了。如果根实例挂载到了一个文档内的元素上，当 mounted 被调用时 vm.$el 也在文档内。

__注意 mounted 不会保证所有的子组件也都一起被挂载。如果你希望等到整个视图都渲染完毕，可以在 mounted 内部使用 vm.$nextTick：__

    mounted: function () {
    this.$nextTick(function () {
        // Code that will run only after the
        // entire view has been rendered
    })
    }

该钩子在服务器端渲染期间不被调用。

## beforeUpdate

- 类型：Function

- 详细：

数据更新时调用，发生在虚拟 DOM 打补丁之前。这里适合在更新之前访问现有的 DOM，比如手动移除已添加的事件监听器。

__该钩子在服务器端渲染期间不被调用，因为只有初次渲染会在服务端进行。__

## updated

- 类型：Function

- 详细：

由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。

当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态。如果要相应状态改变，通常最好使用计算属性或 watcher 取而代之。

注意 updated 不会保证所有的子组件也都一起被重绘。如果你希望等到整个视图都重绘完毕，可以在 updated 里使用 vm.$nextTick：

    updated: function () {
    this.$nextTick(function () {
        // Code that will run only after the
        // entire view has been re-rendered
    })
    }

该钩子在服务器端渲染期间不被调用。

## activated

- 类型：Function

- 详细：

被 keep-alive 缓存的组件激活时调用。

该钩子在服务器端渲染期间不被调用。

- 参考：

[构建组件 - keep-alive][1]
[动态组件 - keep-alive][2]


## deactivated

- 类型：Function

- 详细：

被 keep-alive 缓存的组件停用时调用。

该钩子在服务器端渲染期间不被调用。

- 参考：

[构建组件 - keep-alive][1]
[动态组件 - keep-alive][2]



[1]:https://cn.vuejs.org/v2/api/#keep-alive
[2]:https://cn.vuejs.org/v2/guide/components-dynamic-async.html#%E5%9C%A8%E5%8A%A8%E6%80%81%E7%BB%84%E4%BB%B6%E4%B8%8A%E4%BD%BF%E7%94%A8-keep-alive

## beforeDestroy

- 类型：Function

- 详细：

实例销毁之前调用。在这一步，实例仍然完全可用。

该钩子在服务器端渲染期间不被调用。

## destroyed

- 类型：Function

- 详细：

实例销毁后调用。该钩子被调用后，对应 Vue 实例的所有指令都被解绑，所有的事件监听器被移除，所有的子实例也都被销毁。

该钩子在服务器端渲染期间不被调用。

## errorCaptured

>2.5.0+ 新增

- 类型：(err: Error, vm: Component, info: string) => ?boolean

- 详细：

当捕获一个来自子孙组件的错误时被调用。此钩子会收到三个参数：错误对象、发生错误的组件实例以及一个包含错误来源信息的字符串。此钩子可以返回 false 以阻止该错误继续向上传播。

>你可以在此钩子中修改组件的状态。因此在捕获错误时，在模板或渲染函数中有一个条件判断来绕过其它内容就很重要；不然该组件可能会进入一个无限的渲染循环。

- 错误传播规则

+ 默认情况下，如果全局的 config.errorHandler 被定义，所有的错误仍会发送它，因此这些错误仍然会向单一的分析服务的地方进行汇报。

+ 如果一个组件的继承或父级从属链路中存在多个 errorCaptured 钩子，则它们将会被相同的错误逐个唤起。

+ 如果此 errorCaptured 钩子自身抛出了一个错误，则这个新错误和原本被捕获的错误都会发送给全局的 config.errorHandler。

+ 一个 errorCaptured 钩子能够返回 false 以阻止错误继续向上传播。本质上是说“这个错误已经被搞定了且应该被忽略”。它会阻止其它任何会被这个错误唤起的 errorCaptured 钩子和全局的 config.errorHandler。