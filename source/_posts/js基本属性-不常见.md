---
title: js基本属性-不常见
tags:
  - js
categories:
  - 前端
  - js
date: 2021-07-19 10:09:16
---
# Proxy

>Proxy 对象用于创建一个对象的代理，从而实现基本操作的拦截和自定义（如属性查找、赋值、枚举、函数调用等）。

## 术语

### handler

>包含捕捉器（trap）的占位符对象，可译为处理器对象。

### traps
> 提供属性访问的方法。这类似于操作系统中捕获器的概念。

### target

>被 Proxy 代理虚拟化的对象。它常被作为代理的存储后端。根据目标验证关于对象不可扩展性或不可配置属性的不变量（保持不变的语义）。

## 语法

````javascript
const p = new Proxy(target, handler)
````

## 参数

### target
>要使用 Proxy 包装的目标对象（可以是任何类型的对象，包括原生数组，函数，甚至另一个代理）。
### handler
>一个通常以函数作为属性的对象，各属性中的函数分别定义了在执行各种操作时代理 p 的行为。
## 方法
````javascript
    Proxy.revocable()
````
>创建一个可撤销的Proxy对象。
## handler 对象的方法
>handler 对象是一个容纳一批特定属性的占位符对象。它包含有 Proxy 的各个捕获器（trap）。

>所有的捕捉器是可选的。如果没有定义某个捕捉器，那么就会保留源对象的默认行为。
````javascript
handler.getPrototypeOf()
Object.getPrototypeOf 方法的捕捉器。
handler.setPrototypeOf()
Object.setPrototypeOf 方法的捕捉器。
handler.isExtensible()
Object.isExtensible 方法的捕捉器。
handler.preventExtensions()
Object.preventExtensions 方法的捕捉器。
handler.getOwnPropertyDescriptor()
Object.getOwnPropertyDescriptor 方法的捕捉器。
handler.defineProperty()
Object.defineProperty 方法的捕捉器。
handler.has()
in 操作符的捕捉器。
handler.get()
属性读取操作的捕捉器。
handler.set()
属性设置操作的捕捉器。
handler.deleteProperty()
delete 操作符的捕捉器。
handler.ownKeys()
Object.getOwnPropertyNames 方法和 Object.getOwnPropertySymbols 方法的捕捉器。
handler.apply()
函数调用操作的捕捉器。
handler.construct()
new 操作符的捕捉器。
一些不标准的捕捉器已经被废弃并且移除了。
````
## 示例
### 基础示例
在以下简单的例子中，当对象中不存在属性名时，默认返回值为 37。下面的代码以此展示了 get handler 的使用场景。
````javascript
const handler = {
    get: function(obj, prop) {
        return prop in obj ? obj[prop] : 37;
    }
};

const p = new Proxy({}, handler);
p.a = 1;
p.b = undefined;

console.log(p.a, p.b);      // 1, undefined
console.log('c' in p, p.c); // false, 37
````
### 无操作转发代理
在以下例子中，我们使用了一个原生 JavaScript 对象，代理会将所有应用到它的操作转发到这个对象上。
````javascript
let target = {};
let p = new Proxy(target, {});

p.a = 37;   // 操作转发到目标

console.log(target.a);    // 37. 操作已经被正确地转发
````
### 验证
通过代理，你可以轻松地验证向一个对象的传值。下面的代码借此展示了 set handler 的作用。
````javascript
let validator = {
  set: function(obj, prop, value) {
    if (prop === 'age') {
      if (!Number.isInteger(value)) {
        throw new TypeError('The age is not an integer');
      }
      if (value > 200) {
        throw new RangeError('The age seems invalid');
      }
    }

    // The default behavior to store the value
    obj[prop] = value;

    // 表示成功
    return true;
  }
};

let person = new Proxy({}, validator);

person.age = 100;

console.log(person.age);
// 100

person.age = 'young';
// 抛出异常: Uncaught TypeError: The age is not an integer

person.age = 300;
// 抛出异常: Uncaught RangeError: The age seems invalid
````
### 扩展构造函数
方法代理可以轻松地通过一个新构造函数来扩展一个已有的构造函数。这个例子使用了construct和apply。

````javascript
function extend(sup, base) {
  var descriptor = Object.getOwnPropertyDescriptor(
    base.prototype, "constructor"
  );
  base.prototype = Object.create(sup.prototype);
  var handler = {
    construct: function(target, args) {
      var obj = Object.create(base.prototype);
      this.apply(target, obj, args);
      return obj;
    },
    apply: function(target, that, args) {
      sup.apply(that, args);
      base.apply(that, args);
    }
  };
  var proxy = new Proxy(base, handler);
  descriptor.value = proxy;
  Object.defineProperty(base.prototype, "constructor", descriptor);
  return proxy;
}

