---
title: 快速搭建可用于实战的koa2+mongodb框架
date: 2018-12-04 17:41:30
tags:
---

### 1 准备工作
mac下安装mongodb可以参考这里：{% link Mac安装Mongdb - chendong_的博客 - CSDN博客 https://blog.csdn.net/chendong_/article/details/51440986 %}

源码地址戳这里：{% link chenyucai/hello-koa2-mongodb https://github.com/chenyucai/hello-koa2-mongodb %}

ps：GitHub上面的源码直接链接了我本人的云数据库，可临时提供给小伙伴们调试与使用。



### 2 使用koa-generator生成koa2项目

#### 2.1 全局安装koa-generator
```
npm install -g koa-generator
```

<!-- more -->

#### 2.2 使用命令生成项目基本目录
```
koa2 hello-koa2-mongodb
```
{% img img-class https://pic3.zhimg.com/80/v2-7f443905845c141a4958f77ab4a57172_hd.jpg %}
#### 2.3 创建项目成功之后进入项目目录，进行依赖安装
```
cd hello-koa2-mongodb && npm install
```
{% img img-class https://pic1.zhimg.com/80/v2-f861c4d4b150deba60f99c5fc473ace4_hd.jpg %}
#### 2.4 运行命令预览一下
```
npm start
```
浏览器打开http://localhost:3000/
{% img img-class https://pic1.zhimg.com/80/v2-e9f4621a52730c47d2453d7f29e3d4d8_hd.jpg %}


### 3 目录结构
因为现在的项目基本上都是前后端分离，所以我这里只编写的框架中并不带模版。然后我们需要改造一下目录结构，详细代码可移步{% link 这里 https://github.com/chenyucai/hello-koa2-mongodb %}
{% img img-class https://pic3.zhimg.com/80/v2-4037599e03dbe76f7743f0115c8fcdfe_hd.jpg %}

config: 一些公共的配置，如数据库地址

controller: 控制器层

dbHelper: 链接mongodb

error: 实现统一异常处理

middleware: 各种中间件

model: 实体类

router: 路由信息（api接口地址）

utils: 各种工具类



### 4 代码实现
koa的项目中引用文件都是采用require，对于习惯了使用import关键字的小伙伴可以这样解决：在项目中引入babel-plugin-add-module-exports与babel-register
```
npm install babel-plugin-add-module-exports babel-register --save-dev
```
并在根目录下创建一个.babelrc的文件
```
{
  "presets": ["es2015", "stage-3"],
  "plugins": ["add-module-exports"]
}
```
并在入口文件中require('babel-register')



#### 4.1 入口文件：src/app.js
{% img img-class https://pic4.zhimg.com/80/v2-e438173ab038cc3a2bf67dc5b5363e6b_hd.jpg %}

#### 4.2 连接mongodb等设置：src/config/index.js
{% img img-class https://pic3.zhimg.com/80/v2-654f0a5f51b4c92e8cdcb52634849e16_hd.jpg %}

#### 4.3 router，路由信息，即api地址：src/router/index.js
{% img img-class https://pic1.zhimg.com/80/v2-a586038a541ff1e4fdbe5ebab71c7a30_hd.jpg %}

#### 4.4 controller控制器
{% img img-class https://pic1.zhimg.com/80/v2-c40ffb28db5fb40e7697548b04aad4c0_hd.jpg %}

这里使用了class，习惯写fucntion的小伙伴用function也是一样的



#### 4.5 model实体
{% img img-class https://pic4.zhimg.com/80/v2-6b83ac38de0ade17c30cc03dd358f863_hd.jpg %}

下面这样可以配置mongoose自动更新createTime和updateTime

{% img img-class https://pic3.zhimg.com/80/v2-5d93845dd13ab454484980006d92cd02_hd.jpg %}


### 5 统一异常处理
如果是写java的小伙伴都知道，统一的异常处理是非常有必要的

ApiErrorNames.js定义异常code码
{% img img-class https://pic3.zhimg.com/80/v2-f7c6b01f2427425662365ed46334083a_hd.jpg %}

ApiError.js实现统一异常处理
{% img img-class https://pic3.zhimg.com/80/v2-47a87137277fc57fa3cb83dd9999e992_hd.jpg %}

注意：为什么不用class，因为用了babel，class会被编译成es5，导致instanceof没用


### 6 jwt验证
使用jsonwebtoken库
```
npm i jsonwebtoken  // 一个实现jwt的包
```
自己实现一个jwt校验中间件 （也可以直接使用koa-jwt）
{% img img-class https://pic2.zhimg.com/80/v2-21135d1013b76255226e0386b6a78941_hd.jpg %}

在需要校验的接口上加上verify就行了
{% img img-class https://pic1.zhimg.com/80/v2-6f60bca9de6c89923d52e56afcaeec84_hd.jpg %}

<br>


ps: service也要的，后续加上