---
title: Linux Shell取之前的日期及时间
tags:
  - linux
  - shell
categories:
  - 技术分享
date: 2019-11-13 17:43:28
---

>  写脚本时经常用到取之前日期的情况，解决方法总结如下：

<!-- more -->

```
#!/bin/bash 
#一月前  
historyTime=$(date "+%Y-%m-%d %H" -d '1 month ago')  
echo ${historyTime}  

#一个月前时间戳
historyTimeStamp=$(date -d "$historyTime" +%s)  
echo ${historyTimeStamp}  

#一周前  
historyWeek=$(date "+%Y-%m-%d %H" -d '7 day ago') 
echo ${historyWeek} 

#本月一月一日  
date_this_month=`date +%Y%m01`  
echo $data_this_month 

#一天前  
date_today=`date -d '1 day ago' +%Y%m%d`  
echo $date_today 

#一小时前  
one_Hour=$(date "+%Y-%m-%d %H" -d '-1 hours') 
echo $one_Hour 

```