var Person = function (name) {
  this.name = name
};

var Boy = extend(Person, function (name, age) {
  this.age = age;
});

Boy.prototype.sex = "M";

var Peter = new Boy("Peter", 13);
console.log(Peter.sex);  // "M"
console.log(Peter.name); // "Peter"
console.log(Peter.age);  // 13
````
### 操作 DOM 节点
有时，我们可能需要互换两个不同的元素的属性或类名。下面的代码以此为目标，展示了 set handler 的使用场景。
````javascript
let view = new Proxy({
  selected: null
}, {
  set: function(obj, prop, newval) {
    let oldval = obj[prop];

    if (prop === 'selected') {
      if (oldval) {
        oldval.setAttribute('aria-selected', 'false');
      }
      if (newval) {
        newval.setAttribute('aria-selected', 'true');
      }
    }

    // 默认行为是存储被传入 setter 函数的属性值
    obj[prop] = newval;

    // 表示操作成功
    return true;
  }
});

let i1 = view.selected = document.getElementById('item-1');
console.log(i1.getAttribute('aria-selected')); // 'true'

let i2 = view.selected = document.getElementById('item-2');
console.log(i1.getAttribute('aria-selected')); // 'false'
console.log(i2.getAttribute('aria-selected')); // 'true'
````

### 一个完整的 traps 列表示例
出于教学目的，这里为了创建一个完整的 traps 列表示例，我们将尝试代理化一个非原生对象，这特别适用于这类操作：由 发布在 document.cookie页面上的“小型框架” (en-US)创建的docCookies全局对象。

````javascript
/*
  var docCookies = ... get the "docCookies" object here:
  https://developer.mozilla.org/zh-CN/docs/DOM/document.cookie#A_little_framework.3A_a_complete_cookies_reader.2Fwriter_with_full_unicode_support
*/

var docCookies = new Proxy(docCookies, {
  "get": function (oTarget, sKey) {
    return oTarget[sKey] || oTarget.getItem(sKey) || undefined;
  },
  "set": function (oTarget, sKey, vValue) {
    if (sKey in oTarget) { return false; }
    return oTarget.setItem(sKey, vValue);
  },
  "deleteProperty": function (oTarget, sKey) {
    if (sKey in oTarget) { return false; }
    return oTarget.removeItem(sKey);
  },
  "enumerate": function (oTarget, sKey) {
    return oTarget.keys();
  },
  "ownKeys": function (oTarget, sKey) {
    return oTarget.keys();
  },
  "has": function (oTarget, sKey) {
    return sKey in oTarget || oTarget.hasItem(sKey);
  },
  "defineProperty": function (oTarget, sKey, oDesc) {
    if (oDesc && "value" in oDesc) { oTarget.setItem(sKey, oDesc.value); }
    return oTarget;
  },
  "getOwnPropertyDescriptor": function (oTarget, sKey) {
    var vValue = oTarget.getItem(sKey);
    return vValue ? {
      "value": vValue,
      "writable": true,
      "enumerable": true,
      "configurable": false
    } : undefined;
  },
});

/* Cookies 测试 */

alert(docCookies.my_cookie1 = "First value");
alert(docCookies.getItem("my_cookie1"));

docCookies.setItem("my_cookie1", "Changed value");
alert(docCookies.my_cookie1);
````

# instanceof

>instanceof 运算符用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上。

## 语法
```javascript
object instanceof constructor
```
## 参数
    object
    某个实例对象
    constructor
    某个构造函数
## 描述
>instanceof 运算符用来检测 constructor.prototype 是否存在于参数 object 的原型链上。
```javascript
// 定义构造函数
function C(){}
function D(){}

var o = new C();


o instanceof C; // true，因为 Object.getPrototypeOf(o) === C.prototype


o instanceof D; // false，因为 D.prototype 不在 o 的原型链上

o instanceof Object; // true，因为 Object.prototype.isPrototypeOf(o) 返回 true
C.prototype instanceof Object // true，同上

C.prototype = {};
var o2 = new C();

o2 instanceof C; // true

o instanceof C; // false，C.prototype 指向了一个空对象,这个空对象不在 o 的原型链上.

D.prototype = new C(); // 继承
var o3 = new D();
o3 instanceof D; // true
o3 instanceof C; // true 因为 C.prototype 现在在 o3 的原型链上
```

