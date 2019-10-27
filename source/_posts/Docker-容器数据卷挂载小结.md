---
title: Docker 容器数据卷挂载小结
tags:
  - docker
categories:
  - 技术
date: 2019-10-26 20:34:03
---

> 为了更直观了解数据卷挂载的操作，做个实验一一验证数据卷挂载的各种情况。

<!-- more -->

**情况一、本地不存在文件挂载到容器存在文件**

首先是当本地不存在该文件，而容器内存在该文件的情况，尝试把不存在的文件挂载到存在该文件的容器中。以一个 Alpine 镜像为例，这里把一个修改后的 Alpine 镜像打了新标签，叫做 volume_test：

```
# 本地目录不存在 test 文件。
$ docker run --name=test -v ~/test.txt:/etc/hosts -d volume_test
0cba2e50229df7508c616bd456c4ab131f2fe1a88385c34f8a5876fbc577b176
docker: Error response from daemon: oci runtime error: rootfs_linux.go:53: mounting
"/var/lib/docker/devicemapper/mnt/6b83c07ebedcb828f34cac69eac5a85ce3a5f59e1e8688c8dae40198671d0ecb/rootfs/etc/hosts" to rootfs
"/var/lib/docker/devicemapper/mnt/6b83c07ebedcb828f34cac69eac5a85ce3a5f59e1e8688c8dae40198671d0ecb/rootfs" caused "not a directory".
# 启动容器失败。
```

**情况二、本地不存在文件夹挂载到容器存在文件夹**

然后是把本地不存在的文件夹挂载到容器内存在的文件夹，在 volume_test 镜像中存在一个 /srv 的文件夹，文件夹里面有一个 index.php 文件。

```
# 本地目录不存在 srv文件夹。
$ docker run --name=test -v ~/srv:/srv -d volume_test 
c71cf1cfa4932e3e398a7d6c4e2ae94f915b832f5506e374aedb19af4cb1ac62
# 启动正常，但是进入容器发现目录被清空。
$ docker exec -it test sh
/srv # ls
/srv # 
```

**情况三、宿主机存在文件挂载到容器不存在文件**

我们继续，假设宿主机存在文件，容器内不存在该文件：

```
# 本地目录存在 test.txt文件
$ docker run --name=test -v ~/test.txt:/srv/test.txt-d volume_test 
2d6853c10643a735ae3d7f3aaac8c6344f9c75170e531f613d08db7cdf484e54
# 容器内存在 /srv 文件夹，里面原本有一个 index.php 。
$ docker exec -it test sh
/srv # ls
index.php  test.txt
/srv # 
# 可以看到文件挂载成功。
```

**情况四、宿主机存在文件夹挂载到容器不存在文件夹**

接下来是宿主机存在文件夹，容器不存在该文件夹，宿主机的 test 文件夹里面存在一个 hello 文件：

```
$ docker run --name=test -v ~/test:/srv/test -d volume_test 
c935ffa0d9fc5e5ac8f213a51a878e71056472b0597d2e385a29e5c748012958
# 进入容器，查看是否存在 test 文件夹，以及文件夹里面是否有 hello 文件。
$ docker exec -it test sh
/srv # ls
index.php  test
/srv # cd test/
/srv/test # ls
hello
/srv/test # 
```

上面两个例子说明了，容器内部如果不存在文件，宿主机直接挂载。

**情况五、宿主机文件夹挂载到容器文件**

接下来假设宿主机存在 test 文件夹，而容器内部存在的是名为 test 文件，这样挂载会怎样呢？

```
$ docker run --name=test -v ~/test:/srv/test-d volume_test 
385bc78e5333460da11f04535da27a3fd226df218f95c970ff2dd5609b17f816
docker: Error response from daemon: oci runtime error: rootfs_linux.go:53: mounting 
"/var/lib/docker/devicemapper/mnt/fd5c42e844c3550d1a372ed939ed57f90dcacbd375dfed1bedfbb71ef6f3f185/rootfs/etc/hosts" to rootfs 
"/var/lib/docker/devicemapper/mnt/fd5c42e844c3550d1a372ed939ed57f90dcacbd375dfed1bedfbb71ef6f3f185/rootfs" caused "not a directory".
```

上面的情况不出意外是启动错误。

**情况六、同名文件夹挂载**

那么假设宿主机是文件夹，容器也是文件夹，两个文件夹里面内容不一样，宿主机内部有一个 hello 文件，容器的文件夹里面有一个 index.php ：

```
$ docker run --name=test -v ~/srv:/srv -d volume_test 
3aec30122bd7010c694e0ff8b77f7b7b6bb6f850c258786db125313060fad43b
$ docker exec-it test sh
/srv # ls
hello
/srv # 
# 可以看到，宿主机文件夹会覆盖容器内部的文件夹。
```

**情况七、同名文件挂载**

假设宿主机有一个 test.txt 文件，里面写着 Hello World，而容器里面也存在一个 test.txt 文件，里面写着 Hi World，现在挂载文件：

```
$ docker run --name=test -v ~/test.txt:/srv/test.txt -d volume_test 
047cbfe45b5bc868c864fe94f7a22643d52b644947f40260097dbb579de56c5c
$ docker exec -it test sh
/srv # cat /test
Hello World
/srv # 
# 宿主机会覆盖容器的文件。
```

**情况八、宿主机文件挂载到容器文件夹**

最后一种情况，宿主机存在文件 test.txt，而容器内部存在一个 test 的文件夹，现在把 文件挂载到文件夹中：

```
$ docker run --name=test -v ~/test.txt:/test -d volume_test 
59b5fd74a1e9e17aa2a6a9be7900b16c7dd4b3c424a4fa72a7671fa1c51bdf69
docker: Error response from daemon: oci runtime error: rootfs_linux.go:53: mounting 
"/var/lib/docker/devicemapper/mnt/b201054ed36a189b5abb599082d0b5bcbe31d07611a0985deefd79d1221447fd/rootfs/home" to rootfs 
"/var/lib/docker/devicemapper/mnt/b201054ed36a189b5abb599082d0b5bcbe31d07611a0985deefd79d1221447fd/rootfs" caused "not a directory".
# 启动失败。
```

**小结：**

| **宿主机文件** | **容器内文件** | **启动参数(加粗为不存在)** | **容器启动情况** |
| :- | :- | :- | :- |
| 不存在 | 文件 | -v **~/test.txt**:/etc/hosts | 启动错误 |
| 不存在 | 文件夹 | -v **~/srv**:/srv | 启动正常 |
| 文件 | 不存在 | -v ~/test.txt:**/srv/test.txt** | 启动正常 |
| 文件夹 | 不存在 | ~/test:**/srv/test** | 启动正常 |
| 文件夹 | 文件 | ~/test:/srv/test | 启动错误 |
| 文件夹 | 文件夹 | -v ~/srv:/srv | 启动正常 |
| 文件 | 文件 | -v ~/test.txt:/srv/test.txt | 启动正常 |
| 文件 | 文件夹 | -v ~/test.txt:/test | 启动错误 |