---
title: Mac下配置多个 ssh-key
date: 2018-04-12 19:20:06
tags:
---


### 创建ssh-key
```
ssh-keygen -t rsa -f ~/.ssh/id_rsa.别名 -C "邮箱地址"
```

示例
```
ssh-keygen -t rsa -f ~/.ssh/id_rsa.github -C 'xxx@xxx.com'
ssh-keygen -t rsa -f ~/.ssh/id_rsa.gitee -C 'xxx@xxx.com'
```

<!-- more -->

### 配置config
```vi
# github
  Host github
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa.github

# gitee
  Host gitee
  HostName gitee.com
  User git
  IdentityFile ~/.ssh/id_rsa.gitee
```

Host是别名。如果只是为了区分github、gitee等，为了方便使用，建议和HostName一致，这样在clone git的时候不用考虑修改hostname

### 测试是否配置成功
ssh -T gitee
返回：Welcome to Gitee.com, xxx!
表示成功


ssh -T github
返回：Hi xinwen-mao! You've successfully authenticated, but GitHub does not provide shell access.
表示成功

{% blockquote %}
如果添加了ssh公钥仍然需要输入账号密码可以尝试
ssh-agent bash && ssh-add ~/.ssh/id_rsa
exit
{% endblockquote %}