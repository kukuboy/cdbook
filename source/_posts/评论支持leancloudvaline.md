---
title: 评论支持leancloudvaline
date: 2020-09-15 10:29:58
tags: 
    - leancloud
    - valine
    - 评论
categories: 
    - 博客
    - 评论
---


# 网站评论支持 leancloud valine

[官方文档](https://www.leancloud.cn/)


# 代码 Valine-Admin

>可直接在github上搜索，按照介绍使用即可，建议自己fork一个，使用自己
>的，方便自己更改主题

# 步骤

1. 首先在leancloud上注册创建应用Valine-Admin
2. 进入对应的应用，点击设置 -> 应用 Keys，获取AppID和AppKey
3. 以Hexo主题配置文件为例，填入对应的地方
            
            
            valine:
            enable: true
            appId: AppID
            appKey: AppKey
            notify: false
            verify: false
            visitor: true
            avatar: 'mp' # Gravatar style : mp/identicon/monsterid/wavatar/retro/hide
            pageSize: 10
            placeholder: 'just go go' # Comment Box placeholder


4. 评论存储
   >有些已默认配置好，以后可在此项查看评论，并删除评论

   + 查看评论
        点击 存储 -> 结构化数据，选择创建Class，名称Comment，其他保持默认，以后就可在此Class内查看
   + 文章阅读量统计
        点击 存储 -> 结构化数据，选择创建Class，名称Counter，其他保持默认，以后就可在此Class内查看

5. 云引擎

    - 选择云引擎->部署
    - 选择git部署，填入github地址，部署即可 （如https://github.com/kukuboy/Valine-Admin）
    - 云引擎->设置:：
    - 填写各变量以及内容，这里不再具体详解，valine-admin项目的rendme已经介绍，其他各博客也有
    - 云引擎->定时任务
    - 自动唤醒：名称自定义，选择生产环境，self_wake，x选择corn表达式 - 0 */20 7-23 * * ? 
    - 补发邮件：名称自定义，选择生产环境 - resend_mails，x选择corn表达式 - 0 0 8 * * ?

6. 设置

>这里需要自己由自己的域名，建议还是买一个域名，其实还是挺便宜的，一年也就几块钱，用处很多，因为这里国家要求，leancloud需要检查备案信息，所以还是花几块钱买个吧
    

   - 安全中心：在Web 安全域名中填入自己博客地址
   - 域名绑定：可自己定义一个二级域名，不要与博客域名相同，不要与博客域名相同，不要与博客域名相同，重要的事情说三遍，这个域名是用来评论管理的，按照介绍绑定以及解析即可



如有问题可评论留下你的问题，或者私发邮件给博主，博主是个小博主，还是会尽力回的


