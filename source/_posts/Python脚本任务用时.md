---
title: Python脚本任务用时
date: 2019-10-27 18:24:40
tags:
  - python
categories:
  - 技术
---

>很简单，别忘了导入time模块

```
import time
start = time.time()
(要执行的代码...)
end = time.time()
print ('添加任务用时：', end - start)
```
