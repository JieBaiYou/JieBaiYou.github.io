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
git clone git@code.aliyun.com:liziyu/ds.git 
```
 ##### 检出分支
```
cd ds  
git checkout --orphan latest_branch 
```
 ##### 初始化提交
```
git add -A 
git commit -am "Initial commit." 
```
 ##### 删除分支
```
git branch -D master 
```
 ##### 重命名分支为master
```
git branch -m master 
```
 ##### 强制推送
```
git push -f origin master 
```

