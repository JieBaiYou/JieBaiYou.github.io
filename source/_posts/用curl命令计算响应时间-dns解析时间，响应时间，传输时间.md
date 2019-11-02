---
title: 用curl命令计算响应时间(dns解析时间，响应时间，传输时间)
tags:
  - linux
  - curl
categories:
  - 技术分享
date: 2019-11-02 10:38:18
---

#### 计算响应时间

```shell
curl -o /dev/null -s -w " http_code:%{http_code}\n time_namelookup:%{time_namelookup}\n time_redirect:%{time_redirect}\n time_pretransfer:%{time_pretransfer}\n time_connect:%{time_connect}\n time_starttransfer:%{time_starttransfer}\n time_total:%{time_total}\n speed_download:%{speed_download}\n " "https://jiebaiyou.com"
```

**输出结果：**

```
 http_code:000
 time_namelookup:0.002986
 time_redirect:0.000000
 time_pretransfer:0.000000
 time_connect:0.137609
 time_starttransfer:0.000000
 time_total:0.412923
 speed_download:0.000
```

#### HttpTime 数据说明

```
time_total 总时间，按秒计。精确到小数点后三位。
time_namelookup DNS解析时间,从请求开始到DNS解析完毕所用时间。
time_connect 连接时间,从开始到建立TCP连接完成所用时间,包括前边DNS解析时间，如果需要单纯的得到连接时间，用这个time_connect 时间减去前边time_namelookup时间。
time_appconnect 连接建立完成时间，如SSL/SSH等建立连接或者完成三次握手时间。
time_pretransfer 从开始到准备传输的时间。
time_redirect 重定向时间，包括到最后一次传输前的几次重定向的DNS解析，连接，预传输，传输时间。
time_starttransfer 开始传输时间。在发出请求之后，Web 服务器返回数据的第一个字节所用的时间
```