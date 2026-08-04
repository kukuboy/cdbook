---
title: vuex_state数据刷新丢失问题
date: 2020-09-04 09:00:41
tags: 
    - vue
    - vuex
    - vue-cli3
    - state刷新丢失
categories: 
    - 前端
    - vue
    - vuex
---

>在使用vuex存储数据进行调试的过程中，刷新会直接丢失数据，在这里直接敷上解决方法

>此项目由vue-cli3建立

# store,index.js文件


            /*
            * @Description:
            * @Author: 水印红枫
            * @Date: 2020-07-02 16:46:07
            * @LastEditors: 水印红枫
            */
            import Vue from "vue";
            import Vuex from "vuex";
            import { cmd } from "@/util/myUtil";

            Vue.use(Vuex);

            const store = new Vuex.Store({
            state: sessionStorage.getItem("state")
                ? JSON.parse(sessionStorage.getItem("state"))
                : {
                    currentPage: "/"
                },
            getters: {
                currentPage(state) {
                return state.currentPage;
                }
            },
            mutations: {
                setcurrentPage(state, val) {
                state.currentPage = val;
                }
            },
            actions: {
                setcurrentPage({ commit }, val) {
                commit("setcurrentPage", val);
                }
            }
            });
            export default store;


## app.vue 或者home.vue中


        
        destroyed() {
            window.removeEventListener("unload", this.saveState); // 解决刷新后vux内容丢失问题
        },
        mounted() {
            this.init();
        },
        methods: {
            init() {
            // 解决刷新后vux内容丢失问题
            window.addEventListener("unload", this.saveState);
            },
            saveState() {
            sessionStorage.setItem("state", JSON.stringify(this.$store.state));
            // 需要在刷新时进行操作可在这里
            // sessionStorage.setItem("state_page", 1);
            this.clearState();
            },
            clearState() {
            //刷新后需要清空的数据放在这里处理
            },
        }
