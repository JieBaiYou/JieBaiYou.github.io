---
title: Redis中批量删除Key
tags:
  - redis
categories:
  - 技术分享
date: 2019-11-20 11:48:42
---



最近在自己的阿里云服务器上跑一个Redis容器，不小心监听了宿主机器的0.0.0.0地址，而且Redis server裸奔没密码，被嗅探到并植入了一堆辣鸡Key，网卡流量跑了接近1TB。还好是docker跑的，因为容器的隔离，宿主机器没被植入啥后门。修复的措施也比较简单，直接rm了docker容器，重新跑了一个redis，把端口修改为只监听127.0.0.1的本机地址，问题解决。

等等，容器的安全搞定了，那一堆Redis的Key怎么清理掉呢？搜索了一下，Redis本身并没有提供批量删除Key的功能。但是，我们可以用一些骚操作来实现批量Key的删除。

<!--more-->

大致使用到的骚操作如下：

```
redis-cli --scan --pattern users:* | xargs redis-cli del
```

如果你使用的Redis版本为4.0或者更高，还可以使用[`unlink`](https://redis.io/commands/unlink)命令来替代`del`命令:

```
redis-cli --scan --pattern users:* | xargs redis-cli unlink
```

### 所以，这个脚本到底实现了啥？

- 首先，我们使用`redis-cli --scan --pattern `模糊匹配出了所有以`users:`打头的Redis Key，每个Key会输出为一行。
- 然后，这样的输出结果，通过管道操作交给了`xargs`命令来处理，`xargs`命令负责把多行的输出合并为一行，并传递给`redis-cli del`命令。所以最终执行的效果类似于`redis-cli del   ...`
- 如果有几千个Key符合这样的匹配，都会通过`xargs`命令，传递给`redis-cli del`一并删除。



转载自： https://xiaozhou.net/redis-batch-delete-2019-06-07.html  喜欢的话去打赏作者吧