---
title: 自己实现array.map()、array.filter()
date: 2018-02-08 22:54:57
tags:
---

## Array.map
先看看map的用法
{% blockquote %}
var new_array = arr.map(function callback(currentValue[, index[, array]]) {
 // Return element for new_array
}[, thisArg])
{% endblockquote %}

<!-- more -->

{% blockquote %}
**参数**
callback
    &nbsp;&nbsp;&nbsp;生成新数组元素的函数，使用三个参数：
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;currentValue - callback数组中正在处理的当前元素。
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;index可选 - callback数组中正在处理的当前元素的索引。
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;array可选 - callback map 方法被调用的数组。
thisArg可选
    &nbsp;&nbsp;&nbsp;执行 callback 函数时使用的this 值。

**返回值**
一个新数组，每个元素都是回调函数的结果
{% endblockquote %}

简单实现
```javascript
Array.prototype.selfMap = function () {

  // 这两个参数也可以通过行参传入
  let [fn, thisArg] = [].slice.call(arguments);

  let arr = this; // 数组本身

  let result = [];

  if (typeof fn !== 'function') {
    throw new TypeError(`${fn} is not a function`);
  }

  for (let i = 0; i < arr.length; i++) {
    result.push(fn.call(thisArg, arr[i], i, arr));
  }

  return result;
};

let arr = [1,2,3,4,5];
let result = arr.selfMap(item => item + 1);
console.log(result); // [2,3,4,5,6]
```

## Array.filter
先看一下filter的用法
{% blockquote %}
var newArray = arr.filter(callback(element[, index[, array]])[, thisArg])
{% endblockquote %}
{% blockquote %}
**参数**
callback
    &emsp;&emsp;用来测试数组的每个元素的函数。返回 true 表示该元素通过测试，保留该元素，false 则不保留。它接受一下三个参数：
                &emsp;&emsp;element - 数组中当前正在处理的元素。
                &emsp;&emsp;index可选 - 正在处理的元素在数组中的索引。
                &emsp;&emsp;array可选 - 调用了 filter 的数组本身。
thisArg可选
    &emsp;&emsp;执行 callback 时，用于 this 的值。
**返回值**
    &emsp;&emsp;一个新的、由通过测试的元素组成的数组，如果没有任何数组元素通过测试，则返回空数组。
{% endblockquote %}

简单实现
```javascript
Array.prototype.selfFilter = function () {
  let [fn, thisArg] = [].slice.call(arguments);

  let arr = this;

  let result = [];

  for (let i = 0; i < arr.length; i++) {
    if (fn.call(thisArg, arr[i], i, arr)) {
      result.push(arr[i]);
    }
  }

  return result;
};

var arr = [1, 2, 3, 4, 5];
var result = arr.selfFilter(item => item > 3);
console.log(result); // [4, 5]
```
