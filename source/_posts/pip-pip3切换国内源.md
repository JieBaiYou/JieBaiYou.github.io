---
title: pip/pip3切换国内源
date: 2019-10-26 17:30:08
tags: 
    - python
categories:
    - 技术
---

>有时候pip的速度太慢，切换到国内的源很有必要

**创建并编辑pip配置文件**
```
mkdir ~/.pip && vim ~/.pip/pip.conf
```
**然后将下面这两行复制进去就好了**
```
[global]
index-url = https://mirrors.aliyun.com/pypi/simple
```
---
**国内其他pip源**
```
清华:
https://pypi.tuna.tsinghua.edu.cn/simple 

中国科技大学:
https://pypi.mirrors.ustc.edu.cn/simple/ 

华中理工大学:
http://pypi.hustunique.com/ 

山东理工大学:
http://pypi.sdutlinux.org/ 

豆瓣:
http://pypi.douban.com/simple/
```