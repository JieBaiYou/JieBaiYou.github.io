---
title: git 放弃本地修改，强制拉取更新
tags:
  - git
categories:
  - 技术分享
date: 2020-01-07 15:14:34
---

开发时，对于本地的项目中修改不做保存操作，可以用到git pull的强制覆盖，具体代码如下：
```bash
git fetch --all
git reset --hard origin/master
git pull //可以省略
```
git fetch 指令是下载远程仓库最新内容，不做合并
git reset 指令把HEAD指向master最新版本

