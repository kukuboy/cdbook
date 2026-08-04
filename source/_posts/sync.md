---
title: .sync
date: 2020-07-28 15:03:17
tags:
    - vue
categories: 
    - 前端
    - vue
---

# .sync修饰符
>一个组件上只能定义一个v-model，如果其他prop也要实现双向绑定的效果该怎么办呢？ 简单的方法是子组件向父组件发送一个事件，父组件监听该事件，然后更新prop。具体如下：
// info.vue组件定义了一个value 属性， 和一个valueChanged事件

    <template>
        <div>
            <input @input="onInput" :value="value"/>
        </div>
    </template>
    
    <script>
    export default {
        props: {
            value: {
                type: String
            }
        },
        methods: {
            onInput(e) {
                this.$emit("valueChanged", e.target.value)
            }
        }
    }
    </script>
    父组件index.vue
    <template>
        <info :value="myValue" @valueChanged="e => myValue = e"></info>
    </template>
    
    <script>
        inport info from './info.vue';
        export default {
            components: {
                info,
            },
            data() {
                return {
                    myValue: 1234,
                }
            },
        }
    </script>
> 上述写法太麻烦了，通过.sync可以简化上面代码，只需要修两个地方：

1. 组件内触发的事件名称以“update:myPropName”命名，相应的上述info组件改为 update:value
2. 父组件v-bind:value 加上.sync修饰符，即 v-bind:value.sync

>这样父组件就不用再手动绑定@update:value事件了。

## 用法1: v-bind:prop.sync="propvalue"

    // info.vue组件
    ...
    methods: {
        onInput(e) {
            this.$emit("update:value", e.target.value)
        }
    }
    // index.vue组件
    <info :value.sync="myValue"></info>
## 用法2 v-bind.sync="obj"

>如果一个组件的多个prop都要实现双向绑定，根据上面学到的知识，只需要每个prop加sync修饰符

    <info :a.sync="value1" :b.sync="value2" :c.sync="value2" :d.sync="value2"></info>

>这样写太麻烦，vue提供了一种更简便的方法， v-bind.sync = "对象"

    <info v-bind.sync="obj"></info>
    ...
    <script>
    ..
    data() {
        obj: {a: '', b: '', c: '', d: ''}
    }
    ..
    </script>

>注意：
带有.sync修饰符的v-bind不能喝表达式一起使用（例如 v-bind:title.sync = "doc.title + '!'"是无效的）。取而代之的是，你只能你想要绑定的属性名。
小结
一个组件需要提供多个双向绑定的属性时使用，只能选用一个属性来提供 v-model 功能，但如果有其他属性也要提供双向绑定，就需要.sync
