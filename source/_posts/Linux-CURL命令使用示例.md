---
title: Linux CURL命令使用示例
tags:
  - linux
  - curl
categories:
  - 技术分享
date: 2019-11-07 13:41:05
---

>1.curl是libcurl这个库支持的，默认支持HTTP1.1（也支持1.0）。
2.curl支持很多的协议。curl supports FTP, FTPS, HTTP, HTTPS, SCP, SFTP, TFTP, TELNET, DICT, LDAP, LDAPS, FILE, POP3, IMAP, SMTP and RTSP at the time of this writing. Wget supports HTTP, HTTPS and FTP.

<!-- more -->

### curl [option] [url]

#### 1.获取页面内容

当我们不加任何选项使用curl时，默认会发送GET请求来获取链接内容到标准输出

```css
curl localhost:80
```

#### 2.显示HTTP头

```undefined
option： -I
curl -I localhost:80
```

##### 结果

```dart
HTTP/1.1 200 OK
Server: nginx
Date: Wed, 06 Nov 2019 01:54:45 GMT
Content-Type: text/html; charset=UTF-8
Connection: keep-alive
Vary: Accept-Encoding
Set-Cookie: PHPSESSID=123sfjd7pudferq6dmvtnfhang; path=/
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Set-Cookie: user%5BmultiLogin%5D=no; path=/
```

#### 3.显示HTTP头和文件内容

```undefined
option： -i
curl -i localhost:80
```

##### 结果

```xml
HTTP/1.1 200 OK
Server: nginx
Date: Wed, 06 Nov 2019 01:55:11 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Vary: Accept-Encoding
Set-Cookie: PHPSESSID=ueuau8ks4ufltk1vhc39tps8va; path=/
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Set-Cookie: user%5BmultiLogin%5D=no; path=/

<!DOCTYPE html><html xmlns="http://www.w3.org/1999/xhtml"><head>
    ...
    ...
</head></html>
```

#### 4.将链接保存到文件

通过输出重定向符号“>”将输出指定到文件中

```css
curl localhost:80 > index.html
option：-o
curl -o index.html localhost:80
option：-O    #URL中要有文件名
curl -O localhost:80/file
```

#### 5.同时下载多个文件

```undefined
curl -O localhost:80/file1 -O localhost:80/file2
curl -o file1.html localhost:80/file/1 -o file2.html localhost:80/file/2
```

#### 6.跟随链接重定向

```undefined
option -L
curl -L localhost:80
```

#### 7.自定义header

```undefined
option -H
curl -H "Host: 118.31.76.144:80" -H "Cookie: token=we8rw9r32ujew8r2jew9823" http://zq.com/html/index.html
```

#### 8.发送POST请求

```undefined
option -d
curl -d "username=root&password=123456"  -X POST  http://zq.com/login
```

*-d：用于指定发送的数据，-X：用于指定请求方式*
 *注：在使用-d(默认为POST请求)发送POST请求是可不加-X*
 如：

```cpp
curl -d "username=root&password=123456"   http://zq.com/login
curl -d "@data.txt" http://zq.com/login      #从文件中读取数据
```

#### 9.发送GET请求

```ruby
curl -d "data" -X GET http://zq.com/api       #强制使用GET请求
curl -d "data" -G http://zq.com/api
```

#### 10.读取 Cookie

```bash
option -b   #-b后面可以是 Cookie 字符串，也可以是保存了 Cookie 的文件名
curl -b "token=we8rw9r32ujew8r2jew9823" http://zq.com/html/index.html
curl -b "collie-filename" http://zq.com/html/index.html
```

#### 11.保存 Cookie

```swift
option -c
curl -c "collie-filename" http://zq.com/html/index.html
```

#### 11.上传

```undefined
option --form
 curl --form "fileupload=@filename.txt" http://zq.com/resource
```

常见参数

```xml
-A/--user-agent <string> 设置用户代理发送给服务器
-b/--cookie <name=string/file> cookie字符串或文件读取位置
-c/--cookie-jar <file> 操作结束后把cookie写入到这个文件中
-C/--continue-at <offset> 断点续转
-D/--dump-header <file> 把header信息写入到该文件中
-e/--referer 来源网址
-f/--fail 连接失败时不显示http错误
-o/--output 把输出写到该文件中
-O/--remote-name 把输出写到该文件中，保留远程文件的文件名
-r/--range <range> 检索来自HTTP/1.1或FTP服务器字节范围
-s/--silent 静音模式。不输出任何东西
-T/--upload-file <file> 上传文件
-u/--user <user[:password]> 设置服务器的用户和密码
-w/--write-out [format] 什么输出完成后
-x/--proxy <host[:port]> 在给定的端口上使用HTTP代理
-#/--progress-bar 进度条显示当前的传送状态
-z 判断日期
--limit-rate 下载限速
```