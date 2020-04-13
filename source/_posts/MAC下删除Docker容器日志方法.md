---
title: MAC下删除Docker容器日志方法
tags:
  - mac
  - docker
categories:
  - 技术分享
date: 2020-04-13 13:00:00
---

**第一步. 取容器的日志路径**

```bash
docker inspect fr2 | grep LogPath
"LogPath": "/var/lib/docker/containers/b03099af97b289629f00e677ab58a7
40699b7c982b470d589ecdcbf52cd4e674/b03099af97b289629f00e677ab58a74069
9b7c982b470d589ecdcbf52cd4e674-json.log",
```

**第二步.进入VM**

```bash
screen ~/Library/Containers/com.docker.docker/Data/vms/0/tty
```

**第三步.清空日志文件**

```bash
true > /var/lib/docker/containers/b03099af97b289629f00e677ab58a740699b7c982b47
0d589ecdcbf52cd4e674/b03099af97b289629f00e677ab58a740699b7c982b470d589ecdcbf52
cd4e674-json.log
```

