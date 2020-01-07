---
title: CentOS 6/7 firewall-cmd与iptables使用
tags:
  - centos
  - firewall
  - iptables
categories:
  - 技术分享
date: 2020-01-07 12:47:25
---



### 一、CentOS7版本对防火墙进行加强,不再使用原来的iptables,启用firewalld

##### 1.firewalld的基本使用
```
启动：  `systemctl start firewalld`
查状态：`systemctl status firewalld `
停止：  `systemctl disable firewalld`
禁用：  `systemctl stop firewalld`
```
<!-- more -->
```
在开机时启用一个服务：`systemctl enable firewalld.service`
在开机时禁用一个服务：`systemctl disable firewalld.service`
查看服务是否开机启动：`systemctl is-enabled firewalld.service`
查看已启动的服务列表：`systemctl list-unit-files|grep enabled`
查看启动失败的服务列表：`systemctl *--failed`*
```
##### 2.配置firewalld-cmd
```
查看版本： `firewall-cmd *--version`*
查看帮助： `firewall-cmd *--help`*
显示状态： `firewall-cmd *--state`*
查看所有打开的端口： `firewall-cmd *--zone=public --list-ports`*
更新防火墙规则： `firewall-cmd *--reload`*
查看区域信息:  `firewall-cmd *--get-active-zones`*
查看指定接口所属区域： `firewall-cmd *--get-zone-of-interface=eth0`*
拒绝所有包：`firewall-cmd *--panic-on`*
取消拒绝状态： `firewall-cmd *--panic-off`*
查看是否拒绝： `firewall-cmd *--query-panic`*
```
##### 3.那怎么开启一个端口呢

添加

```
firewall-cmd --zone=public(作用域) --add-port=80/tcp(端口和访问类型) --permanent(永久生效)
firewall-cmd --zone=public --add-service=http --permanent
firewall-cmd --reload    # 重新载入，更新防火墙规则
firewall-cmd --zone= public --query-port=80/tcp  #查看
firewall-cmd --zone= public --remove-port=80/tcp --permanent  # 删除
firewall-cmd --list-services
firewall-cmd --get-services
firewall-cmd --add-service=
firewall-cmd --delete-service=
```

在每次修改端口和服务后/etc/firewalld/zones/public.xml文件就会被修改,所以也可以在文件中之间修改,然后重新加载

使用命令实际也是在修改文件，需要重新加载才能生效。

```
firewall-cmd --zone=public --query-port=80/tcp
firewall-cmd --zone=public --query-port=8080/tcp
firewall-cmd --zone=public --query-port=3306/tcp
firewall-cmd --zone=public --add-port=8080/tcp --permanent
firewall-cmd --zone=public --add-port=3306/tcp --permanent
firewall-cmd --zone=public --query-port=3306/tcp
firewall-cmd --zone=public --query-port=8080/tcp
firewall-cmd --reload # 重新加载后才能生效
firewall-cmd --zone=public --query-port=3306/tcp
firewall-cmd --zone=public --query-port=8080/tcp
```

##### 4.参数解释

```
–add-service #添加的服务
–zone #作用域
–add-port=80/tcp #添加端口，格式为：端口/通讯协议
–permanent #永久生效，没有此参数重启后失效
```

##### 5.详细使用

```firewall-cmd --permanent --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.0.4/24" service name="http" accept'*//设置某个ip访问某个服务*
firewall-cmd --permanent --add-rich-rule 'rule family=ipv4 source address=192.168.0.1/2 port port=80 protocol=tcp accept'*//设置某个ip访问某个端口*

firewall-cmd --permanent --remove-rich-rule 'rule family=ipv4 source address=192.168.0.1/2 port port=80 protocol=tcp accept'*//删除配置*

firewall-cmd --query-masquerade  # 检查是否允许伪装IP
firewall-cmd --add-masquerade    # 允许防火墙伪装IP
firewall-cmd --remove-masquerade # 禁止防火墙伪装IP
firewall-cmd --add-forward-port=port=80:proto=tcp:toport=8080# 将80端口的流量转发至8080
firewall-cmd --add-forward-port=proto=80:proto=tcp:toaddr=192.168.1.0.1# 将80端口的流量转发至192.168.0.1
firewall-cmd --add-forward-port=proto=80:proto=tcp:toaddr=192.168.0.1:toport=8080# 将80端口的流量转发至192.168.0.1的8080端口

firewall-cmd --permanent --zone=public --remove-rich-rule='rule family="ipv4" source address="192.168.0.4/24" service name="http" accept'*//删除配置*
```



### 二、CentOS7以下版本

##### 1.开放80，22，8080 端口

```
/sbin/iptables -I INPUT -p tcp *--dport 80 -j ACCEPT*
/sbin/iptables -I INPUT -p tcp *--dport 22 -j ACCEPT*
/sbin/iptables -I INPUT -p tcp *--dport 8080 -j ACCEPT*
```

##### 2.保存

```
/etc/rc.d/init.d/iptables save
```

##### 3.查看打开的端口

```
/etc/init.d/iptables status
```

##### 4.关闭防火墙 

1） 永久性生效，重启后不会复原

```
开启： chkconfig iptables on
关闭： chkconfig iptables off
```

2） 即时生效，重启后复原

开启： service iptables start

关闭： service iptables stop

```
firewall-cmd --state               #查看防火墙状态  
firewall-cmd --reload              #更新防火墙规则  
firewall-cmd --state               #查看防火墙状态  
firewall-cmd --reload              #重载防火墙规则  
firewall-cmd --list-ports          #查看所有打开的端口  
firewall-cmd --list-services       #查看所有允许的服务  
firewall-cmd --get-services        #获取所有支持的服务  
```

\#区域相关

```
firewall-cmd --list-all-zones                    #查看所有区域信息  
firewall-cmd --get-active-zones                  #查看活动区域信息  
firewall-cmd --set-default-zone=public           #设置public为默认区域  
firewall-cmd --get-default-zone                  #查看默认区域信息  
firewall-cmd --zone=public --add-interface=eth0  #将接口eth0加入区域public
```

\#接口相关

```
firewall-cmd --zone=public --remove-interface=eth0     #从区域public中删除接口eth0  
firewall-cmd --zone=default --change-interface=eth0    #修改接口eth0所属区域为default  
firewall-cmd --get-zone-of-interface=eth0              #查看接口eth0所属区域  
```

\#端口控制

```
#永久添加80端口例外(全局)
firewall-cmd --add-port=80/tcp --permanent 

#永久删除80端口例外(全局)
firewall-cmd --remove-port=80/tcp --permanent 

#永久增加65001-65010例外(全局)  
firewall-cmd --add-port=65001-65010/tcp --permanent 

#永久添加80端口例外(区域public)
firewall-cmd  --zone=public --add-port=80/tcp --permanent 

#永久删除80端口例外(区域public)
firewall-cmd  --zone=public --remove-port=80/tcp --permanent 

#永久增加65001-65010例外(区域public) 
firewall-cmd  --zone=public --add-port=65001-65010/tcp --permanent 

#查询端口是否开放
firewall-cmd --query-port=8080/tcp 

#开放80端口
firewall-cmd --permanent --add-port=80/tcp

#移除端口
firewall-cmd --permanent --remove-port=8080/tcp 

#重启防火墙(修改配置后要重启防火墙)
firewall-cmd --reload 
```

