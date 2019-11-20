---
title: CentOS 7/8 中设置时间、时区、时间同步
tags:
  - centos
  - timezone
  - ntp
categories:
  - 技术分享
date: 2019-11-20 15:33:37
---

> 在安装设置服务器时，经常需要同步或设置Linux系统的时间，本文将通过终端命令`timedatectl`设置`date`、`time`、`timezone`和`synchronize time`，以此来管理服务器时间。
> 在CentOS 8 中，使用`chrony`来实现时间同步，如果最小化安装系统，需要使用下面命令安装`chrony`服务。
> `yum install -y chrony`

<!--more-->

### 查找和设置Linux本地时区

##### 1、要显示系统的当前时间和日期，使用命令行中的`timedatectl`命令，如下：

```
# timedatectl
OR
# timedatectl status
```

```
# timedatectl
               Local time: 三 2019-11-20 15:42:13 CST
           Universal time: 三 2019-11-20 07:42:13 UTC
                 RTC time: 三 2019-11-20 15:42:13
                Time zone: Asia/Shanghai (CST, +0800)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
```

　　在上面的示例中，RTC time是指硬件时钟的时间、Time zone为时区。

##### 2、要查看所有可用的时区，运行以下命令：

```
# timedatectl list-timezones
```

#####  3、要根据地理位置找到本地的时区，运行以下命令：

```
# timedatectl list-timezones | egrep -o ‘’Asia/B.*”
# timedatectl list-timezones | egrep -o “America/N.*”
```

##### 4、要设置本地时区，使用set-timezone选项：

​      中国上海的时区：

```
# timedatectl set-timezone "Asia/Shanghai"
```

​      也可以使用协调世界时，即UTC (Universal Time Coordinated) 

```
# timedatectl set-timezone UTC
```



### 设置Linux时间和日期

​      可以使用timedatectl命令，设置系统上的日期和时间，如下所示：

##### 5、同时设置日期和时间：

```
# timedatectl set-time '16:10:40 2015-11-20'
```
##### 6、只设置时间可以使用set-time选项，按HH:MM:SS（时，分，秒）的时间格式。

```
# timedatectl set-time 15:58:30
```

##### 7、只设置日期可以使用set-time选项，按YY:MM:DD（年，月，日）的日期格式。

```
# timedatectl set-time 20151120
```



### 设置Linux硬件时钟

##### 8、要设置硬件时钟以协调世界时，UTC，可以使用 set-local-rtc boolean-value选项，如下所示：

​      首先确定你的硬件时钟是否设置为本地时区：

```
# timedatectl | grep local
  RTC in local TZ: no
```

​      将你的硬件时钟设置为本地时区：

```
# timedatectl set-local-rtc 1
```

​      将你的硬件时钟设置为协调世界时（UTC）：

```
# timedatectl set-local-rtc 0
```



### 设置同步远程NTP服务器时间

​      NTP即Network Time Protocol（网络时间协议），是一个互联网协议，用于同步计算机之间的系统时钟。`timedatectl`程序可以使用NTP服务器自动同步Linux系统时钟。

​      注意，你必须在系统上安装NTP以实现与NTP服务器的自动时间同步，并确保`chrony`服务正常运行。

​      要开启NTP时间同步，在终端键入以下命令：

```
# timedatectl set-ntp true
```
​      要禁用NTP时间同步，在终端键入以下命令。

```
# timedatectl set-ntp false
```

​      同步的NTP服务器信息存储在 /etc/chrony.conf 文件中，可以根据自己的需要自行添加

```
# cat /etc/chrony.conf 
server 0.centos.pool.ntp.org iburst
server 1.centos.pool.ntp.org iburst
server 2.centos.pool.ntp.org iburst
server 3.centos.pool.ntp.org iburst
......
```

