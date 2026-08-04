---
title: js堆和栈-基本类型和引用类型
date: 2021-04-02 10:25:35
tags: 
    - 堆
    - 栈
    - 基本类型
    - 引用类型
    - 深拷贝
categories:
    - 前端
---

>>堆和栈其实是内存存储的方式，在js中基本类型是放在栈内存中的，引用类型是放在堆内存中的,引用类型的引用还是放在栈内存中的

___因为引用类型指向的是栈中的指针地址，指针地址在指向堆中的内容，故由此存在了浅复制的问题，故在最后还会讲下深度克隆的方法___

# 基本类型

## 数字（Number）

1. 包括浮点数和整数

    什么是浮点数：浮点数就是该数值中必须有一个小数点，并且小数点后面必须至少有一位数字。
    由于保存浮点数值的内存空间是保存整数值的两倍，所以js会不失时机的把浮点数转换为整数。小数点后面没有数值或者1.0就会被保存成整数。

2. 特殊的数字类型NaN

    1、表示不是数字，但是其实它是一个特殊的数字（NaN：Not a Number）

    2、当运算操作符错误的时候，一般会得到NaN

    3、NaN具有传染性，即NaN参与任何运算，结果都为NaN

    4、NaN与任何数值都不相等

+ isNaN()函数

    isNaN()函数---判断一个数是否为NaN的唯一方式

    判断一个数是否是NaN，如果是NaN则返回true，否则返回false

3. Infinity

   1、Infinity：数据超过了JS可以表示的范围，是一个特殊的数字

   2、Infinity与其他数据类型进行操作运算，结果还是Infinity

   3、数字除以0得到Infinity


+ isFinite()函数

    如果是NaN或者Infinity返回false，否则返回true

## 字符串（String）

多个字符的有序序列，双引号和单引号引起来的都是字符串

+ 字符串特点：

- 单引号和双引号引起来的都是字符串

- 两个字符串相加，实际上是将两个字符串拼接

- 两个字符串相加，是拼接而不是运算，其他运算结果为NaN，表示计算错误

- 字符串加数字，首先将数字转化为响应的字符串然后再参与运算。同样不支持-, *, /等其他运算


## 布尔值（Boolean）
包括true和false，通常用在流程控制语句，选择判断语句

+ false值：

- 数字0
- NaN
- “ ”，空字符串
- false
- undefined
- null

+ true值： 除了false值就是true值

## Undefined
如果使用一个未定义的变量，会得到一个undefined值，当定义了一个变量未初始化，默认的也会给它初始化成undefined值

## null
代表什么也没有

注意：null和undefined的最大区别是如果变量值为null，说明变量是存在的，只不过它的值是空值null

## Symbol(es6)

### 概述
>主要用于变量名,货号内的内容是描述

>ES5 的对象属性名都是字符串，这容易造成属性名的冲突。比如，你使用了一个他人提供的对象，但又想为这个对象添加新的方法（mixin 模式），新方法的名字就有可能与现有方法产生冲突。如果有一种机制，保证每个属性的名字都是独一无二的就好了，这样就从根本上防止属性名的冲突。这就是 ES6 引入Symbol的原因。

>ES6 引入了一种新的原始数据类型Symbol，表示独一无二的值。它是 JavaScript 语言的第七种数据类型，前六种是：undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。

>Symbol 值通过Symbol函数生成。这就是说，对象的属性名现在可以有两种类型，一种是原来就有的字符串，另一种就是新增的 Symbol 类型。凡是属性名属于 Symbol 类型，就都是独一无二的，可以保证不会与其他属性名产生冲突。

        let s = Symbol();

        typeof s
        // "symbol"

>上面代码中，变量s就是一个独一无二的值。typeof运算符的结果，表明变量s是 Symbol 数据类型，而不是字符串之类的其他类型。

>注意，Symbol函数前不能使用new命令，否则会报错。这是因为生成的 Symbol 是一个原始类型的值，不是对象。也就是说，由于 Symbol 值不是对象，所以不能添加属性。基本上，它是一种类似于字符串的数据类型。

