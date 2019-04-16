---
title: setInterval计时器不准确的解决方法
date: 2017-05-10 11:22:16
tags:
---

我们知道js是单线程的，加上event loop的机制，拿setInterval拿来做倒计时，往往是不准确的。

我们先验证一下这个问题
```javascript
var startTime = new Date().getTime();
var count = 0;

// 创建一个耗时任务来模拟
setInterval(function () {
  var i = 0;
  while (i++ < 1000000000) {
  }
}, 0);

// 计时器
setInterval(function () {
  count++;
  // 打印延迟毫秒数
  var offset = new Date().getTime() - (startTime + count * 1000);
  console.log(offset)
}, 1000);
```

<!-- more -->

{% img img-class /images/WX20190410-120931@2x.png %}

从图中可以看出是有延迟的，因为计算机执行其他任务的时候也是耗时的，而且延迟会越来越严重。


我们得想想办法怎么减少这个延迟，做到相对准确一些。
大概思路是这样的：我们使用setTimeout，假设间隔时间是1000毫秒执行一次，如果这次延迟了700毫秒，那下一次执行的时间就为300毫秒，如果延迟超过1000毫秒，那下次执行时间就为0毫秒。

```javascript
var startTime = new Date().getTime();
var count = 0;

// 创建一个耗时任务来模拟
setInterval(function () {
  var i = 0;
  while (i++ < 800000000) {
  }
}, 0);

// 纠正误差
function fixed() {
  count++;
  // 延迟毫秒数
  var offset = new Date().getTime() - (startTime + count * 1000);
  // 下一次触发时间
  var nextTime = 1000 - offset;
  if (nextTime < 0) {
    nextTime = 0;
  }
  setTimeout(fixed, nextTime);

  console.log(offset)
}

setTimeout(fixed, 1000);
```

{% img img-class /images/WX20190410-121126-fixed@2x.png %}


ps：延迟和误差是无法避免的，我们只能尽可能的去校正误差。


