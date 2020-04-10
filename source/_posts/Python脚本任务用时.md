---
title: Python脚本任务用时
date: 2019-10-27 18:24:40
tags:
  - python
categories:
  - 技术分享
---

>很简单，别忘了导入time模块

##### 第一种方法：

```python
import time
start = time.time()
(要执行的代码...)
end = time.time()
print ('添加任务用时：', end - start)
```
##### 第二种方法
```python
# import time
# 函数运行时间装饰器
def run_time(func):
    # 定义嵌套函数，用来打印出装饰的函数的执行时间
    def wrapper(*args, **kwargs):
        # 定义开始时间和结束时间，将func夹在中间执行，取得其返回值
        start = time.time()
        func_return = func(*args, **kwargs)
        end = time.time()
        # 打印方法名称和其执行时间
        log.info(f'函数名:{func.__name__}() 执行时间: {end - start}s') #日志输出
        print(f'函数名:{func.__name__}() 执行时间: {end - start}s') #打印输出
        # 返回func的返回值
        return func_return
    # 返回嵌套的函数
    return wrapper
```

