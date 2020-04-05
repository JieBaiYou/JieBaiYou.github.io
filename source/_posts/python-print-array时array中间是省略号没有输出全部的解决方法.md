---
title: python print array时array中间是省略号没有输出全部的解决方法
tags:
  - python
  - numpy
categories:
  - 技术分享
date: 2020-03-25 16:23:25
---

解决方法如下，在开头加入：

```
import numpy as np
np.set_printoptions(threshold=np.inf)
```

