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
  git reset --hard  \<commit_id\>
  git push origin HEAD --force
```
说明:
<!-- more -->
根据–soft –mixed –hard，会对working tree和index和HEAD进行重置:
* git reset –mixed：此为默认方式，不带任何参数的git reset，即时这种方式，它回退到某个版本，只保留源码，回退commit和index信息

* git reset –soft：回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可

* git reset –hard：彻底回退到某个版本，本地的源码也会变为上一个版本的内容



  HEAD 最近一个提交
  HEAD^ 上一次
  \<commit_id\>  每次commit的SHA1值. 可以用`git log` 看到,也可以在页面上commit标签页里找到.

commit合并:
[http://www.douban.com/note/318248317/](https://www.douban.com/note/318248317/)