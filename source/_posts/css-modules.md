---
title: CSS Scoped 和 CSS Modules
date: 2018-08-13 16:26:27
tags:
---

### css scoped
现在多数项目都提倡scoped这种技术，是一种避免冲突、私有化都概念
使用方法:
```javascript
<style scoped>
    h1 {
      color: blue;
    }
</style>
```
编译后结果如下
```javascript
h1[data-v-f3a85fa2] {
    color: blue;
}
```

<!-- more -->

优点
很大程度上实现了样式私有化，避免了相同类名带来的样式冲突

缺点
1.并不能完全避免冲突，例如父子组件定义了相同的class名，父组件的样式权重要大于子组件的
2.scoped内不能定义全局样式
3.样式覆盖需要使用更高权重的inline-style或important关键字

### css modules
css modules既不是官方标准，也不是浏览器特性，而是在构建过程中对css类名做了特殊处理，起到限定作用域的一种方式
使用方法
```javascript
<template>
    <div :class="$style.title">
        hello world!
    </div>
</template>
<style lang="less" module>
    .title {
        color: #5daf34;
    }
</style>
```
编译后结果如下
```javascript
<div class="index_title_Ta0VH">
    hello world!
</div>
```
可以看出经过css modules处理过的class加了特地前缀和hash，从而起到了命名唯一，也可以通过global设置全局样式。

并且还有很多高级用法，所以现在越来越多的人开始使用css modules，特别是能很好的结合jsx编程。

另外css modules在vue cli3.0上面也是开箱即用的。