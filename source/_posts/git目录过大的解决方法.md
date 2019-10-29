---
title: .git目录过大的解决方法
tags:
  - git
categories:
  - 技术分享
date: 2019-10-29 15:33:32
---

> 最近在生产环境中发现.git目录特别大，导致clone项目要等很久，严重影响工作效率，分析发现是项目目录下.git文件夹有1.4G之大，上网学习一下总结如下：

#### 1.查看问题

首先查看一下项目目录的大小

```
du -d 1 -h
or
du -sh
```

会发现其中最大的是 .git/objects/pack 文件夹，这里面保存了所有的历史提交。比如你传了一个很大的视频文件提交后，当你删除这个文件再次提交的时候，这个文件在项目中并没有删除，只是删除了他的索引（可能因为多了一次提交反而会更大了一点）

>  git会记录你的每一次操作，这个是使用git中的很重要的概念 

#### 2.解决问题

解决这个问题有两种方法

* 使用git自带的`git rev-list` `git filter-branch` 命令查找并删除大文件
* 使用bfg工具（效率工具，强烈推荐）

##### 2.1  使用git命令方式

这种方式操作比较麻烦，建议有一些经验的朋友采用。但就一个字`慢`，在生产环境中我删除一个文件需要2个多小时，真是不能忍。如果想这样操作，推荐看一下下面两位的文章：

[寻找并删除Git记录中的大文件]( https://harttle.land/2016/03/22/purge-large-files-in-gitrepo.html )

[解决github项目体积过大的问题]( https://juejin.im/post/5ce5043c518825240245beb7 )

##### 2.2 使用bfg工具

**安装java环境并下载bfq工具**

```
# 下载java环境包
https://www.java.com/zh_CN/
# 下载bfq工具
https://repo1.maven.org/maven2/com/madgag/bfg/1.13.0/bfg-1.13.0.jar
```
**首先使用`--mirror`标记克隆git库副本**

```
git clone --mirror git://example.com/some-big-repo.git
```
**在确保已备份后，使用下面的命令清理git库中大于10M的文件**

```
java -jar bfg.jar --strip-blobs-bigger-than 10M some-big-repo.git
```
**或者删除某些敏感文件，比如秘钥之类**

```
bfg --delete-files id_{dsa,rsa}  my-repo.git
bfg --replace-text passwords.txt  my-repo.git
```
**执行完命令后，会提示所有符合清理条件的文件**

**按提示进行最后一步，推送**

```
cd some-big-repo.git
git reflog expire --expire=now --all && git gc --prune=now --aggressive
git push
```

其间给了一个提示，有点意思

![https://s3-cn-east-1.qiniucs.com/jiebaiyou-blog/python001.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=LW523SRVcyaYd2iOH9tpm7pW0k-XMECg4kT6rYFt%2F20191029%2Fcn-east-1%2Fs3%2Faws4_request&X-Amz-Date=20191029T083600Z&X-Amz-Expires=600&X-Amz-Signature=ef94f5db068c0e6e088c8172e8047d033a346188a69c882189f74b6d6884b2ad&X-Amz-SignedHeaders=host]()



> 附作者的github地址： https://github.com/rtyley/bfg-repo-cleaner 

