---
title: git清除所有历史记录
tags:
  - git
categories:
  - 技术分享
date: 2019-10-31 11:44:41
---

> 一起来清理掉git上所有的历史记录吧

 ##### 克隆项目
```
git clone git@code.aliyun.com:user/project.git 
```
<!-- more -->

 ##### 检出新分支

> 使用 git checkout --orphan new_branch ,基于当前分支创建一个独立的分支new_branch； 

```
cd project  
git checkout --orphan new_branch
```
##### 添加文件到暂存区
```
git add -A 
```
##### 添加提交记录
```
git commit -am "Initial commit." 
```
 ##### 删除当前分支
```
git branch -D master 
```
 ##### 重命名分支
```
git branch -m master 
```
 ##### 强制推送至远程分支
```
git push -f origin master 
```

##### 关联本地 master 到远程 master

```
git branch --set-upstream-to=origin/master
```