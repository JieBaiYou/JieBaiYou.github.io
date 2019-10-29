---
title: pip/pip3切换国内源
date: 2019-10-26 17:30:08
tags: 
  - python
categories:
  - 技术分享
---

>大家知道，pip是Python中非常方便易用的安装包管理器，但是在实际下载安装包的时候经常会连接不上或者下载速度特别慢，将pip更换为国内源，可以大大的提高安装成功率和速度。

<!-- more -->

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