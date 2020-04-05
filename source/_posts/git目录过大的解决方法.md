---
title: .git目录过大的解决方法
tags:
  - git
categories:
  - 技术分享
date: 2019-10-29 15:33:32
---

> 最近在生产环境中发现.git目录特别大，导致clone项目要等很久，严重影响工作效率，分析发现是项目目录下.git文件夹有1.4G之大，上网学习后总结如下：



<!-- more -->

### 查看问题

首先查看一下项目目录的大小

```bash
du -d 1 -h
or
du -sh
```

会发现其中最大的是 .git/objects/pack 文件夹，这里面保存了所有的历史提交。比如你传了一个很大的视频文件提交后，当你删除这个文件再次提交的时候，这个文件在项目中并没有删除，只是删除了他的索引（可能因为多了一次提交反而会更大了一点）

>  git会记录你的每一次操作，这个是使用git中的很重要的概念 

### 解决问题

解决这个问题有两种方法

* 使用git自带的`git rev-list` `git filter-branch` 命令查找并删除大文件
* 使用bfg工具（效率工具，强烈推荐，本文采用此种方式）

#### 使用git命令方式

这种方式操作比较麻烦，建议有一些经验的朋友采用。但就一个字`慢`，在生产环境中我删除一个文件需要2个多小时，真是不能忍。如果想这样操作，推荐看看下面两位的文章：

[寻找并删除Git记录中的大文件]( https://harttle.land/2016/03/22/purge-large-files-in-gitrepo.html )

[解决github项目体积过大的问题]( https://juejin.im/post/5ce5043c518825240245beb7 )

#### 使用bfg工具

##### 首先安装java环境并下载bfq工具

```
# 下载java环境包
https://www.java.com/zh_CN/
# 下载bfq工具
https://repo1.maven.org/maven2/com/madgag/bfg/1.13.0/bfg-1.13.0.jar
```
##### 然后使用`--mirror`标记克隆git库副本

```
git clone --mirror git://example.com/some-big-repo.git
```
##### 在确保已备份后，使用下面的命令清理git库中大于10M的文件

```
java -jar bfg-1.13.0.jar --strip-blobs-bigger-than 10M some-big-repo.git
```
##### 或者删除某些敏感文件，比如秘钥之类

```
java -jar bfg-1.13.0.jar --delete-files id_{dsa,rsa}  my-repo.git
java -jar bfg-1.13.0.jar --replace-text passwords.txt  my-repo.git
```
执行完命令后，会提示所有符合清理条件的文件

##### 按提示进行最后一步，推送

```
cd some-big-repo.git
git reflog expire --expire=now --all && git gc --prune=now --aggressive
git push
```

其间给了一个提示，有点意思

![](http://q02nuv786.bkt.clouddn.com/let.png)



> 附作者的github地址： https://github.com/rtyley/bfg-repo-cleaner 



#### 使用git自带命令

首先，我们需要通过命令找出我们Git提交记录中的大文件。
我们需要删除的视频文件，确定是最大的文件，所以我们只需找出排名前 1 的 pack 记录即可，不过这里我们为了演示取排名前3的，执行以下命令：

```
git verify-pack -v .git/objects/pack/pack-*.idx | sort -k 3 -g | tail -3
```
上面的命令执行后出现如下信息：
```
16779d71545f8b76faf02afffe554aac... blob   1102745 1102346 8459682
68f450adbce465995f52796f05956f29... blob   2081189 2081811 5111192
d0781e7d125599010f4885fa9518cd44... blob   278367052 278045657 10601748
```
最后一条就是最大的一条记录，`d0781e7d125599010f4885fa9518cd44...`是它的 id。
找出该记录对应的文件，执行以下命令：

```
git rev-list --objects --all | grep d0781e7d125599010f4885fa95802a1d7018cd44
```
上面的命令执行后出现如下信息：
```
d0781e7d125599010f4885fa95802a1d7018cd44  app/src/assets/img/FS.mp4
```
这个文件就是我们不小心提交上去的视频文件，它占了有 200 多 M 的空间。

既然文件找到了，那么得将该文件从历史记录中删除，执行以下命令:
```
git log --pretty=oneline --branches --  app/src/assets/img/FS.mp4
```
上面的命令执行后只是从历史记录中移除，还没有完全删除它，我们需要重写所有 commit，将该文件从 Git 历史中完全删除：
```
git filter-branch --index-filter 'git rm --cached --ignore-unmatch  app/src/assets/img/FS.mp4 -- --all
```
上面的命令执行后，此时历史记录中已经没有该文件了，此时是真正删除了它。
不过我们运行 filter-branch 产生的日志还是会对该文件有引用，所以我们还需要运行以下几条命令，把该文件的引用完全删除：
```
rm -Rf .git/refs/original
rm -Rf .git/logs/
git gc
git prune
```
现在我们再看 .git 文件的大小明显变小了，少了那个大文件，说明我们之前误提交的大文件已经删除了。
最后一步就是 push 代码了，不过就是需要强制 push：
```
git push --force
```
大功告成，以上就是删除 Git 历史记录（已提交）中大文件的步骤。

原文链接：https://blog.csdn.net/weixin_45115705/article/details/90604963



#### 命令示例：

```
# 找到最大的前10个文件
git rev-list --all | xargs -rL1 git ls-tree -r --long | sort -uk3 | sort -rnk4 | head -10

git filter-branch --tree-filter "rm -f docker/docker-images.tar" -- --all
git log --pretty=oneline --branches -- docker/docker-images.tar
git filter-branch --index-filter 'git rm --cached --ignore-unmatch docker/docker-images.tar'  -- --all
rm -Rf .git/refs/original
rm -Rf .git/logs/
git gc
git prune
git push --force
```