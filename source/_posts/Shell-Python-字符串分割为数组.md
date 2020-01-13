---
title: Shell/Python 字符串分割为数组
tags:
  - shell
  - python
  - 字符串
  - 数组
categories:
  - 技术分享
date: 2020-01-04 09:32:20
---

Shell中要将字符串列表转变为数组，只需要在前面加()，所以关键是将分隔符转变为空格分隔，常用有下面几种方法

### Shell分割字符串为数组

##### 方法一: 借助于{str//,/}来处理
```
# str="ONE,TWO,THREE,FOUR"
# arr=(${str//,/})
# echo ${arr[@]}
ONE TWO THREE FOUR
```
<!-- more -->

##### 方法二: 借助于tr命令来处理

```
# str="ONE,TWO,THREE,FOUR"
# arr=(`echo $str | tr ',' ' '`) 
# echo ${arr[@]}
ONE TWO THREE FOUR
```

##### 方法三: 借助于awk命令来处理
```
# str="ONE,TWO,THREE,FOUR"
# arr=($(echo $str | awk 'BEGIN{FS=",";OFS=" "} {print $1,$2,$3,$4}'))
# echo ${str[*]}
```

##### 方法四: 借助于IFS来处理分隔符
```
# str="ONE,TWO,THREE,FOUR"
# IFS=","
# arr=(str)
# echo ${str[@]}
```

##### 方法五：使用AWK分割无分隔符的文本

```
# text="abcdefghijklmnopqrstuvwxyz"
# echo $text | awk 'BEGIN { FS=OFS="" } { for( i=1; i<=NF; i++ ) { if( i%4==0 && i!=NF ) { printf $i" " } else { printf $i }} print "" }'
```



### Python分隔字符串为数组

##### re

```
import re
re.findall('..','1234567890')
['12', '34', '56', '78', '90']
```

也可以这样做：

```
import re
re.findall('..?', '123456789')
['12', '34', '56', '78', '9']
```

您还可以执行以下操作，以简化较长块的正则表达式：

```
import re
re.findall('.{1,2}', '123456789')
['12', '34', '56', '78', '9']
```

##### lambda

```
split_string = lambda x, n: [x[i:i+n] for i in range(0, len(x), n)]
s = '1234567890'
split_string(s,2)
['12', '34', '56', '78', '90']
```

##### other

```
s = '1234567890'
o = []
while s:
    o.append(s[:2])
    s = s[2:]
```

```
s='1234567890'
print([s[idx:idx+2] for idx,val in enumerate(s) if idx%2 == 0])
```





