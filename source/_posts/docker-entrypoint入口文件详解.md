---
title: docker entrypoint入口文件详解
tags:
  - docker entrypoint
categories:
  - 技术分享
date: 2020-01-13 09:06:46
---

在编写Dockerfile的时候，包含一个entrypoint配置，该配置的作用是在容器启动之前做一些初始化配置，或者一些自定义的配置等。通常是一个脚本，然后在脚本里配置相关预定义项。这篇文档就详细说一说entrypoint入口文件的编写技巧。

下面以mysql官方镜像中的entrypoint文件docker-entrypoint.sh为例，文件地址为：
[docker-entrypoint.sh](https://raw.githubusercontent.com/docker-library/mysql/607b2a65aa76adf495730b9f7e6f28f146a9f95f/5.7/docker-entrypoint.sh)

<!-- more -->

## set -e

你写的每个脚本都应该在文件开头加上`set -e`, 这句语句告诉bash如果任何语句的执行结果不是true则应该退出. 这样的好处是防止错误像滚雪球般变大导致一个致命的错误, 而这些错误本应该在之前就被处理掉. 如果要增加可读性, 可以使用`set -o errexit`, 它的作用与`set -e`相同

## set -o pipefail

设计用途同上, 就是希望在执行错误之后立即退出, 不要再向下执行了. 而 -o pipefail 的作用域是管道, 也就是说在 Linux 脚本中的管道, 如果前面的命令执行出了问题, 应该立即退出

## shopt -s nullglob

在使用 Linux 中的通配符时 `* ?`等 如果没有匹配到任何文件, 不会报 No such file or directory 而是将命令后面的参数去掉执行

## if [ "${1:0:1}" = '-' ]; then...

这是一个判断语句, 在官方文件中, 上一行已经给出了注释: `if command starts with an option, prepend mysqld`

这个判断语句是 `${1:0:1}` 意思是判断 `$1`(调用该脚本的第一个参数), 偏移量0(不偏移), 取一个字符(取字符串的长度)

如果判断出来调用这个脚本后面所跟的参数第一个字符是-中横线的话, 就认为后面的所有字符串都是 mysqld 的启动参数

上面的这个操作类似于 Python 的字符串切片

# set -- mysqld "$@"

在上面判断完第一个参数是-开头之后, 紧接着就执行了 set -- mysqld "$@" 这个命令. 使用了 set -- 的用法. set --会将他后面所有以空格区分的字符串, 按顺序分别存储到$1, $2, $3 变量中, 其中新的$@为set --后面的全部内容

举例来说: `bash docker-entrypoint.sh -f xxx.conf`

在这种情况下, `set -- mysqld "$@"` 中的 $@ 的值为 `-f xxx.conf`

当执行完 `set -- mysqld "$@"` 这条命令后:

- `$1=mysqld`
- `$2=-f`
- `$3=xxx.conf`
- `$@=mysqld -f xxx.conf`

可以看到, 当执行 docker-entrypoint.sh脚本的时候后面加了 -x形式的参数之后, $@的值发生的改变, 在原有$@值的基础之上, 在前面又预添加了 mysqld 命令

## exec "$@"

几乎在每个`docker-entrypoint.sh`脚本的最后一行, 执行的都是 `exec "$@"`命令

这个命令的意义在于你已经为你的镜像预想到了应该有的调用情况, 当实际使用镜像的人执行了你没有预料到的可执行命令时, 将会走到脚本的这最后一行, 去执行用户新的可执行命令

## 情况判断

上面直接说了脚本的最后一行, 在之前的脚本中, 需要充分的去考虑你自己的脚本可能会被调用的情况. 还是拿 MySQL 官方的 dockerfile 来说, 他判断以下情况:

- 开头是 `-` , 认为是参数的情况
- 开头是 mysqld, 且用户 id 为0 (root 用户) 的情况
- 开头是 mysqld 的情况
- 判断完自己应用的所有调用形态之后, 最后应该加上`exec "$@"` 命令兜底

## ${mysql[@]}

Shell 中的数组, 直接执行 `${mysql[@]}` 会把这个数组当做可执行程序来执行

```
mysql=( mysql --protocol=socket -uroot -hlocalhost --socket="${SOCKET}" )
echo ${mysql[1]}
-- output: mysql
echo ${mysql[2]}
--output: --protocol=socket
echo ${mysql[3]}
--output: -uroot
echo ${mysql[4]}
--output: -hlocalhost
echo ${mysql[@]}
--output: mysql --protocol=socket -uroot -hlocalhost --socket=
```

## exec gosu mysql "$BASH_SOURCE" "$@"

这里的 gosu 命令, 是 Linux 中 sudo 命令的轻量级”替代品”

gosu 是一个 golang 语言开发的工具, 用来取代 shell 中的 sudo 命令. su 和 sudo 命令有一些缺陷, 主要是会引起不确定的 TTY, 对信号量的转发也存在问题. 如果仅仅为了使用特定的用户运行程序, 使用 su 或 sudo 显得太重了, 为此 gosu 应运而生.

gosu 直接借用了 libcontainer 在容器中启动应用程序的原理, 使用 /etc/passwd 处理应用程序. gosu 首先找出指定的用户或用户组, 然后切换到该用户或用户组. 接下来, 使用 exec 启动应用程序. 到此为止, gosu 完成了它的工作, 不会参与到应用程序后面的声明周期中. 使用这种方式避免了 gosu 处理 TTY 和转发信号量的问题, 把这两个工作直接交给了应用程序去完成



引用自：https://www.cnblogs.com/breezey/p/8812197.html