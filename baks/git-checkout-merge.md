---
title: Git中Checkout、Merge的流程
date: 2019-05-30 14:36:52
tags: git
---

#### 基本流程

{% img img-class /images/git-base.png %}

#### 注意：
1.功能开发、hotfix修复必须从master切分支
2.不要把develop、staging分支merge到自己的开发分支，避免造成污染

<!--more-->

#### 特殊情况

假设预发布环境上面需要验证featA+featB+featC的功能，但是上线需要先上线featA+featB的功能，而featC功能往后搁置。

这时候需要一个临时分支（combine分支）

1.先从master切出一个combine分支
2.把featA、featB合并到这个combine分支
3.把combine分支合并到staging进行测试，后续流程与基本流程一致

备注：或是combine分支可以直接合并到master打个tag然后先放到预发布环境验证

{% img img-class /images/git-special.png %}