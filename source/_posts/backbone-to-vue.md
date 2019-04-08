---
title: 从Backbone迁移到Vue
date: 2017-01-07 18:15:32
tags:
---

目前公司用的前端技术栈是require.js + backbone.js + zepto + underscore ，是我以前写的，差不多用了两年，还算稳定。最近公司打算优化系统架构，所以我也对目前使用的前端框架梳理了一遍，发现存在一些致命的缺陷，这些缺陷我觉得是框架本身所决定的，很难从编码能力上解决。先来看下现在的框架结构图
<!-- more -->
{% img img-class https://pic1.zhimg.com/80/v2-f8f0c2378ed3bff3f41eb6b6dbbd3314_hd.png %}


不难看出，Backbone的Model层比较简单，一个Collection由多个同类的Model组成，这样的设计说明什么呢，说明Backbone的Model层比较适合做CRUD的操作，不太适合做业务处理。所以很多逻辑处理都写在了Backbone的View对象里面，一些数据的状态会在Model、Collection、View三者之间流来流去，流来流去，写着写着.....就崩了.....代码怎么这么乱！以及需要手动去更新dom一些麻烦的操作等等。这里我就不多说了...


所以基于这些问题我就去找解决方案，第一个想到的就是Vue，不是因为现在业界对Vue的评价有多高，而是我自己有一些项目都是用Vue写的，熟悉，不过不得不说，Vue非常好用。当然，架构的重新调整不是简单的考虑你比较熟悉某种框架就用那个框架，你考虑的要很多，你要去结合业务等等。


我们公司这个项目承载了很多的客户，意思是一套系统要适配多个客户，因为客户的不同，所以就产生出一些个性化的需求，个性化的需求会产生出个性化的代码，比如页面展示不同，js处理不同等。那么这部分代码怎么很好的管理起来呢，我们首先考虑到的是具有组件化思想的React和Vue。


所以，不仅Vue，我还去研究了一下React，以及React+Redux衍生出的dva。Vue的后来讨论下来vue可能更符合当前团队的技术特点。以及我们可以把每个个性化需求都封装成一个.vue，然后很好的管理起来，以及配合Vuex的单向数据流模型也能很好的去解决state流向混乱和无法预测的问题。所以我们打算用Vue，


配套工程体系那就是：vue + vuex + axios + es6 + webpack + vue-router

下面是大概的一个框架设计图
{% img img-class https://pic2.zhimg.com/80/v2-ab00e2fcfb809c43ab282700d8159279_hd.png %}



从上面的图可以看出



1.非常清晰的单向数据流模型，这样任何时候任何地方的state变更我们都能预测到。

2.我们使代码结构分层，并遵循component中stateless的原则，把所有关于业务逻辑有关的代码全都封装在store的mutations和actions中，component只允许少量页面逻辑处理的state，即vue实例的data。另外，我们封装出一个层来管理所有的request请求，通过module分模块管理，即server层。

3.同步和异步处理分离：同步处理只能在Mutations中操作，异步处理在Actions中操作。Actions和server层进行通信，进行request请求。

.........

更多的细节我就不多说啦，大家可以去参考。本人也表示非常的感谢！

* 尤老师的{% link vue.js https://cn.vuejs.org/ %}

* Vuex：{% link vuejs/vuex https://github.com/vuejs/vuex/ %}

* 蚂蚁金服的{% link dvajs/dva https://github.com/dvajs/dva %}


<br>

写在最后：上面说的其实都偏理论化和技术化，在实际的架构设计或者框架迁移中，要考虑的东西非常非常多，比如目前系统的问题所在、目前系统的代码量、能给公司带来什么价值和收益，此外，架构还要关注效率和质量，对于中小型企业来说，效率要比质量要多关注一点。等等。。。
