---
title: 简单实现一个Promise.all
date: 2019-03-25 16:33:26
tags: javascript
---


### 1.我们先来看看Promise.all的用法
{% codeblock lang:javascript %}
let p1 = new Promise((resolve, reject) => {
  resolve('p1')
})
let p2 = new Promise((resolve, reject) => {
  resolve('p2')
})
let p3 = Promise.reject('p3 error')

Promise.all([p1, p2]).then(results => {
  console.log(results)    // ['p1', 'p2']
}).catch(error => {
  console.log(error)
})

Promise.all([p1,p2,p3]).then(results => {
  console.log(results)
}).catch(error => {
  console.log(error)      // 'p3 error'
})
{% endcodeblock %}


可以看出，Promise.all输入是由多个Promise对象组成的一个数组，最后输出一个新的Promise对象。成功时返回的是一个结果数组，和输入的promise数组顺序是一致的，失败时则返回最先被reject的错误值。

<!-- more -->

### 2.简单实现一个
{% codeblock lang:javascript %}
  function promiseAll(promises) {
    return new Promise((resolve, reject) => {
      let resultCount = 0;
      let results = new Array(promises.length);

      for (let i = 0; i < promises.length; i++) {
        promises[i].then(value => {
          resultCount++;
          results[i] = value;
          if (resultCount === promises.length) {
            return resolve(results)
          }
        }, error => {
          reject(error)
        })
      }
    })
  }

  let p1 = new Promise(resolve => resolve('p1'))
  let p2 = new Promise(resolve => resolve('p2'))
  let p3 = Promise.reject('p3 error')

  promiseAll([p1, p2]).then(results => {
    console.log(results)    // ['p1', 'p2']
  }).catch(error => {
    console.log(error)
  })

  promiseAll([p1, p2, p3]).then(results => {
    console.log(results)
  }).catch(error => {
    console.log(error)      // 'p3 error'
  })
{% endcodeblock %}