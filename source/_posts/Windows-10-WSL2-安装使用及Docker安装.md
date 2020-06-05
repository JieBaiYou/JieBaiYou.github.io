---
title: Windows 10 WSL2 安装使用及Docker安装
tags:
  - wls
  - docker
categories:
  - 技术分享
date: 2020-06-05 10:06:45
---

## 1. WSL2 子系统安装使用

> WLS2 需要 Windows 10 Version 2004 Build 19041 或更高版本，使用 Windows Update 自动更新到2004版本。如果没有检测到更新，可以去微软官网下载[ 易升 ](https://software-download.microsoft.com/download/pr/MediaCreationTool2004.exe)工具并安装运行。工具会检测到更新并安装。
>
> 如果需要使用 Docker for Windows，需要下载[ WLS Updata ](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)补丁并安装。

#### 1.1 启用Linux子系统

打开 `控制面板` -> `程序` -> `启用或关闭Windows功能`。找到 `适用于Linux的Windows子系统` 和 `虚拟机平台`，勾选这两项之后确定，并重新启动计算机。

也可以通过命令行启用，以`管理员身份`打开 PowerShell，输入如下命令，并 `重启电脑`

```
# 开启 虚拟机平台 可选组件
Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform
# 开启 适用于Linux的Windows子系统 可选组件
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

<!--more-->

#### 1.2 安装Ubuntu 20.04 

打开 `Microsoft Store `搜索  `Linux`，选择 `Ubuntu 20.04` 并点击 `安装`：

安装过程中，请设置用户名及密码，如果需要使用root用户，则需要执行以下命令

```bash
$sudo passwd root
New password:
Retype new password:
passwd: password updated successfully
```

<!--more-->

为了更方便的使用子系统，推荐安装 Terminal 

#### 1.3 安装Windows Terminal 

打开 `Microsoft Store`， 搜索 `Windows Terminal`，选择 `安装`.

#### 1.4 WSL常用命令

##### 1.4.1 查看现有的WSL

```bash
wsl -l -v # 或 wsl --list --verbose 
# 输出结果
  NAME                   STATE           VERSION
* docker-desktop         Stopped         2
  Ubuntu-20.04           Stopped         2
```

#####  1.4.2 WSL1转化为WSL2

```bash
wsl --set-version $已经使用的WSL的名字 2
# 输出结果
正在进行转换，这可能需要几分钟时间...
有关与 WSL 2 的主要区别的信息，请访问 https://aka.ms/wsl2
转换完成。
```

##### 1.4.3 将WSL2设置为默认

```bash
wsl --set-default-version 2
# 输出结果
有关与 WSL 2 的主要区别的信息，请访问 https://aka.ms/wsl2
```

##### 1.4.4 立即终止所有正在运行的发行版和WSL2虚拟机

```
wsl --shutdown
```

##### 1.4.5 获取宿主Windows的IP

```bash
# WSL2使用的是虚拟机技术和WSL第一版本不一样，和宿主windows不在同一个网络内
# 获取宿主windows的ip
export windows_host=`ipconfig.exe | grep -n4 WSL  | tail -n 1 | awk -F":" '{ print $2 }'  | sed 's/^[ \r\n\t]*//;s/[ \r\n\t]*$//'`
```

##### 1.4.6 全面设置WSL内的代理

假设你的宿主Windows代理端口是1080

```bash
export ALL_PROXY=socks5://$windows_host:1080
export HTTP_PROXY=$ALL_PROXY
export http_proxy=$ALL_PROXY
export HTTPS_PROXY=$ALL_PROXY
export https_proxy=$ALL_PROXY
```

##### 1.4.7 设置git的代理

```bash
if [ "`git config --global --get proxy.https`" != "socks5://$windows_host:1080" ]; then
    git config --global proxy.https socks5://$windows_host:1080
fi
```

##### 1.4.8 设置docker内的代理

在/etc/default/docker有

```bash
export http_proxy="socks5://[windows_host]:1080"
export https_proxy="socks5://[windows_host]:1080"
sudo sed -i -E "s#socks5.*?1080#socks5://$windows_host:1080#" /etc/default/docker
```

##### 1.4.9 设置WSL内的DNS, 默认是系统自己创建的

```bash
sudo bash -c 'echo -e "nameserver 114.114.114.114\nnameserver 8.8.8.8\nnameserver 8.8.4.4" > /etc/resolv.conf'
```

##### 1.4.10 打通Window与Ubuntu网络

以 `管理员身份` 打开 PowerShell

```bash
# x.x.x.x 为 Ubuntu IP，请替换
netsh interface portproxy add v4tov4 listenport=80 listenaddress=0.0.0.0 connectport=80 connectaddress=x.x.x.x
```

##### 1.4.11 WSL、WSL2 导出迁移

WSL2虚拟机长时间使用占用空间过大导致C盘爆满，把他迁移到E盘

```
# EXPORT 导出
wsl --export Alpine ./Alpine.tar
# IMPORT 导入
mkdir e:\newAlpine
wsl --import Alpine e:\newAlpine Alpine.tar
explorer.exe e:\newAlpine
```







## 2. Docker 安装

#### 2.1 更新操作系统 

```bash
sudo apt-get update
sudo apt-get upgrade -y
sudo apt autoremove
```

#### 2.2 安装常用软件

```
sudo apt-get -y install git curl wget unzip lrzsz net-tools
```

#### 2.2 安装Docker前准备

##### 2.2.1 卸载旧版本

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

##### 2.2.2 安装依赖使apt能够使用基于https的仓库

```bash
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common -y
```

##### 2.2.3 添加docker的离线 gpg key

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

##### 2.2.4 验证key的信息

```bash
sudo apt-key fingerprint 0EBFCD88
    
pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid   [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]
```

##### 2.2.5 设置docker各版本的安装源（此处是 stable 版本）

To add the **nightly** or **test** repository, add the word `nightly` or `test` (or both) after the word `stable` in the commands below.

```bash
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

#### 2.3 开始安装 docker

##### 2.3.1 更新软件目录

```bash
sudo apt-get update
```

##### 2.3.2 安装最新版本的 docker

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io -y
```

##### 2.3.3 启动测试

```bash
# 启动 docker 守护进程
sudo service docker start
# 运行测试
sudo docker run hello-world
```

#### 2.4 免sudo使用docker命令

如果在一台新机器上面安装docker时候发现在安装完成之后爆出类似于下面的错误

```bash
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.26/images/json: dial unix /var/run/docker.sock: connect: permission denied
```

报错显示权限不够、那么如何解决这个问题呢？

官方文档给出了解决方案，那就是将你的用户添加到 `docker` 这个用户组里面即可

##### 2.4.1 添加docker group组

```bash
sudo groupadd docker
```

#####　2.4.2 将用户加入该group内

退出并重新登录就生效啦。

```bash
sudo gpasswd -a ${USER} docker
```

##### 2.4.3 重启docker服务

```bash
sudo service docker restart
```

##### 2.4.4 切换当前会话到新group或者重启会话

```bash
newgrp - docker
```

现在配置就完成了、可以免 sudo 使用，docker 命令了。

