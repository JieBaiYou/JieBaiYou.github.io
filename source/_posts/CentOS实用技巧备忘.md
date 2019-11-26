---
title: CentOS实用技巧备忘
tags:
  - linux
  - centos
categories:
  - 技术分享
date: 2019-10-30 13:48:05
---





#### 用命令反查安装包
```
yum whatprovides command
```
#### 关闭SELinux
```
# 临时关闭
setenforce 0 
# 永久关闭
sed -i 's/SELINUX=enforcing/SElINUX=disabled/g' /etc/selinux/config 
```
<!-- more -->
#### 开启bash命令补全
> 如nmcli、yum等命令，参数比较多，开启后按`tab`即可补全
```
yum install bash-completion -y
```
#### 创建自解压包
> makeself.sh是一个Shell脚本，可从目录生成自解压文件。生成的文件显示为Shell脚本（一般后缀为.run），可以直接启动。归档文件将自身解压缩到一个临时目录，并执行一个可选的任意命令（例如，安装脚本）。这与Windows中使用WinZip自解压器生成的存档非常相似。
> 下载地址：https://github.com/megastep/makeself

```
./makeself.sh [args] archive_dir file_name label startup_script [script_args]
./makeself.sh --gzip updata updata.bin CentOS_7_Updata ./install.sh
# 还有很多可选参数请查看作者github地址

```
#### 使用downloadonly下载安装包及其依赖

> 主要解决某些内网的主机安装软件的问题。可以先在有互联网的主机上下载包，再到内网中安装

```
# 在有网络的主机上下载一个安装包
yum install -y --downloadonly --downloaddir=./ lrzsz
# 在没有网络（内网）的主机上安装包
rpm -ivh lrzsz-0.12.20-43.el8.x86_64.rpm -y
```

#### 关闭并禁用系统服务

```
systemctl stop nginx.service && systemctl disable nginx.service
systemctl stop firewalld.service && systemctl disable firewalld.service
```

#### tar压缩包简单加密

```
#加密压缩
tar -czvf - file | openssl des3 -salt -k password -out /path/to/file.tar.gz
#解密解压
openssl des3 -d -k password -salt -in /path/to/file.tar.gz | tar xzf -
```

#### CentOS 7 替换阿里源

```
cd /etc/yum.repos.d/
mv CentOS-Base.repo CentOS-Base.repo.bak
wget http://mirrors.aliyun.com/repo/Centos-7.repo
yum clean all
yum makecache
yum update
```

#### CentOS 安装epel扩展源

> EPEL (Extra Packages for Enterprise Linux)是基于Fedora的一个项目，用以创建、维护以及管理针对企业版 Linux 的一个高质量附加软件包集，面向的对象包括但不限于 红帽企业版 Linux (RHEL)、 CentOS、Scientific Linux (SL)、Oracle Linux (OL) 。

> EPEL 的软件包通常不会与企业版 Linux 官方源中的软件包发生冲突，或者互相替换文件。EPEL 项目与 Fedora 基本一致，包含完整的构建系统、升级管理器、镜像管理器等等。

> 官网：[ https://fedoraproject.org/wiki/EPEL ](https://fedoraproject.org/wiki/EPEL)

```
yum install -y epel-release
yum makecache
```

#### 查找进程 PPID（查找并杀死僵尸进程）
```
ps -p 29327 -o ppid 
ps -p 29327 -o ppid=

ps -A -o stat,ppid,pid,cmd | grep -e '^[Zz]' | awk '{ print $2 }' | xargs kill 9
```

