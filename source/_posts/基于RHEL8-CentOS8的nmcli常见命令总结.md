---
title: 基于RHEL8/CentOS8的nmcli常见命令总结
tags:
  - centos
  - linux
categories:
  - 技术分享
date: 2019-10-29 18:40:16
---

转载修改自：https://blog.51cto.com/cyent/2332161
首先看一下帮助，了解一下命令的简写
```
[root@redhat ~]# nmcli -h
用法：nmcli [选项] 对象 

选项：
  -o[verview]                    概览模式（隐藏默认值）
  -t[erse]                       简洁输出
  -p[retty]                      整齐输出
  -m[ode] tabular|multiline      输出模式
  -c[olors] auto|yes|no          是否在输出中使用颜色
  -f[ields] |all|common          指定要输出的字段
  -g[et-values] |all|common      -m tabular -t -f 的快捷方式
  -e[scape] yes|no               在值中转义列分隔符
  -a[sk]                         询问缺少的参数
  -s[how-secrets]                允许显示密码
  -w[ait]                        为完成的操作设置超时等待时间
  -v[ersion]                     显示程序版本
  -h[elp]                        输出此帮助

对象：
  g[eneral]       网络管理器（NetworkManager）的常规状态和操作
  n[etworking]    整体联网控制
  r[adio]         无线网络管理器开关
  c[onnection]    网络管理器的连接
  d[evice]        由网络管理器管理的设备
  a[gent]         网络管理器的密钥（secret）代理或 polkit 代理
  m[onitor]       监视网络管理器更改
```
<!-- more -->

> 以下提及的ifcfg均指代/etc/sysconfig/network-scripts/ifcfg-ethX及/etc/sysconfig/network-scripts/route-ethX

#### 查看ip（类似于ifconfig、ip addr）
nmcli

#### 创建connection，配置静态ip（等同于配置ifcfg，其中BOOTPROTO=none，并ifup启动）
nmcli connection add type ethernet con-name ethX ifname ethX ipv4.addr 192.168.1.100/24 ipv4.gateway 192.168.1.1 ipv4.method manual

#### 创建connection，配置动态ip（等同于配置ifcfg，其中BOOTPROTO=dhcp，并ifup启动）
nmcli connection add type ethernet con-name ethX ifname ethX ipv4.method auto

#### 修改ip（非交互式）
nmcli connection modify ethX ipv4.addr '192.168.1.200/24'
nmcli connection up ethX

#### 修改ip（交互式）
nmcli connection edit ethX
nmcli> goto ipv4.addresses
nmcli ipv4.addresses> change
Edit 'addresses' value: 192.168.1.200/24
Do you also want to set 'ipv4.method' to 'manual'? [yes]: yes
nmcli ipv4> save
nmcli ipv4> activate
nmcli ipv4> quit

#### 启用connection（相当于ifup）
nmcli connection up ethX

#### 停止connection（相当于ifdown）
nmcli connection down

#### 删除connection（类似于ifdown并删除ifcfg）
nmcli connection delete ethX

#### 查看connection列表
nmcli connection show
nmcli connection

#### 查看connection详细信息
nmcli connection show ethX

#### 重载所有ifcfg或route到connection（不会立即生效）
nmcli connection reload

#### 重载指定ifcfg或route到connection（不会立即生效）
nmcli connection load /etc/sysconfig/network-scripts/ifcfg-ethX
nmcli connection load /etc/sysconfig/network-scripts/route-ethX

#### 立即生效connection，有3种方法
nmcli connection up ethX
nmcli device reapply ethX
nmcli device connect ethX

#### 查看device列表
nmcli device

#### 查看所有device详细信息
nmcli device show

#### 查看指定device的详细信息
nmcli device show ethX

#### 激活网卡
nmcli device connect ethX

#### 关闭无线网络（NM默认启用无线网络）
nmcli radio all off

#### 查看NM纳管状态
nmcli networking

#### 开启NM纳管
nmcli networking on

#### 关闭NM纳管（谨慎执行）
nmcli networking off

#### 监听事件
nmcli monitor

#### 查看NM本身状态
nmcli

#### 检测NM是否在线可用
nm-online