---
title: vue项目首页加载画面
date: 2020-12-01 13:53:56
tags: 
    - 首页加载过慢
    - vue
categories: 
    - 前端
    - vue
    - vue-cli项目
---

# 介绍

__适用于vue-cli创建的项目__

__原理为页面加载后内容被替代，加载之前变现实该内容__

# 代码

    <!DOCTYPE html>
    <html lang="en">

    <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no,viewport-fit=cover">
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <title><%= htmlWebpackPlugin.options.title %></title>
    </head>

    <body>
    <noscript>
        <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled.
        Please enable it to continue.</strong>
    </noscript>
    <div id="app">
        <!-- built files will be auto injected -->
        <div id="rotate">
        <div class="ball"></div>
        <div class="line lineOne"></div>
        <div class="line lineTwo"></div>
        <div class="line lineThree"></div>
        <div class="line lineFour"></div>
        <div class="line lineFive"></div>
        </div>
        <div class="iconApp" style="display: none;" onclick="back()">
        <p class="iconfont iconP">&#xe87c;</p>
        <p class="iconP">加载失败</p>
        </div>
        <script>
        function back() {
            //可编写返回或者弹窗内容
        }
        </script>
        <style>
        #app {
            position: absolute;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
        }

        #rotate {
            width: 100%;
            margin-top: 3vh;
            position: relative;
            opacity: 0;
            animation: opactiyChange 1s linear 1s 1 forwards normal;
        }

        @keyframes opactiyChange {
            from {
            opacity: 0;
            }

            to {
            opacity: 1;
            }
        }

        #rotate .line {
            width: 12vw;
            height: 0.3vw;
            background-color: #000000;
            top: 8vw;
            position: absolute;
            left: 44vw;
        }

        #rotate .ball {
            position: absolute;
            margin-left: 47.5vw;
            width: 5vw;
            height: 5vw;
            border-radius: 2.5vw;
            top: 5.5vw;
            z-index: 2;
            background-color: #ffffff;
        }

        #rotate .lineOne {
            animation: rotateOne 3s linear 0s infinite normal;
        }

        #rotate .lineTwo {
            animation: rotateTwo 3s linear 0s infinite normal;
        }

        #rotate .lineThree {
            animation: rotateThree 3s linear 0s infinite normal;
        }

        #rotate .lineFour {
            animation: rotateFour 3s linear 0s infinite normal;
        }

        #rotate .lineFive {
            animation: rotateFive 3s linear 0s infinite normal;
        }

        @keyframes rotateOne {
            0% {
            transform: rotate(0deg);
            }

            20% {
            transform: rotate(36deg);
            }

            40% {
            transform: rotate(72deg);
            }

            60% {
            transform: rotate(108deg);
            }

            80% {
            transform: rotate(144deg);
            }

            100% {
            transform: rotate(180deg);
            }
        }

        @keyframes rotateTwo {
            0% {
            transform: rotate(36deg);
            }

            20% {
            transform: rotate(72deg);
            }

            40% {
            transform: rotate(108deg);
            }

            60% {
            transform: rotate(144deg);
            }

            80% {
            transform: rotate(180deg);
            }

            100% {
            transform: rotate(216deg);
            }
        }

        @keyframes rotateThree {
            0% {
            transform: rotate(72deg);
            }

            20% {
            transform: rotate(108deg);
            }

            40% {
            transform: rotate(144deg);
            }

            60% {
            transform: rotate(180deg);
            }

            80% {
            transform: rotate(216deg);
            }

            100% {
            transform: rotate(252deg);
            }
        }

        @keyframes rotateFour {
            0% {
            transform: rotate(108deg);
            }

            20% {
            transform: rotate(144deg);
            }

            40% {
            transform: rotate(180deg);
            }

            60% {
            transform: rotate(216deg);
            }

            80% {
            transform: rotate(252deg);
            }

            100% {
            transform: rotate(288deg);
            }
        }

        @keyframes rotateFive {
            0% {
            transform: rotate(144deg);
            }

            20% {
            transform: rotate(180deg);
            }

            40% {
            transform: rotate(216deg);
            }

            60% {
            transform: rotate(252deg);
            }

            80% {
            transform: rotate(288deg);
            }

            100% {
            transform: rotate(324deg);
            }
        }
        </style>
    </div>
    </body>
    </html>
    <style>
    #rotate {
        display: none;
    }

    .iconApp {
        display: block !important;
        text-align: center;
        margin: 0;
    }

    .iconApp .iconP {
        margin: 0;
    }

    @font-face {
        font-family: 'iconfont';
        src: url('iconfont.eot?t=1605171141748');
        /* IE9 */
        src: url('iconfont.eot?t=1605171141748#iefix') format('embedded-opentype'),
        /* IE6-IE8 */
        url('data:application/x-font-woff2;charset=utf-8;base64,d09GMgABAAAAAALsAAsAAAAABuQAAAKdAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAHEIGVgCCcAqBYIFPATYCJAMICwYABCAFhG0HPhsDBhHVk2tkXyTYjtFbgDIrYZ6GfIV2WYUzwfP8Or5zH8YfRMJSzJLiRkHZcfM0QAAd8BxjBQ+Azc3QNSpCsSTneT99RzompPnpE/dO/wookPmAclx706QuwDiggPbGKKBAC6SQG8YueIG3IUBAoXrStXvfoXgY7EUCyNxZMybhFUIYDMt6CE7DToPswOKpjeoZsD36vPyhIQ8UVmPXDZzebSodfy5x8vSKeMXYgEC83H0AFtBAPTAgQxtT/dEIY2uClKEUx6oW+LkkjgMLclQjwv46uzICIB+KnknsSR71lwoAA1igKzB40muA9VOTk/0Z52+uvLHp2orr0z5sGrrnwMGzWR83D9sbns/af2hC4r13xebk+25JDz+W2VMfxh24Vp547kauPXyzKuns9ZyhZ4Ri89hUh5snGps7q57ohV3jBV3Pjbw4/ezkNruruGwfM7JrrxUDe/a0Ha2vG5jM8W8EjSdWnGgMOkbWmx3mGEFHAOJrRyN/9TtMqezFhKS2/73QAPD1bfZ9PCe+QQvgndst8A/FA7uMIbXNymjixpyiK/beGgICIIZ9qX5M1WXcsEPwCmIGhUsBNB7lpGHVwxLSBAePDgio02N/SJrFaMT4QK21AEKyC1AkchOaZM9Jw/oOS6Z/cEhOI2C4pJ0YUhk6S3QhlIwG1A+CbtW6VhZx9RXdK0vKSyPOE6mHKCzjXM5esCItsaa/3cpswVIrsIP7MOcGB7WImkfPfGzTZJueNOpWBkEXQslogPqBoFu1QX8Wlfdf0b2ypI6OuvATqYfhwWI0DyAvqjqo41Ee6W+3YrbAUitgBw7MTqqBo3laRM0jPyFxbCbUyw5Vjdtryt8dAAJsvRJnhCZ9ryPIGr38vwzdBxUE53UYAAAAAA==') format('woff2'),
        url('iconfont.woff?t=1605171141748') format('woff'),
        url('iconfont.ttf?t=1605171141748') format('truetype'),
        /* chrome, firefox, opera, Safari, Android, iOS 4.2+ */
        url('iconfont.svg?t=1605171141748#iconfont') format('svg');
        /* iOS 4.1- */
    }

    .iconApp .iconfont {
        margin-top: 20vh;
        font-family: 'iconfont' !important;
        font-size: 20vw;
        font-style: normal;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
    }
    </style>