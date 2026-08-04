---
title: vue-router实现404
date: 2020-09-21 15:08:16
tags: 
    - vue
    - vue-router
    - vue-cli3
    - 404
categories: 
    - 前端
    - vue
    - vue-router
---

# 实现页面跳转到404页面



        const page404 = {
        path: "/*",
        name: "error_404",
        hidden: true,
        meta: {
            title: "404-页面不存在"
        },
        component: () => import("../views/error_page/404.vue")
        };
        const routes = [
        {
            path: "/",
            // name: "Home",
            component: () => import("../views/Home"),
        },
        page500,
        page404
        ];
        //添加其他页面请写在page404之前，否则会覆盖匹配


 
# 实现不存页面均跳转到首页



        const pageNotFound = {
        path: "/*",
        component: NotFoundComponent
        };
        const routes = [
        {
            path: "/",
            // name: "Home",
            component: () => import("../views/Home"),
        },
        pageNotFound
        ];
        //添加其他页面请写在pageNotFound之前，否则会覆盖匹配