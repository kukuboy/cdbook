---
title: scss函数
date: 2020-07-23 10:10:14
tags: scss
categories: 
    - 前端
    - css
    - scss
---

# Scss函数

## abs($number) 绝对值

>说明：取变量的绝对值，可以包含单位。
>返回类型：number

    @debug abs(-10);   // 10
    @debug abs(-10in); // 10in
    @debug abs(-10px); // 10px
    
## ceil($number) 向高位取整

>说明：把数值中的小数舍去，取下一个整数。返回类型：number

    @debug ceil(4); // 4
    @debug ceil(4.2); // 5
    @debug ceil(4.9); // 5

## comparable($number,$number2) 判断单位

>说明：判断$number和$number2是否有同类型的单位，例如px和px，cm和mm等。如果返回true，这两个变量可以相加、相减、比较，否则对$number和$number2进行上述运算时会报错。
返回类型：boolean

    @debug comparable(2px, 1px); // true
    @debug comparable(100px, 3em); // false
    @debug comparable(10cm, 3mm); // true

## floor($number) 向低位取整

>说明：把数值中的小数舍去。
>返回类型：number

    @debug floor(4); // 4
    @debug floor(4.2); // 4
    @debug floor(4.9); // 4

## max($number...) 取最大值

>说明：参数中最大的一个数。如果参数是数组，则需要在后面添加...才能执行，不然会报错
>返回类型：number

    @debug max(1px, 4px); // 4px
    $widths: 50px, 30px, 100px;
    @debug max($widths...); // 100px

## min($number...) 取最小值

>说明：参数中最小的一个数。如果参数是数组，则需要在后面添加...才能执行，不然会报错
>返回类型：number

    @debug min(1px, 4px); // 1px
    $widths: 50px, 30px, 100px;
    @debug min($widths...); // 30px


## percentage($number) 转换为百分比

>说明：把没有单位的值转换为百分比，通常是0到1的浮点数，带单位会报错。此函数和$number * 100%效果是一样的。
>返回类型：number

    @debug percentage(0.2); // 20%
    $number : 0.2;
    @debug $number * 100%;  // 20%
    @debug percentage(100px / 50px); // 200%  100px和50px带单位，但是除法运算后是2，不带单位


## random($limit:null)  生成随机数

>说明：生成随机数。参数$limit默认为null，为随机数最大值。
>返回类型：number
>如果$limit为null时，结果介于0与1之间。

    @debug random(); // 0.2821251858
    @debug random(); // 0.6221325814

>如果$limit不为空时，结果介于1与$limit之间，参数必须大于或等于1，而且为整数，否则报错。
   
    @debug random(10); // 4
    @debug random(10000); // 5373


## round($number) 四舍五入

>说明：四舍五入（0.5进位操作）
>返回类型：number

    @debug round(4); // 4
    @debug round(4.2); // 4
    @debug round(4.9); // 5


## unit($number) 取单位

>说明：获取参数的单位，字符串形式返回
>返回类型：quoted string
>注意：这个方法是用于调试的，不保证输出结果和发布的一致

    @debug unit(100); // ""
    @debug unit(100px); // "px"
    @debug unit(5px * 10px); // "px*px"
    @debug unit(5px / 1s); // "px/s"


## unitless($number)

>说明：判断$number是否没有单位
>返回类型：boolean

    @debug unitless(100); // true
