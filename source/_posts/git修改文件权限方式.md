---
title: git修改文件权限方式
tags:
  - git
categories:
  - 技术分享
date: 2020-01-13 11:05:31
---

### 查看文件权限

```
$ git ls-tree HEAD
100644 blob ce71d57cfdcddcf2f8aa19fecc46ba05c85aaed1    install.sh
```

### 修改权限

```
$ git update-index --chmod=+x install.sh
```

### 提交修改

```
$ git commit -m "修改install.sh文件权限"
[master c26ebb5] 修改install.sh文件权限
 1 file changed, 0 insertions(+), 0 deletions(-)
 mode change 100644 => 100755 install.sh
 
$ git push    
```

