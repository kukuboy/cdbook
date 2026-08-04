---
title: express
date: 2020-09-14 14:50:10
tags: express
categories:
    - 前端
    - express
---

# hello world


            mkdir helloExpress
            cd helloExpress
            // 可手动操作，创建文件夹并进入

            npm init 
            // 一直enter,除下项
            entry point: (index.js)：app.js
            // 建议此项改为app.js

            npm i express -S
            // 安装依赖

            接下来创建app.js文件
            文件内容如下

            const express = require('express')
            const app = express()
            const port = 3000

            app.get('/', (req, res) => res.send('Hello World!'))

            app.listen(port, () => console.log(`Example app listening on port ${port}!`))


            node app.js
            // 命令行运行，打印出相关信息


# 项目建立

            mkdir myapp
            cd myapp
            // 可手动操作，创建文件夹并进入

            npx express-generator
            // 包含在 Node.js 8.2.0 及更高版本中


            npm install -g express-generator
            express --view=pug myapp
            // 老版本用此句生成

            cd myapp
            npm i

            官方文档

            在 MacOS 或 Linux 中，通过如下命令启动此应用：
            $ DEBUG=myapp:* npm start
            在 Windows 中，通过如下命令启动此应用：
            > set DEBUG=myapp:* & npm start

            若启动出错，可直接
            npm start

# 基本路由


            app.METHOD(PATH, HANDLER)
            // 基本方法

            app.get('/', function (req, res) {
            res.send('Hello World!')
            })

            app.post('/', function (req, res) {
            res.send('Got a POST request')
            })

            app.put('/user', function (req, res) {
            res.send('Got a PUT request at /user')
            })

            app.delete('/user', function (req, res) {
            res.send('Got a DELETE request at /user')
            })



# 利用express 静态托管页面
> 在上述生成器生成的项目中，已默认托管public文件夹


            express.static(root, [options])

            app.use(express.static('public'))
            // 默认

            app.use(express.static('public'))
            app.use(express.static('files'))
            // 多个

            app.use('/static', express.static('public'))
            // 带访问路径

            http://localhost:3000/static/images/kitten.jpg

            app.use('/static', express.static(path.join(__dirname, 'public')))




