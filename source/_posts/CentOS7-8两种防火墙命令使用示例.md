---
title: CentOS7/8两种防火墙命令使用示例
tags:
  - linux
  - centos
categories:
  - 技术分享
date: 2019-11-07 13:40:10
---

转发修改自：[ https://www.toutiao.com/i6755704307607142920/ ](https://www.toutiao.com/i6755704307607142920/)

#### 一、CentOS7/8关闭防火墙的命令

##### 1:查看防火状态

```
systemctl status firewalld
service iptables status
```

##### 2:暂时关闭防火墙

```
systemctl stop firewalld
service iptables stop
```

##### 3:永久关闭防火墙

```
systemctl disable firewalld
chkconfig iptables off
```

##### 4:重启防火墙

```
systemctl enable firewalld
service iptables restart 
```

##### 5:永久关闭后重启

```
chkconfig iptables on
```

###  二、firewalld

> Centos7默认安装了firewalld，如果没有安装的话，可以使用以下命令进行安装。

```
yum install firewalld firewalld-config 
```

#### 1.启动防火墙

```
systemctl start firewalld 
```

#### 2.禁用防火墙

```
systemctl stop firewalld
```

#### 3.设置开机启动

```
systemctl enable firewalld
```

#### 4.停止并禁用开机启动

```
sytemctl disable firewalld
```

#### 5.重启防火墙

```
firewall-cmd --reload
```

#### 6.查看状态

```
systemctl status firewalld
或
firewall-cmd --state
```

#### 7.查看版本

```
firewall-cmd --version
```

#### 8.查看帮助

```
firewall-cmd --help
```

#### 9.查看区域信息

```
firewall-cmd --get-active-zones
```

#### 10.查看指定接口所属区域信息

```
firewall-cmd --get-zone-of-interface=eth0
```

#### 11.拒绝所有包

```
firewall-cmd --panic-on
```

#### 12.取消拒绝状态

```
firewall-cmd --panic-off
```

#### 13.查看是否拒绝

```
firewall-cmd --query-panic
```

#### 14.将接口添加到区域(默认接口都在public)

```
firewall-cmd --zone=public --add-interface=eth0
(永久生效再加上 --permanent 然后reload防火墙)
```

#### 15.设置默认接口区域

```
firewall-cmd --set-default-zone=public
(立即生效，无需重启)
```

#### 16.更新防火墙规则

```
firewall-cmd --reload
或
firewall-cmd --complete-reload
(两者的区别就是第一个无需断开连接，就是firewalld特性之一动态添加规则，第二个需要断开连接，类似重启服务)
```

#### 17.查看指定区域所有打开的端口

```
firewall-cmd --zone=public --list-ports
```

#### 18.在指定区域打开端口（记得重启防火墙）

```
firewall-cmd --zone=public --add-port=80/tcp
(永久生效再加上 --permanent)
```