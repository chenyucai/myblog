---
title: 判断一个对象是不是Array
date: 2017-02-10 10:21:56
tags:
---

我们一般都是通过**typeof**来判断一个变量的类型，但是Object 和 Array比较特殊，无法通过typeof来判断
```javascript
var str = 'i am a string';
var num = 12;
var obj = {a: 1};
var arr = [1, 2, 3];

typeof str // 'string'
typeof num // 'number'
typeof obj // 'object'
typeof arr // 'object'
```

我们可以通过下面几个方法判断是否是数组

<!-- more -->

### instanceof
```javascript
var arr = [1, 2, 3]
```

```javascript
arr instanceof Array // true
```

### construtor

```javascript
arr.constructor === Array // true
```

### Array.isArray
```javascript
Array.isArray(arr) // true
```
不过这个api存在一些兼容性问题

### Object.prototype.toString.call(arr)
```javascript
Object.prototype.toString.call(arr) === '[object Array]' // true
```
可以自己实现一个
```javascript
function isArray(obj) {
    if (typeof obj === 'object') {
        return Object.prototype.toString.call(obj) === '[object Array]';
    }
    return false;
}
```