_需要注意的是，如果表达式 obj instanceof Foo 返回 true，则并不意味着该表达式会永远返回 true，因为 Foo.prototype 属性的值有可能会改变，改变之后的值很有可能不存在于 obj 的原型链上，这时原表达式的值就会成为 false。另外一种情况下，原表达式的值也会改变，就是改变对象 obj 的原型链的情况，虽然在目前的ES规范中，我们只能读取对象的原型而不能改变它，但借助于非标准的 __proto__ 伪属性，是可以实现的。比如执行 obj.__proto__ = {} 之后，obj instanceof Foo 就会返回 false 了。_

>instanceof 和多全局对象(例如：多个 frame 或多个 window 之间的交互)

>>在浏览器中，我们的脚本可能需要在多个窗口之间进行交互。多个窗口意味着多个全局环境，不同的全局环境拥有不同的全局对象，从而拥有不同的内置类型构造函数。这可能会引发一些问题。比如，表达式 [] instanceof window.frames[0].Array 会返回 false，因为 Array.prototype !== window.frames[0].Array.prototype，并且数组从前者继承。

>>起初，你会认为这样并没有意义，但是当你在你的脚本中开始处理多个 frame 或多个 window 以及通过函数将对象从一个窗口传到另一个窗口时，这就是一个有效而强大的话题。比如，实际上你可以通过使用Array.isArray(myObj) 或者Object.prototype.toString.call(myObj) === "[object Array]" 来安全的检测传过来的对象是否是一个数组。

>>比如检测一个 Nodes 在另一个窗口中是不是 SVGElement，你可以使用myNode instanceof myNode.ownerDocument.defaultView.SVGElement

# Object.defineProperties()

>Object.defineProperties() 方法直接在一个对象上定义新的属性或修改现有属性，并返回该对象。
## 语法

```javascript
Object.defineProperties(obj, props)
```
## 参数

    obj
    在其上定义或修改属性的对象。
    props
    要定义其可枚举属性或修改的属性描述符的对象。对象中存在的属性描述符主要有两种：数据描述符和访问器描述符（更多详情，请参阅Object.defineProperty()）。描述符具有以下键：
        configurable
          true 只有该属性描述符的类型可以被改变并且该属性可以从对应对象中删除。
          默认为 false
        enumerable
          true 只有在枚举相应对象上的属性时该属性显现。
          默认为 false
        value
          与属性关联的值。可以是任何有效的JavaScript值（数字，对象，函数等）。
          默认为 undefined.
        writable
          true只有与该属性相关联的值被assignment operator改变时。
          默认为 false
        get
          作为该属性的 getter 函数，如果没有 getter 则为undefined。函数返回值将被用作属性的值。
          默认为 undefined
        set
          作为属性的 setter 函数，如果没有 setter 则为undefined。函数将仅接受参数赋值给该属性的新值。
          默认为 undefined
    返回值
      传递给函数的对象。

## 描述
>Object.defineProperties本质上定义了obj 对象上props的可枚举属性相对应的所有属性。

## 例子
```javascript
var obj = {};
Object.defineProperties(obj, {
  'property1': {
    value: true,
    writable: true
  },
  'property2': {
    value: 'Hello',
    writable: false
  }
  // etc. etc.
});
```
## Polyfill

>假设一个原始的执行环境，所有的名称和属性都引用它们的初始值，Object.defineProperties几乎完全等同于（注意isCallable中的注释）以下JavaScript中的重新实现：

```javascript
function defineProperties(obj, properties) {
  function convertToDescriptor(desc) {
    function hasProperty(obj, prop) {
      return Object.prototype.hasOwnProperty.call(obj, prop);
    }

    function isCallable(v) {
      // NB: modify as necessary if other values than functions are callable.
      return typeof v === 'function';
    }

    if (typeof desc !== 'object' || desc === null)
      throw new TypeError('bad desc');

    var d = {};

    if (hasProperty(desc, 'enumerable'))
      d.enumerable = !!desc.enumerable;
    if (hasProperty(desc, 'configurable'))
      d.configurable = !!desc.configurable;
    if (hasProperty(desc, 'value'))
      d.value = desc.value;
    if (hasProperty(desc, 'writable'))
      d.writable = !!desc.writable;
    if (hasProperty(desc, 'get')) {
      var g = desc.get;

      if (!isCallable(g) && typeof g !== 'undefined')
        throw new TypeError('bad get');
      d.get = g;
    }
    if (hasProperty(desc, 'set')) {
      var s = desc.set;
      if (!isCallable(s) && typeof s !== 'undefined')
        throw new TypeError('bad set');
      d.set = s;
    }

    if (('get' in d || 'set' in d) && ('value' in d || 'writable' in d))
      throw new TypeError('identity-confused descriptor');

    return d;
  }

  if (typeof obj !== 'object' || obj === null)
    throw new TypeError('bad obj');

  properties = Object(properties);

  var keys = Object.keys(properties);
  var descs = [];

  for (var i = 0; i < keys.length; i++)
    descs.push([keys[i], convertToDescriptor(properties[keys[i]])]);

  for (var i = 0; i < descs.length; i++)
    Object.defineProperty(obj, descs[i][0], descs[i][1]);

  return obj;
}
```
# Object.seal()

