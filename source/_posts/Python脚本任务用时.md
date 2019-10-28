---
title: Python脚本任务用时
date: 2019-10-27 18:24:40
tags:
  - python
categories:
  - 技术
---

>很简单，别忘了导入time模块

![python001](https://s3-cn-east-1.qiniucs.com/jiebaiyou-blog/python001.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=LW523SRVcyaYd2iOH9tpm7pW0k-XMECg4kT6rYFt%2F20191028%2Fcn-east-1%2Fs3%2Faws4_request&X-Amz-Date=20191028T064427Z&X-Amz-Expires=600&X-Amz-Signature=486ced9856a812d1f1cfe2c16e7b3ded382d4e73b92d352775b2c43941d64962&X-Amz-SignedHeaders=host)

```
import time
start = time.time()
(要执行的代码...)
end = time.time()
print ('添加任务用时：', end - start)
```