>Symbol函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。

    let s1 = Symbol('foo');
    let s2 = Symbol('bar');

    s1 // Symbol(foo)
    s2 // Symbol(bar)

    s1.toString() // "Symbol(foo)"
    s2.toString() // "Symbol(bar)"

>上面代码中，s1和s2是两个 Symbol 值。如果不加参数，它们在控制台的输出都是Symbol()，不利于区分。有了参数以后，就等于为它们加上了描述，输出的时候就能够分清，到底是哪一个值。

>如果 Symbol 的参数是一个对象，就会调用该对象的toString方法，将其转为字符串，然后才生成一个 Symbol 值。

    const obj = {
    toString() {
        return 'abc';
    }
    };
    const sym = Symbol(obj);
    sym // Symbol(abc)
    注意，Symbol函数的参数只是表示对当前 Symbol 值的描述，因此相同参数的Symbol函数的返回值是不相等的。

    // 没有参数的情况
    let s1 = Symbol();
    let s2 = Symbol();

    s1 === s2 // false

    // 有参数的情况
    let s1 = Symbol('foo');
    let s2 = Symbol('foo');

    s1 === s2 // false
    上面代码中，s1和s2都是Symbol函数的返回值，而且参数相同，但是它们是不相等的。

>Symbol 值不能与其他类型的值进行运算，会报错。

    let sym = Symbol('My symbol');

    "your symbol is " + sym
    // TypeError: can't convert symbol to string
    `your symbol is ${sym}`
    // TypeError: can't convert symbol to string
>但是，Symbol 值可以显式转为字符串。

    let sym = Symbol('My symbol');

    String(sym) // 'Symbol(My symbol)'
    sym.toString() // 'Symbol(My symbol)'
>另外，Symbol 值也可以转为布尔值，但是不能转为数值。

    let sym = Symbol();
    Boolean(sym) // true
    !sym  // false

    if (sym) {
        // ...
    }

    Number(sym) // TypeError
    sym + 2 // TypeError
    Symbol.prototype.description
>创建 Symbol 的时候，可以添加一个描述。

    const sym = Symbol('foo');

>上面代码中，sym的描述就是字符串foo。

>但是，读取这个描述需要将 Symbol 显式转为字符串，即下面的写法。

    const sym = Symbol('foo');

    String(sym) // "Symbol(foo)"
    sym.toString() // "Symbol(foo)"
>上面的用法不是很方便。ES2019 提供了一个实例属性description，直接返回 Symbol 的描述。

    const sym = Symbol('foo');

    sym.description // "foo"
>作为属性名的 Symbol
>由于每一个 Symbol 值都是不相等的，这意味着 Symbol 值可以作为标识符，用于对象的属性名，就能保证不会出现同名的属性。这对于一个对象由多个模块构成的情况非常有用，能防止某一个键被不小心改写或覆盖。

    let mySymbol = Symbol();

    // 第一种写法
    let a = {};
    a[mySymbol] = 'Hello!';

    // 第二种写法
    let a = {
        [mySymbol]: 'Hello!'
    };

    // 第三种写法
    let a = {};
    Object.defineProperty(a, mySymbol, { value: 'Hello!' });

    // 以上写法都得到同样结果
    a[mySymbol] // "Hello!"
    上面代码通过方括号结构和Object.defineProperty，将对象的属性名指定为一个 Symbol 值。

>注意，Symbol 值作为对象属性名时，不能用点运算符。

    const mySymbol = Symbol();
    const a = {};

    a.mySymbol = 'Hello!';
    a[mySymbol] // undefined
    a['mySymbol'] // "Hello!"
>上面代码中，因为点运算符后面总是字符串，所以不会读取mySymbol作为标识名所指代的那个值，导致a的属性名实际上是一个字符串，而不是一个 Symbol 值。

>同理，在对象的内部，使用 Symbol 值定义属性时，Symbol 值必须放在方括号之中。

    let s = Symbol();

    let obj = {
        [s]: function (arg) { ... }
    };

    obj[s](123);
    上面代码中，如果s不放在方括号中，该属5性的键名就是字符串s，而不是s所代表的那个 Symbol 值。