> Object.seal()方法封闭一个对象，阻止添加新属性并将所有现有属性标记为不可配置。当前属性的值只要原来是可写的就可以改变。

## 语法
````javascript
Object.seal(obj)
````
## 参数

  obj
  将要被密封的对象。
  返回值
  被密封的对象。

## 描述

>通常，一个对象是可扩展的（可以添加新的属性）。密封一个对象会让这个对象变的不能添加新属性，且所有已有属性会变的不可配置。属性不可配置的效果就是属性变的不可删除，以及一个数据属性不能被重新定义成为访问器属性，或者反之。但属性的值仍然可以修改。尝试删除一个密封对象的属性或者将某个密封对象的属性从数据属性转换成访问器属性，结果会静默失败或抛出TypeError（在严格模式 中最常见的，但不唯一）。

>不会影响从原型链上继承的属性。但 __proto__ (  ) 属性的值也会不能修改。

>返回被密封对象的引用。

## 例子
````javascript
var obj = {
  prop: function() {},
  foo: 'bar'
};

// 可以添加新的属性
// 可以更改或删除现有的属性
obj.foo = 'baz';
obj.lumpy = 'woof';
delete obj.prop;

var o = Object.seal(obj);

o === obj; // true
Object.isSealed(obj); // === true

// 仍然可以修改密封对象的属性值
obj.foo = 'quux';


// 但是你不能将属性重新定义成为访问器属性
// 反之亦然
Object.defineProperty(obj, 'foo', {
  get: function() { return 'g'; }
}); // throws a TypeError

// 除了属性值以外的任何变化，都会失败.
obj.quaxxor = 'the friendly duck';
// 添加属性将会失败
delete obj.foo;
// 删除属性将会失败

// 在严格模式下，这样的尝试将会抛出错误
function fail() {
  'use strict';
  delete obj.foo; // throws a TypeError
  obj.sparky = 'arf'; // throws a TypeError
}
fail();

// 通过Object.defineProperty添加属性将会报错
Object.defineProperty(obj, 'ohai', {
  value: 17
}); // throws a TypeError
Object.defineProperty(obj, 'foo', {
  value: 'eit'
}); // 通过Object.defineProperty修改属性值
````
## 注意
>在ES5中，如果这个方法的参数不是一个（原始）对象，那么它将导致TypeError。在ES2015中，非对象参数将被视为已被密封的普通对象，会直接返回它。

````javascript
Object.seal(1);
// TypeError: 1 is not an object (ES5 code)

Object.seal(1);
// 1                             (ES2015 code)
````

# Object.freeze()
>使用Object.freeze()冻结的对象中的现有属性值是不可变的。用Object.seal()密封的对象可以改变其现有属性值。

# Object.getOwnPropertyDescriptor()
> Object.getOwnPropertyDescriptor() 方法返回指定对象上一个自有属性对应的属性描述符。（自有属性指的是直接赋予该对象的属性，不需要从原型链上进行查找的属性）

## 语法
````javascript
Object.getOwnPropertyDescriptor(obj, prop)
````
## 参数
    obj
    需要查找的目标对象
    prop
    目标对象内属性名称
    返回值
    如果指定的属性存在于对象上，则返回其属性描述符对象（property descriptor），否则返回 undefined。
  
## 举例
````javascript
const object1 = {
  property1: 42
};

const descriptor1 = Object.getOwnPropertyDescriptor(object1, 'property1');

console.log(descriptor1.configurable);
// expected output: true

console.log(descriptor1.value);
// expected output: 42
````

# Object.create()

>Object.create()方法创建一个新对象，使用现有的对象来提供新创建的对象的_proto__(请打开浏览器控制台以查看运行结果.))

## 语法
```javascript
Object.create(proto，[propertiesObject])
```

