---
title: git 删除错误提交的commit
tags:
  - git
categories:
  - 技术分享
date: 2020-01-13 14:05:44
---

### git 删除错误提交的commit

方法: 
```bash
  git reset --hard  [commit_id]
  
  git push origin HEAD --force
```
说明:

根据–soft –mixed –hard，会对working tree和index和HEAD进行重置:

<!-- more -->

* git reset –mixed：此为默认方式，不带任何参数的git reset，即时这种方式，它回退到某个版本，只保留源码，回退commit和index信息

* git reset –soft：回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可

* git reset –hard：彻底回退到某个版本，本地的源码也会变为上一个版本的内容



  HEAD 最近一个提交
  HEAD^ 上一次
  [commit_id]  每次commit的SHA1值. 可以用`git log` 看到,也可以在页面上commit标签页里找到.

### commit合并
引用自：[http://www.douban.com/note/318248317/](https://www.douban.com/note/318248317/)

有时commit太多，而且可能一个commit只是提交一个小bug，那么合并commit势在必行。
有两种方法：
一是在提交最后一个修改的commit使用参数，这时之前的一个commit将会合并到这个即将提交的commit中来：
```
git commit -a --amend -m "my message here"如果之前有一个提交，并且信息为:

git commit -a -m "my last commit message"
```
则这个commit message将不存在。但该commit的信息已经合并到"my message here"中了。

第二个是，如果你提交了最后的修改，这时可用：
```
git reset --soft HEAD^ #或HEAD^意为取消最后commit

git commit --amend
```
这将会把最后一个commit合并到前一个提交中去，例如（由上往下读）：
```
git add b.text

git commit -a -m "my message here"

git add a.text

git commit -a -m "my last commit message"
```
那么最后存在的将是"my last commit message"。也可后退n个，合并到前面第n+1个commit中去：
```
git reset --soft HEAD~n #后退到第n，我也不清楚具体含义。

git commit --amend [-m "new message"]
```
我觉得最方面的是调用reflog查看操作历史，找到具体的commit id，然后直接git reset --hard [commit_id]就回到你要的版本！