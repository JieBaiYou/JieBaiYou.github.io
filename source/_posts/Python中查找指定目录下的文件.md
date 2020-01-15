---
title: Python中查找指定目录下的文件
tags:
  - python
categories:
  - 技术分享
date: 2019-12-16 08:58:19
---

```python
#!/usr/bin/env python
# coding=utf-8
import os
def findConfigFile(dir, name):
    for relpath, dirs, files in os.walk(dir):
        if name in files:
            full_path = os.path.join(dir, relpath, name)
            return os.path.normpath(os.path.abspath(full_path))

config = findConfigFile('/dir/', 'config.py')
print(config)
```
<!-- more -->
https://python3-cookbook.readthedocs.io/zh_CN/latest/c13/p09_find_files_by_name.html

还有其它几种方式:

os.listdir(path)
查看指定path下的文件，一般结合os.path.isfile(path)(是否为文件)使用递归对目录进行遍历
使用介绍 http://www.runoob.com/python/os-listdir.html

os.walk(top, topdown=True, οnerrοr=None, followlinks=False)
一般只传入第一个参数，即要遍历的路径，topdown指明遍历的顺序，该方法对于每个目录返回一个三元组，(dirpath, dirnames, filenames)。第一个是路径，第二个是路径下面的目录，第三个是路径下面的非目录（对于windows来说也就是文件）。通过for循环即可完成目录的递归了。
使用介绍 http://www.runoob.com/python/os-walk.html

glob.glob(path)
返回所有匹配的文件路径列表。它只有一个参数path，定义了文件路径匹配规则，这里可以是绝对路径，也可以是相对路径。
```python
[root@test ~]# python3
import glob
glob.glob('./*.py')
['./1.py', './3.py', './2.py']
```