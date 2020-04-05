---
title: MAC清理DOCKER容器日志
tags:
  - docker
  - mac
categories:
  - 技术分享
date: 2020-04-06 00:46:16
---

**第一步. 取容器的日志路径**

```
docker inspect fr2 | grep LogPath
"LogPath": "/var/lib/docker/containers/b03099af97b289629f00e677ab58a740699b7c982b470d589ecdcbf52cd4e674/b03099af97b289629f00e677ab58a740699b7c982b470d589ecdcbf52cd4e674-json.log",
```
**第二步.进入VM**

```
screen ~/Library/Containers/com.docker.docker/Data/vms/0/tty
```
**第三步.清空日志文件**

```
true > /var/lib/docker/containers/b03099af97b289629f00e677ab58a740699b7c982b470d589ecdcbf52cd4e674/b03099af97b289629f00e677ab58a740699b7c982b470d589ecdcbf52cd4e674-json.log
```

