---
title: 解决vim粘贴格式混乱(自动缩进问题)
tags:
  - vim
categories:
  - 技术分享
date: 2019-10-31 13:30:42
---

>  问题如下，复制后因为自动缩进，产生了每行行首的自动缩进累积：
```
#原内容  
  'WeChat' =>
    array (
      'Token' => '',
      'EncodingAESKey' => '',
      'AppID' => '',
    ),
#粘贴后内容             
  'WeChat' =>
    array (
        'Token' => '',
            'EncodingAESKey' => '',
                'AppID' => '',
                    ),
```

#### 方法一：

在粘贴前，取消缩进

```
:set paste
```

粘贴完后，恢复缩进

```
:set nopaste
```
<!-- more -->

#### 方法二：

推荐的方式，使用`+p` 或 `*p` 粘贴缓冲区内容， `+p`比 `ctrl+v`命令更好，它可以更快更可靠地处理大块文本的粘贴，也能够避免粘贴大量文本时，发生每行行首的自动缩进累积，因为`ctrl+v`是通过系统缓存的stream处理，一行一行地处理粘贴的文本。 

#### 方法三：

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

#### 方法四：

在配置文件中增加如下一行，使用鼠标复制内容到选择缓冲区：
```
vim /etc/vimrc 
set mouse=v
```
需要带行号时，使用鼠标选择内容区域。
不需要行号，使用`yny`复制n行或可视模式下选择。
将选择缓冲区中内容粘贴到vim文件：普通模式下按`+p` 或 `*p`粘贴缓冲区内容