## 参数
```javascript
proto
新创建对象的原型对象.
propertiesObject
可选.需要传入一个对象，该对象的属性类型参照Object.defineProperties()的第二个参数.如果该参数被指定且不为undefined、该传入对象的自有可枚举属性(即其自身定义的属性，而不是其原型链上的枚举属性)将为新创建的对象添加指定的属性值和对应的属性描述符.
返回值
一个新对象，带着指定的原型对象和属性.

例外
如果propertiesObject参数是null或非原始包装对象，则抛出一个TypeError异常.
```

## 例子

用 Object.create实现类式继承
下面的例子演示了如何使用Object.create()来实现类式继承.这是一个所有版本JavaScript都支持的单继承。
```javascript
// Shape - 父类(superclass)
function Shape() {
  this.x = 0;
  this.y = 0;
}

// 父类的方法
Shape.prototype.move = function(x, y) {
  this.x += x;
  this.y += y;
  console.info('Shape moved.');
};

// Rectangle - 子类(subclass)
function Rectangle() {
  Shape.call(this); // call super constructor.
}

// 子类续承父类
Rectangle.prototype = Object.create(Shape.prototype);
Rectangle.prototype.constructor = Rectangle;

var rect = new Rectangle();

console.log('Is rect an instance of Rectangle?',
  rect instanceof Rectangle); // true
console.log('Is rect an instance of Shape?',
  rect instanceof Shape); // true
rect.move(1, 1); // Outputs, 'Shape moved.'

如果你希望能继承到多个对象，则可以使用混入的方式.

function MyClass() {
     SuperClass.call(this);
     OtherSuperClass.call(this);
}

// 继承一个类
MyClass.prototype = Object.create(SuperClass.prototype);
// 混合其它
Object.assign(MyClass.prototype, OtherSuperClass.prototype);
// 重新指定constructor
MyClass.prototype.constructor = MyClass;

MyClass.prototype.myMethod = function() {
     // do a thing
};
```
Object.assign会把OtherSuperClass原型上的函数拷贝到MyClass原型上，使MyClass的所有实例都可用Other SuperClass的方法.指定是在ES 2015引入的，且可用多填充...要支持旧浏览器的话，可用使用JQuery.EXTEND()或者_.分配()。

使用Object.create 的 propertyObject参数
```javascript
var o;

// 创建一个原型为null的空对象
o = Object.create(null);


o = {};
// 以字面量方式创建的空对象就相当于:
o = Object.create(Object.prototype);


o = Object.create(Object.prototype, {
  // foo会成为所创建对象的数据属性
  foo: {
    writable:true,
    configurable:true,
    value: "hello"
  },
  // bar会成为所创建对象的访问器属性
  bar: {
    configurable: false,
    get: function() { return 10 },
    set: function(value) {
      console.log("Setting `o.bar` to", value);
    }
  }
});


function Constructor(){}
o = new Constructor();
// 上面的一句就相当于:
o = Object.create(Constructor.prototype);
// 当然,如果在Constructor函数中有一些初始化代码,Object.create不能执行那些代码


// 创建一个以另一个空对象为原型,且拥有一个属性p的对象
o = Object.create({}, { p: { value: 42 } })

// 省略了的属性特性默认为false,所以属性p是不可写,不可枚举,不可配置的:
o.p = 24
o.p
//42

o.q = 12
for (var prop in o) {
   console.log(prop)
}
//"q"

delete o.p
//false

//创建一个可写的,可枚举的,可配置的属性p
o2 = Object.create({}, {
  p: {
    value: 42,
    writable: true,
    enumerable: true,
    configurable: true
  }
});
```
## polyfill
>这个聚填充涵盖了主要的应用场景，它创建一个已经选择了原型的新对象，但没有把第二个参数考虑在内.

>请注意，尽管在ES5中Object.create支持设置为[[Prototype]]为null、但因为那些ECMAScript5以前版本限制，此Poly填充无法支持该特性.

if (typeof Object.create !== "function") {
    Object.create = function (proto, propertiesObject) {
        if (typeof proto !== 'object' && typeof proto !== 'function') {
            throw new TypeError('Object prototype may only be an Object: ' + proto);
        } else if (proto === null) {
            throw new Error("This browser's implementation of Object.create is a shim and doesn't support 'null' as the first argument.");
        }

        if (typeof propertiesObject !== 'undefined') throw new Error("This browser's implementation of Object.create is a shim and doesn't support a second argument.");

        function F() {}
        F.prototype = proto;

        return new F();
    };
}

