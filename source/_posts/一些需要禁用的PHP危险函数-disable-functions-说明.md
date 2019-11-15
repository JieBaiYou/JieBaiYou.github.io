---
title: 一些需要禁用的PHP危险函数(disable_functions)说明
tags:
  - php
categories:
  - 技术分享
date: 2019-11-14 17:58:41
---
>禁用方法如下：
打开php.ini文件，如：/usr/local/php/etc/php.ini
查找到 disable_functions ，添加需禁用的函数名，如下：
```
disable_functions = exec,scandir,shell_exec,phpinfo,eval,passthru,system,chroot,chgrp,chown,
        proc_open,proc_get_status,ini_alter,ini_restore,dl,openlog,syslog,readlink,
        symlink,popepassthru,stream_socket_server,fsocket
```
<!-- more -->

**phpinfo()**
功能描述：输出 PHP 环境信息以及相关的模块、WEB 环境等信息。
危险等级：中

**exec()**
功能描述：允许执行一个外部程序（如 UNIX Shell 或 CMD 命令等）。
危险等级：高

**passthru()**
功能描述：允许执行一个外部程序并回显输出，类似于 exec()。
危险等级：高

**system()**
功能描述：允许执行一个外部程序并回显输出，类似于 passthru()。
危险等级：高

**chroot()**
功能描述：可改变当前 PHP 进程的工作根目录，仅当系统支持 CLI 模式
PHP 时才能工作，且该函数不适用于 Windows 系统。
危险等级：高

**scandir()**
功能描述：列出指定路径中的文件和目录。
危险等级：中

**chgrp()**
功能描述：改变文件或目录所属的用户组。
危险等级：高

**chown()**
功能描述：改变文件或目录的所有者。
危险等级：高

**shell_exec()**
功能描述：通过 Shell 执行命令，并将执行结果作为字符串返回。
危险等级：高

**proc_open()**
功能描述：执行一个命令并打开文件指针用于读取以及写入。
危险等级：高

**proc_get_status()**
功能描述：获取使用 proc_open() 所打开进程的信息。
危险等级：高

**error_log()**
功能描述：将错误信息发送到指定位置（文件）。
安全备注：在某些版本的 PHP 中，可使用 error_log() 绕过 PHP safe mode，
执行任意命令。
危险等级：低

**ini_alter()**
功能描述：是 ini_set() 函数的一个别名函数，功能与 ini_set() 相同。
具体参见 ini_set()。
危险等级：高

**ini_set()**
功能描述：可用于修改、设置 PHP 环境配置参数。
危险等级：高

**ini_restore()**
功能描述：可用于恢复 PHP 环境配置参数到其初始值。
危险等级：高

**dl()**
功能描述：在 PHP 进行运行过程当中（而非启动时）加载一个 PHP 外部模块。
危险等级：高

**pfsockopen()**
功能描述：建立一个 Internet 或 UNIX 域的 socket 持久连接。
危险等级：高

**syslog()**
功能描述：可调用 UNIX 系统的系统层 syslog() 函数。
危险等级：中

**readlink()**
功能描述：返回符号连接指向的目标文件内容。
危险等级：中

**symlink()**
功能描述：在 UNIX 系统中建立一个符号链接。
危险等级：高

**popen()**
功能描述：可通过 popen() 的参数传递一条命令，并对 popen() 所打开的文件进行执行。
危险等级：高

**stream_socket_server()**
功能描述：建立一个 Internet 或 UNIX 服务器连接。
危险等级：中

**putenv()**
功能描述：用于在 PHP 运行时改变系统字符集环境。在低于 5.2.6 版本的 PHP 中，可利用该函数
修改系统字符集环境后，利用 sendmail 指令发送特殊参数执行系统 SHELL 命令。
危险等级：高

