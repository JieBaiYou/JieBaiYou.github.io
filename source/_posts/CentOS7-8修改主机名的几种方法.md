---
title: CentOS7/8修改主机名的几种方法
tags:
  - linux
  - centos
categories:
  - 技术分享
date: 2019-11-07 16:49:42
---

在CentOS7/8中，有三种定义的主机名:


> * 静态的（Static hostname）
>   “静态”主机名也称为内核主机名，是系统在启动时从/etc/hostname自动初始化的主机名。
> * 瞬态的（Tansient hostname）
>   “瞬态”主机名是在系统运行时临时分配的主机名，例如，通过DHCP或mDNS服务器分配。
> * 灵活的（Pretty hostname）
>   “灵活”主机名也有人叫做“别名”主机名。
>   “灵活”主机名则允许使用自由形式（包括特殊/空白字符）的主机名，以展示给终端用户（如aibox@user01）。
> * “静态”主机名和“瞬态”主机名都遵从作为互联网域名同样的字符限制规则。

<!-- more -->

在CentOS 7/8 中，有个叫hostnamectl的命令行工具，它允许你查看或修改与主机名相关的配置。

#### 查看主机名:

#####  查看当前主机名，显示全部三种主机名

```
$ hostnamectl
or
$ hostnamectl status
Static hostname: aibox
      Icon name: computer-desktop
      Chassis: desktop
      Machine ID: 6394c3b5598044e6bd2da1862eafa27d
      Boot ID: cb326252e23d470a820b681f9b129bcd
Operating System: CentOS Linux 8 (Core)
      CPE OS Name: cpe:/o:centos:centos:8
      Kernel: Linux 4.18.0-80.11.2.el8_0.x86_64
      Architecture: x86-64
```

##### 只查看静态、瞬态或灵活主机名，分别使用--static，--transient或--pretty选项

```
$ hostnamectl --static
aibox
$ hostnamectl --transient
aibox
$ hostnamectl --pretty

//或者使用hostname查看到的是瞬态的（Tansient hostname）
$ hostname
aibox

//或者查看主机名配置文件，查看到的是静态的（Static hostname）
#cat /etc/hostname
aibox
查看当前Linux操作系统相关信息（内核版本号、硬件架构、主机名称和操作系统类型等）:

uname -a			//查看到的是瞬态的（Tansient hostname）
cat /etc/redhat-release		//查看操作系统环境
```

#### 修改主机名:

##### 方法1：临时有效

只能临时修改的主机名，当重启机器后，主机名称会变回来

```
hostname 主机名 
hostname aibox
```

##### 方法2：永久生效（不需重启--建议使用）

永久性的修改主机名称，重启后能保持修改后的。

```
hostnamectl set-hostname aibox
```

删除主机名

```
hostnamectl set-hostname ""
hostnamectl set-hostname "" --static
hostnamectl set-hostname "" --pretty
```

修改所有三个主机名：静态、瞬态和灵活主机名：

```
$ hostnamectl set-hostname aibox
$ hostnamectl set-hostname aibox --static
```

修改灵活主机名和瞬态主机名

```
$ hostnamectl set-hostname www.aibox.one --pretty
$ hostnamectl set-hostname aibox --transient
```

>   ​        就像上面示例的那样，在修改静态/瞬态主机名时，任何特殊字符或空白字符会被移除，而提供的参数中的任何大写字母会自动转化为小写。
>   ​       一旦修改了静态主机名，/etc/hostname 将被自动更新。然而，/etc/hosts 不会更新以保存所做的修改，所以你每次在修改主机名后一定要手动更新/etc/hosts，之后再重启CentOS。否则系统再启动时会很慢。

手动更新/etc/hosts

```
$ cat /etc/hosts 
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
127.0.0.1   aibox
```

修改后会立即生效

```
$ ping aibox
PING aibox (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_seq=1 ttl=64 time=0.045 ms
64 bytes from localhost (127.0.0.1): icmp_seq=2 ttl=64 time=0.024 ms
```



##### 方法3：永久生效（需要重启）

修改配置文件/etc/hostname来实现主机名的修改。把该文件中的aibox替换成自己想要的主机名重启即可。

```
$ cat /etc/hostname 
aibox
```

##### 方法4：永久生效

通过nmcli修改

```
nmcli general hostname aibox
```

还可以通过`nmtui`进入图形界面来修改主机名。
将光标通过键盘的上下键移动到“设定系统主机名”菜单处，按下回车键。
此时，屏幕出现“设定主机名”选项卡，输入需要设定的主机名，通过键盘方向键将光标移动到“确定”处，回车键确定即可完成主机名的修改。

>版权声明：本文修改自 https://blog.csdn.net/xuheng8600/article/details/79983927