---
title: CentOS7安装Python3.8.0
date: 2019-10-26 16:08:20
tags: 
    - python
    - centos
categories:
    - 技术分享
---

> 在CentOS7上安装Python3.8.0版（编译安装方式）

<!-- more -->

#### 安装支持库

```
yum install zlib-devel bzip2-devel openssl-devel ncurses-devel -y
yum install sqlite-devel readline-devel tk-devel libffi-devel -y
yum install gcc make -y
```

#### 安装Python3
**下载安装包**
以3.8.0为例，需要下载其它版本请访问：<https://www.python.org/ftp/python>
```
wget --no-check-certificate https://www.python.org/ftp/python/3.8.0/Python-3.8.0.tgz
```
**解压安装包**

```
tar xzvf Python-3.8.0.tgz
cd Python-3.8.0
```
**编译安装**
指定安装目录

```
./configure --prefix=/usr/local/python3
```
*--enable-shared 启用动态连接库（可选），方便其他依赖库的正常安装。如：mysqlclient pyinstall
如果选择该参数，您可能需要设置动态库的环境变量,否则可能出现打不到库文件“libpython3.8m.so.1.0”的情况*

```
./configure --enable-shared
export LD_LIBRARY_PATH=/lib:/usr/lib:/usr/local/lib:/usr/local/python3/lib
sudo ldconfig /usr/local/python3/lib
```
*--enable-optimizations 优化选项（可选）执行完上一步后会提示执行以下的代码对Python解释器进行优化，据说性能有10%左右的提升，执行该代码后，会编译安装到 /usr/local/bin/python3.8 下，且不用添加软连接或环境变量。但安装目录也许并不是你想要的*
```
./configure --enable-optimizations
```
编译并安装
```
make && make install
```
**创建软链接**

```
ln -s /usr/local/python3/bin/python3 /usr/bin/python3
ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3
```

#### 验证安装
```
python3 -V
pip3 -V
```

