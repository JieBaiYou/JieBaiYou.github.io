---
title: 解决vim粘贴格式混乱(TAB缩进)
tags:
  - vim
categories:
  - 技术分享
date: 2019-10-31 13:30:42
---

>  问题如下：  

![]( http://q02nuv786.bkt.clouddn.com/vimpaste.png )

方法一：

在粘贴前，取消缩进

```
:set paste
```

粘贴完后，恢复缩进

```
:set nopaste
```

方法二：

推荐的方式，使用`+p` `*p` 复制缓冲区

<!-- more -->

方法三：

使用键盘映射，这样在每一次复制前按F11取消缩进，复制后再按一次恢复缩进

```
vim /etc/vimrc 
set pastetoggle=<F11>
```

当然也可以

```
:map <F10> :set paste<CR>
:map <F11> :set nopaste<CR>
```

方法四：

在/etc/vimrc中增加如下一行，使用鼠标复制内容到选择缓冲区：
```
set mouse=v
```
需要带行号时，使用鼠标选择内容区域。
不需要行号，使用`*yny`复制n行或可视模式下选择。
将选择缓冲区中内容粘贴到vim文件：普通模式下按`*p`粘贴缓冲区内容




