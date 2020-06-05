---
title: Linux—添加开机启动(服务/脚本)
tags:
  - linux
  - rc.local
  - profile
  - 开机启动
categories:
  - 技术分享
date: 2020-06-05 10:04:46
---

**系统启动时需要加载的配置文件**
/etc/profile、/root/.bash_profile
/etc/bashrc、/root/.bashrc
/etc/profile.d/*.sh、/etc/profile.d/lang.sh
/etc/sysconfig/i18n、/etc/rc.local（/etc/rc.d/rc.local）
<!--more-->
**一、修改开机启动文件**

1. /etc/rc.local（或者/etc/rc.d/rc.local）

```bash
# 1.编辑rc.local文件，该文件是一个软链接，可以直接编辑/etc/rc.d/rc.local
[root@localhost ~]# vi /etc/rc.d/rc.local

# 2.修改rc.local文件加入以下命令。保存并退出。
/etc/init.d/mysqld start                                         # mysql开机启动
/etc/init.d/nginx start                                          # nginx开机启动

# 3.最后修改rc.local文件的执行权限
[root@localhost ~]# chmod +x  /etc/rc.d/rc.local
[root@localhost ~]# chmod 755 /etc/rc.d/rc.local
[root@localhost ~]# systemctl status rc-local                    # 查看一下服务是否自启动
[root@localhost ~]# systemctl start rc-local                     # 启动服务
[root@localhost ~]# systemctl enable rc-local                    # 开机自启
```

2. /etc/profile (或者 /etc/profile.d/)

```bash
# 1.编辑profile文件，在文件中添加需要运行的脚本命令
[root@localhost ~]# vi /etc/profile

# 2.将脚本(.sh文件)放到目录 /etc/profile.d/ 下，系统启动后就会自动执行该目录下的所有shell脚本  （账号登录执行,开机不执行）
```

   

**二、自己写一个shell脚本**
将写好的脚本（.sh文件）放到目录 /etc/profile.d/ 下，系统启动后就会自动执行该目录下的所有shell脚本。
**三、通过chkconfig命令设置**

```
# 1.将(脚本)启动文件移动到 /etc/init.d/或者/etc/rc.d/init.d/目录下。（前者是后者的软连接）
mv  /www/wwwroot/test.sh /etc/rc.d/init.d

# 2.启动文件前面务必添加如下三行代码，否侧会提示chkconfig不支持。
#!/bin/sh                          告诉系统使用的shell,所以的shell脚本都是这样
#chkconfig: 35 20 80               分别代表运行级别，启动优先权，关闭优先权，此行代码必须
#description: http server          自己随便发挥！！！，此行代码必须
/bin/echo $(/bin/date +%F_%T) >> /tmp/test.log

# 3.增加脚本的可执行权限
chmod +x  /etc/rc.d/init.d/test.sh

# 4.添加脚本到开机自动启动项目中。添加到chkconfig，开机自启动。
[root@localhost ~]# cd /etc/rc.d/init.d
[root@localhost ~]# chkconfig --add test.sh
[root@localhost ~]# chkconfig test.sh on

# 5.关闭开机启动 
[root@localhost ~]# chkconfig test.sh off

# 6.从chkconfig管理中删除test.sh
[root@localhost ~]# chkconfig --del test.sh

# 7.查看chkconfig管理
[root@localhost ~]# chkconfig --list test.sh
```

**四、自定义服务文件，添加到系统服务，通过Systemctl管理**
1.写服务文件：如nginx.service、redis.service、supervisord.service

```
[Unit]:服务的说明
Description:描述服务
After:描述服务类别

[Service]服务运行参数的设置
Type=forking            是后台运行的形式
ExecStart               为服务的具体运行命令
ExecReload              为服务的重启命令
ExecStop                为服务的停止命令
PrivateTmp=True         表示给服务分配独立的临时空间
注意：启动、重启、停止命令全部要求使用绝对路径

[Install]               服务安装的相关设置，可设置为多用户
WantedBy=multi-user.target 
```

2.文件保存在目录下：以754的权限。目录路径：/usr/lib/systemd/system。如上面的supervisord.service文件放在这个目录下面。

```
[root@localhost ~]# cat /usr/lib/systemd/system/nginx.service
[root@localhost ~]# cat /usr/lib/systemd/system/supervisord.service
```

3.设置开机自启动(任意目录下执行)。如果执行启动命令报错，则执行：systemctl daemon-reload

```
设置开机自启动
[root@localhost ~]# systemctl enable nginx.service       
[root@localhost ~]# systemctl enable supervisord

停止开机自启动
[root@localhost ~]# systemctl disable nginx.service
[root@localhost ~]# systemctl disable supervisord       # 验证一下是否为开机启动
```

4.其他命令

```
启动nginx服务
[root@localhost ~]# systemctl start nginx.service
停止nginx服务
[root@localhost ~]# systemctl start nginx.service
重启nginx服务
[root@localhost ~]# systemctl restart nginx.service
查看nginx服务当前状态
[root@localhost ~]# systemctl status nginx.service
查看所有已启动的服务
[root@localhost ~]# systemctl list-units --type=service
```

5.服务文件示例：

```
# supervisord.service进程管理服务文件[Unit]
Description=Process Monitoring and Control Daemon   # 内容自己定义：Description=Supervisor daemon
After=rc-local.service nss-user-lookup.target

[Service]
Type=forking
ExecStart=/usr/bin/supervisord -c /etc/supervisor/supervisord.confExecStop= /usr/bin/supervisorctl shutdown 
ExecReload=/usr/bin/supervisorctl reloadRestart=on-failureRestartSec=42sKillMode=process 
[Install]
WantedBy=multi-user.target
```

```
# nginx.service服务文件[Unit]
Description=nginx - high performance web server
After=network.target remote-fs.target nss-lookup.target
[Service]
Type=forking
ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s stop
[Install]
WantedBy=multi-user.target
```

```
# redis.service服务文件
[Unit]
Description=Redis
After=network.target remote-fs.target nss-lookup.target
[Service]
Type=forking
ExecStart=/usr/local/bin/redis-server /etc/redis.conf
ExecStop=kill -INT `cat /tmp/redis.pid`
User=www
Group=www
[Install]
WantedBy=multi-user.target
```

 https://www.cnblogs.com/zwcry/p/9602756.html

 https://www.jianshu.com/p/e1442913eb0e