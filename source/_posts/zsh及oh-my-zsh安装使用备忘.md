---
title: zsh及oh-my-zsh安装使用备忘
tags:
  - zsh
  - shell
  - linux
categories:
  - 技术分享
date: 2019-11-07 16:52:44
---

> 记录下日常使用的zsh配置及插件

#### 安装 zsh

```bash
$ yum -y install zsh git 
```
<!-- more -->

#### 切换 shell

chsh 命令如果不存在则需 `$ yum install -y util-linux-user`
```bash
$ chsh -s /bin/zsh
```
#### 安装 oh-my-zsh  

```bash
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
$ sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
或
```bash
$ sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
或
```bash
$ curl -Lo install.sh https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh
$ sh install.sh
```
或
```bash
$ git clone https://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
$ cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
```

#### 安装自动补全 incr (两种可以二选一，各有特色)

```bash
$ wget -P ~/.oh-my-zsh/plugins/incr/ http://mimosa-pudica.net/src/incr-0.2.zsh 
$ chmod 777 ~/.oh-my-zsh/plugins/incr/incr-0.2.zsh  
$ echo "source ~/.oh-my-zsh/plugins/incr/incr-0.2.zsh" >> ~/.zshrc 
```
#### 安装自动补全命令zsh-autosuggestions 

```bash
$ git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions 
```
#### 安装自动跳转 autojump 

```bash
$ git clone git://github.com/joelthelion/autojump.git cd autojump && ./install.py   
$ echo "[[ -s /root/.autojump/etc/profile.d/autojump.sh ]] && source /root/.autojump/etc/profile.d/autojump.sh" >> ~/.zshrc 
$ echo "autoload -U compinit && compinit -u" >> ~/.zshrc  
```
#### 安装命令行高亮显示 zsh-syntax-highlighting 

```bash
$ git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting 
```

#### 修改主题 theme 

```bash
$ sed -i 's/ZSH_THEME="robbyrussell"/ZSH_THEME="ys"/' ~/.zshrc 
```
#### 添加插件支持 plugins 

```bash
$ sed -i 's/(git)$/(git extract zsh-autosuggestions zsh-syntax-highlighting)/' ~/.zshrc 
$ sed -i 's/(git)$/(git extract autojump zsh-autosuggestions zsh-syntax-highlighting)/' ~/.zshrc 
# 实际上是修改配置文件plugins字段，添加需要使用的插件
plugins=(git wd web-search history history-substring-search)
```
#### 关闭自动更新

```bash
$ sed -i 's/# DISABLE_AUTO_UPDATE="true"/DISABLE_AUTO_UPDATE="true"/g' ~/.zshrc 
```
#### 加载 zshrc 

```bash
$ source ~/.zshrc 
```