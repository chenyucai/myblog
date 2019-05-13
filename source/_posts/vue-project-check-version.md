---
title: 项目线上代码更新版本后出现app.xxxx.js资源找不到的问题
date: 2019-05-13 17:14:43
tags:
---

通常来说，每次更新版本都应该有类似"蓝绿部署"的措施，一般通过集群和负载均衡来切流量，以保证发不完新版本后老用户流量不会出现资源未找到的情况。

不过当服务端还没有这种解决方案时，前端也可以用一个小技巧暂时处理
```javascript
function handleError (eventErr) {
        console.log(eventErr)
        if (eventErr.srcElement.localName === 'link' || eventErr.srcElement.localName === 'script') {
            var reg = /(app\.|chunk-|\d+\.)\w+(\.js$|\.css$)/;
            if (reg.test(eventErr.srcElement.src)) {
                alert('当前版本已更新，请刷新页面，如多次刷新无效，请联系管理员');
                window.location.reload();
            }
        }
        eventErr.preventDefault()
    };
    window.addEventListener('error', handleError, true);
```
