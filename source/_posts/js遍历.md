---
title: js遍历
date: 2021-02-25 10:11:23
tags: 
    - 数组
    - es6
    - 遍历
categories: 
    - 前端
---
# map

+ 返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值。

+ 按照原始数组元素顺序依次处理元素。

+ 不会改变原数组
```javascript
let arr = new Array(3).fill(0)
let arr1 = arr.map((currentValue,index,array)=>currentValue*index)
// currentValue 必须。当前元素的值
// index 可选。当前元素的索引值
// array 可选。当前元素属于的数组对象
```

# filter

+ 创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。

+ 不会改变原数组
```javascript
let arr = new Array(3).fill(0)
let arr1 = arr.filter((currentValue,index,array)=>currentValue>index)
// currentValue 必须。当前元素的值
// index 可选。当前元素的索引值
// array 可选。当前元素属于的数组对象
```


#  find

+ 返回通过测试（函数内判断）的数组的第一个元素的值。

+ 方法为数组中的每个元素都调用一次函数执行：

  - 当数组中的元素在测试条件时返回 true 时, find() 返回符合条件的元素，之后的值不会再调用执行函数。
  - 如果没有符合条件的元素返回 undefined

+ 并没有改变数组的原始值。
```javascript
let arr = new Array(3).fill(0)
let arr1 = arr.find((currentValue,index,array)=>currentValue===index)
// currentValue 必须。当前元素的值
// index 可选。当前元素的索引值
// array 可选。当前元素属于的数组对象
```



#  findIndex

+ 返回传入一个测试条件（函数）符合条件的数组第一个元素位置。

+ 方法为数组中的每个元素都调用一次函数执行：

  - 当数组中的元素在测试条件时返回 true 时, findIndex() 返回符合条件的元素的索引位置，之后的值不会再调用执行函数。
  - 如果没有符合条件的元素返回 -1

+ 并没有改变数组的原始值。
```javascript
let arr = new Array(3).fill(0)
let arr1 = arr.findIndex((currentValue,index,array)=>currentValue===index)
// currentValue 必须。当前元素的值
// index 可选。当前元素的索引值
// array 可选。当前元素属于的数组对象
```


# forEach

+ 用于调用数组的每个元素，并将元素传递给回调函数。
+ 遍历之后添加的元素不会被遍历
+ 不会改变原数组
+ 没办法终止
```javascript
let arr = new Array(3).fill(0)
let sum=0
let arr1 = arr.forEach((currentValue,index,array)=>sum+=currentValue)
// currentValue 必须。当前元素的值
// index 可选。当前元素的索引值
// array 可选。当前元素属于的数组对象
```

# every

+ 测试一个数组内的所有元素是否都能通过某个指定函数的测试。它返回一个布尔值
+ 若收到一个空数组，此方法在一切情况下都会返回 true
```javascript
let arr = new Array(3).fill(0)
let arr1 = arr.every((currentValue,index,array)=>currentValue<10)
// currentValue 必须。当前元素的值
// index 可选。当前元素的索引值
// array 可选。当前元素属于的数组对象
```  


# entries()，keys() 和 values() 

+ 用for...of循环进行遍历，唯一的区别是keys()是对键名的遍历、values()是对键值的遍历，entries()是对键值对的遍历。
```javascript
for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
// 1 "b"


如果不使用for...of循环，可以手动调用遍历器对象的next方法，进行遍历。

let letter = ['a', 'b', 'c'];
let entries = letter.entries();
console.log(entries.next().value); // [0, 'a']
console.log(entries.next().value); // [1, 'b']
console.log(entries.next().value); // [2, 'c']

```


