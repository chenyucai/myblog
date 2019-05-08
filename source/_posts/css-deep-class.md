---
title: Scoped内使用深度作用选择器
date: 2019-04-24 11:31:50
tags:
---

之前有小伙伴提出怎么局部修改element-ui的样式，这种情况下我们就可以使用深度作用选择器。

那什么是深度作用选择器呢？

<!-- more -->

如果你希望 scoped 样式中的一个选择器能够作用得“更深”，例如影响子组件，你可以使用 >>> 操作符
```javascript
<style scoped>
    .parent >>> .child {
        color: blue;
     }
</style>
```
上面的代码会被编译成
```javascript
.parent[data-v-f3a85fa2] .child { 
    color: blue;
}
```

但是如果用less或者sass的时候， >>>操作符是解析不了的，这种情况可以使用/deep/操作符。
```javascript
<style lang="less" scoped>
    .parent /deep/ .child {
        color: blue;
     }
</style>
```