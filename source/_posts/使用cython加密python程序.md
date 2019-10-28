---
title: 使用cython加密python程序
tags:
  - python
categories:
  - 技术
date: 2019-10-28 16:27:28
---

>Python简单易用，最近因为工作原因也在学习。有时候一些敏感信息不希望通过源码查看到，所以需要加密保证安全性。

<!-- more -->

#### 首先在需要加密的python文件头部添加以下行
```
cython : language_level=3
```

#### 安装cython
```
pip install cython
or
pip3 install cython
```
#### 设置环境变量
```
# 将cython执行文件目录加入系统环境变量,一般在python安装目录下的bin文件夹
CYTHON_DIR=/usr/local/python3/bin
export PATH=$PATH:$CYTHON_DIR

# 确定python安装库文件目录
PYTHON_INCLUDE_DIR=/usr/local/python3/include/python3.7m
```

#### 程序加密
```
# 将文件转换为.c文件
cython app.py

# gcc将.c文件编译成o文件
gcc -c -fPIC -I$PYTHON_INCLUDE_DIR app.c 

# gcc将.o文件编译成so文件
gcc -shared app.o -o app.so

# 如果不需要原文件了，可以删除（强烈建议备份）
# rm app.c app.o app.py
```

#### 加密后程序的使用
```
import app
or
from app import class_name
```

*如果需要加密的文件比较多，可以写个简单的脚本批量处理。*

















