---
title: markdown
date: 2020-07-22 14:08:20
tags: markdown
categories: markdown
---

# 标题

    #一级标题
    ##二级标题
    ###三级标题
## 二级标题
### 三级标题

# 代码

---
    代码 前用一个tab或者四个空格

    this is code 
    before is a tab or four space
    少部分区块
    `代码`
    可以指定一部分预言

    ``` javascript
    代码
    ```



---
# 分割线

    ---
    ***

---
** *

# 字体格式

        *斜体文本*  
        _斜体文本_  
        **粗体文本**  
        __粗体文本__  
        ***粗斜体文本***  
        ___粗斜体文本___  

效果

*斜体文本*  
_斜体文本_  
**粗体文本**  
__粗体文本__  
***粗斜体文本***  
___粗斜体文本___  
    
---
# 无序列表

    - 无序列表
    - 无序列表

- 无序列表
- 无序列表
    ---
# 有序列表

    1. 有序列表
    2. 有序列表

1. 有序列表
2. 有序列表
 
 ---
# 嵌套列表

    - 嵌套列表
     + 嵌套列表
     + 嵌套列表
      - 嵌套列表
       * 嵌套列表
    - 嵌套列表

- 嵌套列表
 + 嵌套列表
 + 嵌套列表
  - 嵌套列表
   * 嵌套列表
- 嵌套列表

---
# 文字超链

    Tooltips可省略
    [不如](http://bruce-sha.github.io "不如的博客")
    [不如][1]
    
    [1]:http://bruce-sha.github.io

    直接使用链接地址
    <http://blog.dingyahui.top>
 
 [不如](http://bruce-sha.github.io "不如的博客")   

 [不如][1]

 [不如][2]
 
 [1]:http://bruce-sha.github.io
 [2]:../_assets/images/vue生命周期图示.png

 <http://blog.dingyahui.top>

 
 
---
# 图片超链

__前面加入!即可显示图片内容__
 
     多个感叹号，Tooltips可省略，要设置大小只能借助HTML标记
     ![GitHub Mark](http://github.global.ssl.fastly.net/images/modules/logos_page/GitHub-Mark.png "GitHub Mark")
    
    ![alt 属性文本](图片地址 "可选标题")

    可以直接使用img标签，指定宽度
    <img src = "http://github.global.ssl.fastly.net/images/modules/logos_page/GitHub-Mark.png"/>

  
![GitHub Mark](http://github.global.ssl.fastly.net/images/modules/logos_page/GitHub-Mark.png "GitHub Mark") 

---
# 转义字符

    Markdown中的转义字符为\，转义的有：
    \\ 反斜杠
    \` 反引号
    \* 星号
    \_ 下划线
    \{\} 大括号
    \[\] 中括号
    \(\) 小括号
    \# 井号
    \+ 加号
    \- 减号
    \. 英文句号
    \! 感叹号
    
---
# 注释

    aaaa
    <!--注释内容 -->


aaaa
<!-- 注释内容 -->


# 区块

    >区块内容
    >>区块内容
    >>>区块内容


>区块内容
>>区块内容
>>>区块内容

# 表格


        | 表头 | 表头 |
        | ---- | ---- |
        | 内容 | 内容 |
        | 内容 | 内容 |


| 表头 | 表头 |
| ---- | ---- |
| 内容 | 内容 |
| 内容 | 内容 |

# 删除线
    ~~sdadadada~~

~~sdasdadssa~~

# 下划线

    <u>这是个带脚注的文本</u>

<u>这是个带脚注的文本</u>


# 脚注

    [^这是个带脚注的文本]
    [^这是个带脚注的文本]:这个是它的脚注


[^这是个带脚注的文本]

[^这是个带脚注的文本]:这个是它的脚注


# 高级技巧

    不在 Markdown 涵盖范围之内的标签，都可以直接在文档里面用 HTML 撰写。

    目前支持的 HTML 元素有：<kbd> <b> <i> <em> <sup> <sub> <br>等 ，如：

    使用 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Del</kbd> 重启电脑


使用 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Del</kbd> 重启电脑
