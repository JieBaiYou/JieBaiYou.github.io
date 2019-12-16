---
title: Shell/Python中创建随机密码
tags:
  - shell
  - python
categories:
  - 技术分享
date: 2019-12-16 08:47:31
---

> 生成16位数字+字母大小写密码

##### SHELL 
```
head -c 160 /dev/urandom | tr -dc a-z0-9A-Z |head -c 16
cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w 16 | sed 1q
```

##### PYTHON

> python3中为string.ascii_letters,而python2下则可以使用string.letters和string.ascii_letters

```
#coding=utf-8
from random import choice
import string

def getPassword(length=8, chars=string.ascii_letters + string.digits):
    return ''.join([choice(chars) for i in range(length)])

print(getPassword(16))
```