>采用增强的对象写法，上面代码的obj对象可以写得更简洁一些。

    let obj = {
    [s](arg) { ... }
    };
>Symbol 类型还可以用于定义一组常量，保证这组常量的值都是不相等的。

    const log = {};

    log.levels = {
    DEBUG: Symbol('debug'),
    INFO: Symbol('info'),
    WARN: Symbol('warn')
    };
    console.log(log.levels.DEBUG, 'debug message');
    console.log(log.levels.INFO, 'info message');
>下面是另外一个例子。

    const COLOR_RED    = Symbol();
    const COLOR_GREEN  = Symbol();

    function getComplement(color) {
    switch (color) {
        case COLOR_RED:
        return COLOR_GREEN;
        case COLOR_GREEN:
        return COLOR_RED;
        default:
        throw new Error('Undefined color');
        }
    }
>常量使用 Symbol 值最大的好处，就是其他任何值都不可能有相同的值了，因此可以保证上面的switch语句会按设计的方式工作。

>还有一点需要注意，Symbol 值作为属性名时，该属性还是公开属性，不是私有属性。

[查看更多内容点此处]("https://wangdoc.com/es6/symbol.html)

# 引用类型

## Object

>数组、函数、正则表达式都是对象。

# 栈

    学过数据结构的都知道 栈 是一种 先进后出 的数据结构，栈内存是内存中用于存放临时变量的一片内存块。当声明一个基本变量时，它就会被存储到栈内存中。比如有这样一段代码，他们在栈内存中存储的形式如下表一样:

    const a = 1;
    const b = "1";

| 变量名 | 变量值 |
| ------ | ------ |
| b      | “1”    |
| a      | 1      |

>而当其发生复制时，会把对应内存中的数据复制一份到新内存中，就像下面这样

    const c = b
| 变量名 | 变量值 |
| ------ | ------ |
| c      | “1”    |
| b      | “1”    |
| a      | 1      |

>很显然，c，b两个变量占用了不同的存储空间，所以他们之间也并没有什么联系。
>栈内存的地址分配是连续的，所以在后续也不能对其进行进一步的扩充或者删除。

__栈总结：__

>栈内存的特点:存取速度快，但不灵活，同时由于结构简单，在变量使用完成后就可以将其释放，内存回收容易实现。

# 堆
>堆内存的存储不同与栈，虽然他们都是内存中的一片空间，但是堆内存存储变量时没有什么规律可言。

    const p1 = {};
    const p2 = {};
    const p3 = {};



>我们在访问引用类型时，需要在栈内存中查找 对应的地址，在去堆内存中取得真正的值，访问速度自然不及栈内存。

>对引用类型进行复制

    const p4 = p3;
    占用不同的栈但指针指向相同的堆


>我们只是将地址复制了一遍，p4 和 p3 都是指向同一个地址，这就说明对 p4 进行修改时就会影响到 p3 的值。所以对引用类型进行复制时，应该把堆内存中的内容复制一遍，在将新地址赋值给新变量，这就涉及到深拷贝了, 那我就简单的实现下深拷贝：


__堆总结：__

>堆内存的特点：使用灵活，可以动态增加或删除空间，但是存取比较慢

# 深拷贝


    // 第一种
    const a = {};
    const b = JSON.parse(JSON.stringfy(a));

    // 第二种
    function clone(obj, hash = new WeakMap()) {
    // 判断是否为 null 或者 typeof 返回类型是否为 object
    if (obj == null || typeof obj !== "object") return obj;
    else if (obj instanceof Date) return new Date(obj);
    else if (obj instanceof RegExp) return new RegExp(obj);
    
    // 判断集合是否有这个属性，有则直接 return obj
    if(hash.has(obj)) return hash.get(obj)
    const newObj = new obj.constructor();
        
    // 将属性和拷贝后的值作为一个map
    hash.set(obj, newObj);
        
    // 遍历 obj
    for (let key in Object.getOwnPropertyDescriptors(obj)) {
        // 过滤掉原型身上的属性
        if (obj.hasOwnProperty(key)) {
            // 递归拷贝
            newObj[key] = clone(obj[key], hash);
        }
        
    }
    return newObj;
    }
