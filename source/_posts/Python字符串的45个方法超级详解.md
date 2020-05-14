---
title: Python字符串的45个方法超级详解
tags:
  - python
categories:
  - 技术分享
date: 2020-05-14 11:36:41
---

Python中字符串对象提供了很多方法来操作字符串，功能相当丰富。必须进行全面的了解与学习，后面的代码处理才能更得心应手，编程水平走向新台阶的坚实基础。目前一共有45个方法，给大家分类整理，可以收藏查询使用。

```
# 获取字所有的符串方法
print(dir(str))
[...,'capitalize', 'casefold', 'center', 'count', 'encode', 'endswith',
'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 
'isascii', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 
'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 
'lstrip', 'maketrans', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 
'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 
'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
```

## 一、大小写转换

### 01、capitalize()

**描述：**将字符串的第一个字母变成大写，其余字母变为小写。

**语法：**str.capitalize()

**示例：**

```python
"i Love python".capitalize() 
'I love python'

"i Love pYthoN".capitalize() 
'I love python'
```

<!--more-->

### 02、title()

**描述：**返回一个满足**标题格式**的字符串。即所有英文单词首字母大写，其余英文字母小写。

**语法：**str.title()

**示例：**

```python
"i am very love python".title()
'I Am Very Love Python'
```



### 03、swapcase()

**描述：**将字符串str中的大小写字母同时进行互换。即将字符串str中的大写字母转换为小写字母，将小写字母转换为大写字母。

**语法：**str.swapcase()

**示例：**

```python
"I Am Love PYTHON".swapcase()
'i aM lOVE python'

"我爱pythoN Python pYTHON".swapcase()
'我爱PYTHOn pYTHON Python'
```



### 04、lower()

**描述：**将字符串中的所有大写字母转换为小写字母。

**语法：**str.lower()

**示例：**

```python
"我爱pythoN Python!".lower()
'我爱python python!'
```



### 05、upper()

**描述：**将字符串中的所有小写字母转换为大写字母。

**语法：** str.upper()

**示例：**

```python
"i am very love python".upper()
'I AM VERY LOVE PYTHON'
```



### 06、casefold()

**描述：**将字符串中的所有大写字母转换为小写字母。也可以将非英文 语言中的大写转换为小写。

注意 ：lower()函数和casefold()函数的区别：lower() 方法只对ASCII编码，即‘A-Z’有效，对于其它语言中把大写转换为小写的情况无效，只能用 casefold() 函数。

**语法：**str.casefold()

**示例：**

```python
 "Groß - α".casefold()#德语 
'gross - α'

"I am verY love python".casefold()
'i am very love python'
```



## 二、字符串填充

### 07、center()

**描述：**返回一个长度为width,两边用fillchar(单字符)填充的字符串，即字符串str居中，两边用fillchar填充。若字符串的长度大于width,则直接返回字符串str。

**语法：**str.center(width , "fillchar")

- width —— 指定字符串长度。
- fillchar —— 要填充的单字符，默认为空格。

**示例：**

```python
'shuai'.center(10)
'  shuai   '

'shuai'.center(10,'*')
'**shuai***'

#名字补齐
L = ['Jack','jenny','joe']
[name.center(10,'#') for name in L]
['###Jack###', '##jenny###', '###joe####']
 
for name in L:
    print(name.center(10,'#'))
###Jack###
##jenny###
###joe####    
```



### 08、ljust()

**描述：**返回一个原字符串左对齐,并使用fillchar填充(默认为空格)至指定长度的新字符串。如果指定的长度小于原字符串的长度则返回原字符串。

**语法：** str.ljust(width, fillchar) -> str 返回一个新的字符串

- width —— 指定字符串的输出长度。
- fillchar—— 将要填充的单字符，默认为空格。

示例：

```python
'shuai'.ljust(10)
'shuai     '

'shuai'.ljust(10,'*')
'shuai*****'

L = ['Jack','jenny','joe']
[name.ljust(10,'#') for name in L]
['Jack######', 'jenny#####', 'joe#######']
 
for name in L:
    print(name.ljust(10,'#'))
Jack######
jenny#####
joe######
```

### 

### **09、rjust()**

**描述：**返回一个原字符串右对齐,并使用fillchar填充(默认为空格)至指定长度的新字符串。如果指定的长度小于原字符串的长度则返回原字符串。

**语法：** str.ljust(width, fillchar)

- width —— 指定字符串的输出长度。
- fillchar—— 将要填充的单字符，默认为空格。

**示例：**

```python
'shuai'.rjust(10)
'     shuai'

'shuai'.rjust(10,'*')
'*****shuai'

L = ['Jack','jenny','joe']
[name.rjust(10,'#') for name in L]
['######Jack', '#####jenny', '#######joe']
 
for name in L:
    print(name.rjust(10,'*'))
******Jack
*****jenny
*******joe

for name in L:
    print(name.rjust(10,'好'))
好好好好好好Jack
好好好好好jenny
好好好好好好好j
```



### 10、zfill()

**描述：**返回指定长度的字符串，使原字符串右对齐，前面用0填充到指定字符串长度。

**语法：**str.zfill(width)

width —— 指定字符串的长度,但不能为空。若指定长度小于字符串长度，则直接输出原字符串。

**示例：**

```python
'12'.zfill(10)
'0000000012'

#对用户ID进行填充
L=['56783','34','987766721','326']  
[id.zfill(10) for id in L]
['0000056783', '0000000034', '0987766721', '0000000326']

for id in L:
    print(id.zfill(10))
0000056783
0000000034
0987766721
0000000326    

#等价于用0填充的右对齐
[id.rjust(10,'0') for id in L]
['0000056783', '0000000034', '0987766721', '0000000326
```



## **三、字符串编码**

### 11、encode()

**描述：**以指定的编码格式编码字符串，默认编码为 'utf-8'。encode英文原意 编码。

**语法：**str.encode(encoding='utf-8', errors='strict')

- encoding 参数可选，即要使用的编码，默认编码为 'utf-8'。字符串编码常用类型有：utf-8，gb2312，cp936，gbk等。
- errors 参数可选，设置不同错误的处理方案。默认为 'strict',意为编码错误引起一个UnicodeEncodeError。 其它可能值有 'ignore', 'replace', 'xmlcharrefreplace'以及通过 codecs.register_error() 注册其它的值。

**示例：**

```python
"我爱祖国".encode(encoding="utf8",errors="strict")
b'\\xe6\\x88\\x91\\xe7\\x88\\xb1\\xe7\\xa5\\x96\\xe5\\x9b\\xbd'

"I love my country".encode(encoding="utf8",errors="strict")
 b'I love my country'
```



### 12、decode()

**描述：**以 encoding 指定的编码格式解码字符串，默认编码为字符串编码。decode英文意思是 解码，

**语法：**str.decode(encoding='utf-8', errors='strict')

- encoding ——要使用的编码，如：utf-8,gb2312,cp936,gbk等。
- errors ——设置不同解码错误的处理方案。默认为 'strict',意为编码错误引起一个 UnicodeDecodeError。 其它可能得值有 'ignore', 'replace'以及通过 codecs.register_error() 注册的1其它值。

**示例：**

```python
str1 = "我爱学习".encode(encoding="utf-8")
str1
b'\\xe6\\x88\\x91\\xe7\\x88\\xb1\\xe5\\xad\\xa6\\xe4\\xb9\\xa0'

str1.decode(encoding="utf-8")
'我爱学习'
```



## 四、字符串查找

### 13、find()

**描述：**查找字符串中指定的子字符串sub第一次出现的位置，可以规定字符串的索引查找范围。若无则返回 -1。

**语法：**str.find(sub,start,end) -> int 返回整数

**参数：**

- sub —要索引的子字符串。
- start —索引的起始位置。默认值为0。
- end —索引的结束位置。默认值为字符串长度len(str)。[start,end) 不包括end。

**示例：**

```python
#查找子字符串"o"
"I love python".find('o')
3

#索引起始位置为4 索引范围为：ve python
"I love python".find('o',4)
11

#索引起始位置为4,结束位置为12 索引范围为：ve pytho
"I love python".find('o',4,12)
11

"I love python".find('o',4,11)#不包括11位的'o',返回-1
```



### 14、rfind()

**描述：**查找字符串中指定的子字符串sub最后一次出现的位置，可以规定字符串的索引查找范围。若无则返回 -1。

**语法：**str.rfind(sub,start,end) -> int 返回整数

**参数：**

- sum —要索引的子字符串。
- start —索引的起始位置。默认值为0。
- end —索引的结束位置。默认值为字符串长度len(str)。[start,end) 不包括end。

注：rfind()函数用法与find()函数相似，rfind()函数返回指定子字符串最后一次出现的位置，find()函数返回指定子字符串第一次出现的位置。

**示例：**

```python
#查找子字符串"o"
"I love python".find('o')
3

#索引起始位置为4 索引范围为：ve python
"I love python".find('o',4)
11

#索引起始位置为4,结束位置为12 索引范围为：ve pytho
"I love python".find('o',4,12
```



### 15、index()

**描述：**查找字符串中第一次出现的子字符串的位置，可以规定字符串的索引查找范围[star,end)。若无则会报错。

**语法：**str.index(sub, start, end) -> int 返回整数

**参数：**

- sub —— 查找的子字符串。
- start —— 索引的起始位置，默认为0。
- end —— 索引的结束位置，默认为字符串的长度。

**示例：**

```python
"I love python".index("o") #默认索引整个字符串

"I love python".index("o",4)  #索引 ve python
11

"I love python".index("o",4,12) #索引 ve pytho
11

"I love python".index("love")    #索引多个字符
2

"I love python".index("k")   #索引字符串不存在，报错
ValueError: substring not fou
```



### 16、rindex()

**描述：** rindex() 方法返回子字符串最后一次出现在字符串中的索引位置，该方法与 rfind() 方法一样，可以规定字符串的索引查找范围[star,end)，只不过如果子字符串不在字符串中会报一个异常。

**语法：**str.rindex(sub, start, end) -> int 返回整数。

**参数：**

- sub —— 查找的子字符串。
- start —— 索引的起始位置，默认为0。
- end —— 索引的结束位置，默认为字符串的长度。

**示例：**

```python
"I love python".rindex('o')
11

"I love python".index('o')
3

"I love python".rindex('k')
ValueError: substring not found

"I love python".rfind('k'
```



## 五、字符串格式化

### 17、format()

**描述：**Python2.6 开始，新增了一种格式化字符串的函数 **str.format()**，它增强了字符串格式化的功能。基本语法是通过 **{}** 和 **:** 来代替以前的 **%** 。使用format()来格式化字符串时，使用在字符串中使用{}作为占位符，占位符的内容将引用format()中的参数进行替换。可以是位置参数、命名参数或者兼而有之。

format 函数可以接受不限个参数，位置可以不按顺序。

**语法：**format(value, format_spec)

**参数：**

**示例：**

```python
# 位置参数
'{}:您{}购买的{}到了！请下楼取快递。'.format('快递小哥','淘宝','快递')
'快递小哥:您淘宝购买的快递到了！请下楼取快递。'

#给批量客户发短息
n_list=['马云','马化腾','麻子','小红','李彦宏','二狗子']
for name in n_list:
    print('{0}：您淘宝购买的快递到了！请下楼取快递！'.format(name))
马云：您淘宝购买的快递到了！请下楼取快递！
马化腾：您淘宝购买的快递到了！请下楼取快递！
麻子：您淘宝购买的快递到了！请下楼取快递！
小红：您淘宝购买的快递到了！请下楼取快递！
李彦宏：您淘宝购买的快递到了！请下楼取快递！
二狗子：您淘宝购买的快递到了！请下楼取快递！  
    
#名字进行填充    
for n in n_list:
    print('{0}：您淘宝购买的快递到了！请下楼取快递！'.format(n.center(3,'*')))
    
*马云：您淘宝购买的快递到了！请下楼取快递！
马化腾：您淘宝购买的快递到了！请下楼取快递！
*麻子：您淘宝购买的快递到了！请下楼取快递！
*小红：您淘宝购买的快递到了！请下楼取快递！
李彦宏：您淘宝购买的快递到了！请下楼取快递！
二狗子：您淘宝购买的快递到了！请下楼取快递！

'{0}, {1} and {2}'.format('gao','fu','shuai')
'gao, fu and shuai'

x=3
y=5
'{0}+{1}={2}'.format(x,y,x+y)

# 命名参数
'{name1}, {name2} and {name3}'.format(name1='gao', name2='fu', name3='shuai')
'gao, fu and shuai'

# 混合位置参数、命名参数
'{name1}, {0} and {name3}'.format("shuai", name1='fu', name3='gao')
'fu, shuai and gao'

#for循环进行批量处理
["vec_{0}".format(i) for i in range(0,5)]
['vec_0', 'vec_1', 'vec_2', 'vec_3', 'vec_4']

['f_{}'.format(r) for r in list('abcde')]
['f_a', 'f_b', 'f_c'
```



### 18、format_map()

**描述：**返回字符串的格式化版本。在Python3中使用format和format_map方法都可以进行字符串格式化，但format是一种所有情况都能使用的格式化方法，format_map仅使用于字符串格式中可变数据参数来源于字典等映射关系数据时才可以使用。

**语法：**str.format_map(mapping) -> str 返回字符串

**参数：**mapping 是一个字典对象

**示例：**

```python
People = {"name": "john", "age": 33}
"My name is {name},iam{age} old".format_map(People)

#对比案例
定义一个字典
student = {'name':'张三','class':'20200504','score':748}

使用format输出相关信息：
'{st[class]}班{st[name]}总分：{st[score]}'.format(st=student)
'20200504班张三总分：748'

format_map方法后如下

'{class}班{name}总分：{score}'.format_map(student)
'20200504班张三总分：7
```



## 六、解决判断问题

### 19、endswith()

**描述：**判断字符串是否以指定字符或子字符串结尾。

**语法：**str.endswith("suffix", start, end) 或str[start,end].endswith("suffix") 用于判断字符串中某段字符串是否以指定字符或子字符串结尾。—> bool 返回值为布尔类型（True,False）

**参数：**

- suffix — 后缀，可以是单个字符，也可以是字符串，还可以是元组（"suffix"中的引号要省略，常用于判断文件类型）。
- start —索引字符串的起始位置。
- end — 索引字符串的结束位置。

注意：空字符的情况。返回值通常为True

**示例：**

```python
"I love python".endswith('n')
True
"I love python".endswith("python")
True
"I love python".endswith("n",0,6)# 索引 i love 是否以“n”结尾。
False
"I love python".endswith("") #空字符
True
"I love python".endswith(("n","z"))#遍历元组的元素，存在即返回True，否者返回False
True
"I love python".endswith(("k","m"))
False

#元组案例
file = "python.txt"
if file.endswith("txt"):
    print("该文件是文本文件")
elif file.endswith(("AVI","WMV","RM")):
    print("该文件为视频文件")
else:
    print("文件格式未知
```



### 20、startswith()

**描述：**判断字符串是否以指定字符或子字符串开头。

**语法：**str.endswith("suffix", start, end) 或

str[start,end].endswith("suffix") 用于判断字符串中某段字符串是否以指定字符或子字符串结尾。

—> bool 返回值为布尔类型（True,False）

**参数：**

- suffix — 后缀，可以是单个字符，也可以是字符串，还可以是元组（"suffix"中的引号要省略）。
- start —索引字符串的起始位置。
- end — 索引字符串的结束位置。

注意：空字符的情况。返回值通常也为True

**示例：**

```python
"hello,i love python".startswith("h")
True

"hello,i love python".startswith("l",2,10)# 索引 llo,i lo 是否以“l”开头。
True

"hello,i love python".startswith("") #空字符
True

"hello,i love python"[0:6].startswith("h") # 只索引  hello,
True

"hello,i love python"[0:6].startswith("e")
False

"hello,i love python"[0:6].startswith("")
True

"hello,i love python".startswith(("h","z"))#遍历元组的元素，存在即返回True，否者返回False
True

"hello,i love python".startswith(("k","m"))
False
```



### 21、isalnum()

**描述：**检测字符串是否由字母和数字组成。str中至少有一个字符且所有字符都是字母或数字则返回 True,否则返回 False

**语法：**str.isalnum() -> bool 返回值为布尔类型（True,False）

**参数：**

**示例：**

```python
"seven-11".isalnum()
False

"seven11".isalnum()
True

"seven".isalnum()
True

"11".isalnum()
Tr
```



### 22、isalpha()

**描述：**检测字符串是否只由字母组成。字符串中至少有一个字符且所有字符都是字母则返回 True,否则返回 False。

**语法：**str.isalpha() -> bool 返回值为布尔类型（True,False）

**参数：无**

**示例：**

```python
"I love python".isalpha()#存在空格返回False
False

"Ilovepython".isalpha()
True

"Ilovepython123".isalpha()
Fals
```



### 23、isdecimal()

**描述：**检查字符串是否只包含十进制字符。字符串中若只包含十进制字符返回True，否则返回False。该方法只存在于unicode对象中。注意:定义一个十进制字符串，只需要在字符串前添加前缀 'u' 即可。

**语法：** str.isdecimal() -> bool 返回值为布尔类型（True,False）

**参数：无**

**示例：**

```python
"123456".isdecimal()
True
u"123456".isdecimal()
True

"123456python".isdecimal()
False
```



### 24、isdigit()

**描述：**检测字符串是否只由数字组成.字符串中至少有一个字符且所有字符都是数字则返回 True,否则返回 False。

**语法：**str.isdigit() -> bool 返回值为布尔类型（True,False）

**参数：无**

注：能判断“①”，不能判断中文数字。但 isnumeric() 函数可以。

**示例：**

```python
"python".isdigit() #全为字母
False

"123".isdigit()  #全为数字
True

"python666".isdigit()   #字母和数字的组合
False

"一二三四五六七".isdigit() #中文数字输出False
False

"①".isdigit()  
True
```



### 25、isidentifier()

**描述：**判断str是否是有效的标识符。str为符合命名规则的变量，保留标识符则返回True,否者返回False。

**语法：**str.isidentifier() -> bool 返回值为布尔类型（True,False）

**参数：**无

**示例：**

```python
"123".isidentifier()  #变量名为123
False

"def".isidentifier()  #变量名为保留字
True

"_123".isidentifier()  #变量名有下划线开头
True

"student".isidentifier()#变量名由字母开端
True
```



### 26、islower()

**描述：**检测字符串中的字母是否全由小写字母组成。（字符串中可包含非字母字符）字符串中包含至少一个区分大小写的字符，且所有这些区分大小写的字符都是小写，则返回 True，否则返回 False。

**语法：**str.islower() -> bool 返回值为布尔类型（True,False）

**参数：**无

**示例：**

```python
#字符串中的字母全为小写
"i love python".islower()  
True

#字符串中的字母全为小写，也存在非字母的字符
"我爱python！".islower() 
True

#字符串中有大写字符
"I love python".islower() 
False
```



### 27、isupper()

**描述：**检测字符串中的字母是否全由大写字母组成。（字符串中可包含非字母字符）。字符串中包含至少一个区分大小写的字符，且所有这些区分大小写的字符都是大写，则返回 True，否则返回 False。

**语法：**str.isupper() -> bool 返回值为布尔类型（True,False）

**参数：**无

**示例：**

```python
"I LOVE PYTHON".isupper() #全为大写字母
True

"i LOVE PYTHON".isupper()  #存在小写字母
False

"我爱PYTHON".isupper()  #存在非字母的字符
Tru
```



### 28、inumeric()

**描述：**测字符串是否只由数字组成。这种方法是只适用于unicode对象。字符串中只包含数字字符，则返回 True，否则返回 False。

**语法：**str.isnumeric() -> bool 返回值为布尔类型（True,False）

**参数：**无

**示例：**

```python
u"123456".isnumeric()  #全为数字
True

"123456".isnumeric()
True

"python666".isnumeric()  #字母数字组合
False

"一二三四五六".isnumeric()  #中文数字
True

"①".isnumeric()
Tr
```



### 29、isprintable()

**描述：**判断字符串中是否有打印后不可见的内容。如：\n \t 等字符。若字符串中不存在\n \t 等不可见的内容，则返回True,否者返回False。

**语法：** str.isprintable() -> bool 返回值为布尔类型（True,False）

**参数：**无

**示例：**

```python
#不存在用print()打印后不可见的内容
"i love  python".isprintable()  
True

#存在用print()打印后不可见的内容 \n
"i love python \n".isprintable() 
False

"i love \t python".isprintable()
Fals
```



### 30、isspace()

**描述：** 检测字符串是否只由空格组成。若字符串中只包含空格，则返回 True，否则返回 False。

**语法：**str.isspace() -> bool 返回值为布尔类型（True,False）

**参数：**无

**示例：**

```python
str1 = "   "#空格
str2 = "i love python" 
print(str1.isspace())
True
print(str2.isspace())
False
print(str2[1].isspace()) #字符串str2 的第二个字符为空格
True
```



### 31、istitle()

**描述：**检测判断字符串中所有单词的首字母是否为大写，且其它字母是否为小写，字符串中可以存在其它非字母的字符。若字符串中所有单词的首字母为大写，且其它字母为小写，则返回 True，否则返回 False.

**语法：**str.istitle() -> bool 返回值为布尔类型（True,False）

**参数：**无

**示例：**

```python
"I Love Python".istitle() #各单词的首字母均为大写，其余字母为小写
True
"I love python".istitle() 
False

"I LOVE PYTHON".istitle()
False

"我爱Python".istitle()  #存在其它非字母字符，
Tru
```



## **七、字符串修剪**

### 32、strip()

**描述：**该函数的作用是去除字符串开头和结尾处指定的字符，不会去除字符串中间对应的字符

**语法：**str.strip(chars)

**参数：**chars -- 要去除的字符 默认为空格或换行符。

**示例：**

```python
#默认参数，去除了空格，\n \t \r字符，且未除去字符串中间相应的字符
a = ' \n111 aaa  '
print(a.strip())
111 aaa

#去除两端的指定字符
b='.-.word:我很帅.-.'
print(b.strip('.-.'))
word:我很帅

c='参考：来自公众号AI入门学习'
print(c.strip('参考：'))
来自公众号AI入门学
```



### 33、lstrip()

**描述：**lstrip() 方法用于截掉字符串左边的空格或指定字符。

**语法：**str.lstrip(chars)

**参数：**chars--要去除的字符 默认为空格或换行符。

**示例：**

```python
#去除左边指定字符
a = '--我爱Python--'
a.lstrip('--')
'我爱Python--'

#重复的值只需要写一个
a.lstrip('-')
'我爱Python--'
```



### 34、 rstrip()

**描述：** 删除 str 字符串末尾的指定字符（默认为空格）

**语法：**str.rstrip(chars)

**参数：**chars --要去除的字符 默认为空格或换行符。

**示例：**

```python
#去除左边指定字符
a = '6234412134445533-456'
a.rstrip('-456')
'6234412134445533'

#对一个列表所有的字符串进行去除
ls = ['34667777777-456','62344121344433-456','28993333455-456']
[i.rstrip('-456') for i in ls]
['34667777777', '62344121344433', '28993333']
```



## **八、字符串加密解密**

### 35、maketrans()

**描述：**制作翻译表，删除表，常与translate()函数连用。 即：返回用于str.translate方法翻译的转换表。

**语法：**str.maketrans(intab, outtab，delchars)

**参数：**

- intab -- 字符串中要替代的字符组成的字符串。
- outtab -- 相应的映射字符的字符串。
- delchars -- 可选参数，表示要删除的字符组成的字符串。

**示例：**

```python
str.maketrans() 生成一个字符一对一映射的table，然后使用 translate(table) 对字符串S中的每个字符进行映射。
例如，现在想要对"I love fairy"做一个简单的加密，将里面部分字符都替换为数字，这样别人就不知道转换后的这句话是什么意思。
in_str  = 'afcxyo'
out_str = '123456'

# maketrans()生成映射表
map_table=str.maketrans(in_str,out_str)

# 使用translate()进行映射
my_love='I love fairy'
my_love.translate(map_table)
'I l6ve 21ir5'

注意maketrans(x, y, z]) 中的x和y都是字符串，且长度必须相等。
如果maketrans(x, y, z]) 给定了第三个参数z，这这个参数字符串中的每个字符都会被映射为None。

#'yo'都会被隐藏了
map_table=str.maketrans(in_str,out_str,'yo')
my_love='I love fairy'
my_love.translate(map_table)
'I lve 21
```

### 36、translate()

**描述：**过滤(删除)，翻译字符串。即根据maketrans()函数给出的字符映射转换表来转换字符串中的字符。

注：translate()函数是先过滤(删除)，再根据maketrans()函数返回的转换表来翻译。

**语法：**str.translate(table)

**参数：**

**示例：**

```
见上述案例
```



## 九、分割字符串

### 37、partition()

**描述：**根据指定的分隔符(sep)将字符串进行分割。从字符串左边开始索引分隔符sep,索引到则停止索引。

**语法：** str.partition(sep)

**参数：**sep —— 指定的分隔符。

**返回值：**(head, sep, tail) 返回一个三元元组，head:分隔符sep前的字符串，sep:分隔符本身，tail:分隔符sep后的字符串。如果字符串包含指定的分隔符sep，则返回一个三元元组，第一个为分隔符sep左边的子字符串，第二个为分隔符sep本身，第三个为分隔符sep右边的子字符串。如果字符串不包含指定的分隔符sep,仍然返回一个三元元组，第一个元素为字符串本身，第二第三个元素为空字符串

**示例：**

```python
string = 'https://www.google.com.hk/'

string.partition("://") #字符串str中存在sep"://"
('https', '://', 'www.google.com.hk/')

string.partition(",")  #字符串str中不存在sep",",返回了两个空字符串。
('https://www.google.com.hk/', '', '')

string.partition(".")  #字符串str中存在两个"." 但索引到www后的"."  停止索引。
('https://www', '.', 'google.com.hk/')

type(string.partition("://")) #返回的是tuple类型
tup
```



### 38、rpartition()

**描述：**根据指定的分隔符(sep)将字符串进行分割。从字符串右边(末尾)开始索引分隔符sep,索引到则停止索引。

**语法：** str.rpartition(sep)

**参数：**sep —— 指定的分隔符。

**返回值：** (head, sep, tail) 返回一个三元元组，head:分隔符sep前的字符串，sep:分隔符本身，tail:分隔符sep后的字符串。如果字符串包含指定的分隔符sep，则返回一个三元元组，第一个为分隔符sep左边的子字符串，第二个为分隔符sep本身，第三个为分隔符sep右边的子字符串。如果字符串不包含指定的分隔符sep,仍然返回一个三元元组，第一个元素为字符串本身，第二第三个元素为空字符串。

注：rpartition()函数与partition()函数用法相似，rpartition()函数从右边(末尾)开始索引，partition()函数从左边开始索引。

**示例：**

```python
string = 'https://www.google.com.hk/'

string.rpartition(".")  #字符串str中不存在sep",",返回了两个空字符串。
 ('https://www.google.com', '.', 'hk/')
string.partition(".")  #字符串str中不存在sep",",返回了两个空字符串。
('https://www', '.', 'google.com.hk/')
```



### 39、split()

**描述：**拆分字符串。通过指定分隔符sep对字符串进行分割，并返回分割后的字符串列表。

**语法：** str.split(sep=None, maxsplit=-1) [n]

- sep —— 分隔符，默认为空格,但不能为空即(")。
- maxsplit —— 最大分割参数，默认参数为-1。
- [n] —— 返回列表中下标为n的元素。列表索引的用法。

**示例：**

```python
#默认空格分割
str1 = "I love python"
str1.split()
['I', 'love', 'python']

#取第三位
str1.split()[2]
'python'

#以"."为分隔符,maxsplit默认为-1
str2 = '列夫·尼古拉耶维奇·托尔斯泰'
str2.split('·')
['列夫', '尼古拉耶维奇', '托尔斯泰']

#以"."为分隔符,只分割一次。
str2.split('·',1)
 ['列夫', '尼古拉耶维奇·托尔斯泰
```



### **40、rsplit()**

**描述：**拆分字符串。通过指定分隔符sep对字符串进行分割，并返回分割后的字符串列表,类似于split()函数，只不过 rsplit()函数是从字符串右边(末尾)开始分割。

**语法：**str.rsplit(sep=None, maxsplit=-1) -> list of strings 返回 字符串列表 或str.rsplit(sep=None, maxsplit=-1)[n]

**参数：**

- sep —— 分隔符，默认为空格,但不能为空即(")。
- maxsplit —— 最大分割参数，默认参数为-1。
- [n] —— 返回列表中下标为n的元素。列表索引的用法。

**示例：**

```python
 # 只搜索到一个sep时，两者结果相同
'abcxyzopq'.partition('xy')
('abc', 'xy', 'zopq')

'abcxyzopq'.rpartition('xy')
('abc', 'xy', 'zopq')

# 搜索到多个sep时，分别从左第一个、右第一个sep分割
'abcxyzopxyq'.partition('xy')
('abc', 'xy', 'zopxyq')

'abcxyzopxyq'.rpartition('xy')
('abcxyzop', 'xy', 'q
```



### 41、splitlines()

**描述：**按照('\n', '\r', \r\n'等)分隔，返回一个包含各行作为元素的列表，默认不包含换行符。\n 换行符 \r 回车符 \r\n 回车+换行

**语法：**S.splitlines([keepends=False])

**参数：**keepends -- 在输出结果里是否去掉行界符('\r', '\r\n', \n'等)，默认为 False，不包含行界符，如果为 True，则保留行界符。

**示例：**

```python
# 字符串以换行符为分隔符拆分，去掉换行符
'HOW\nSOFT\nWORKS'.splitlines()
['HOW', 'SOFT', 'WORKS']

# 如果keepends为True，保留换行符
'HOW\nSOFT\nWORKS'.splitlines(True)
['HOW\n', 'SOFT\n', 'WORKS']

"123\n456\r789\r\nabc".splitlines()
['123', '456', '789', 'abc'
```



### 42、join()

**描述：**将iterable变量的每一个元素后增加一个str字符串。

**语法：** sep.join(iterable)

- sep——分隔符。可以为空。
- iterable—— 要连接的变量 ，可以是 字符串，元组，字典，列表等。

**示例：**

```python
python中经常看到join，特别是在自然语言处理的时候，分词什么的，但是很多初学者不理解其中的意思，这里进行详细的介绍，希望对大家能有帮助。
将可迭代对象(iterable)中的字符串使用string连接起来。注意，iterable中必须全部是字符串类型，否则报错。如果你还是python的初学者，还不知道iterable是什么，却想来看看join的具体语法，那么你可以暂时将它理解为：字符串string、列表list、元组tuple、字典dict、集合set。当然还有生成器generator等也可以用该方法。

1）字符串
L='python'
'_'.join(L)
'p_y_t_h_o_n'
'_uu_'.join(L)
'p_uu_y_uu_t_uu_h_uu_o_uu_n'

2）元组
L1=('1','2','3')
'_'.join(L1)
'1_2_3'

3）集合。注意，集合无序。
L2={'p','y','t','h','o','n'}
'_'.join(L2)
't_n_o_h_y_p'

4）列表
L2=['py','th','o','n']
'_'.join(L2)
'py_th_o_n'

5）字典
L3={'name':"malongshuai",'gender':'male','from':'China','age':18}
'_'.join(L3)
'name_gender_from_age'

6）iterable参与迭代的部分必须是字符串类型，不能包含数字或其他类型。
L1=(1,2,3)
'_'.join(L1)
TypeError: sequence item 0: expected str instance, int found

以下两种也不能join。
L1=('ab',2)
L2=('AB',{'a','
```



## 九、字符串替换

### 43、replace()函数

**描述：把**str.中的 old 替换成 new,如果 count 指定，则替换不超过 count次.。

**语法：**str.replace(old, new, count)

**参数：**

- old —— 将被替换的子字符串。
- new —— 新子字符串，用于替换old子字符串。
- count —— 替换的次数，默认全部替换。

**案例：**

```python
s = "我的小伙伴张三"
s.replace("张三","马云")
'我的小伙伴马云'

s = "I love python"
#默认字符串中的全部"o" 全部替换为"w"
s.replace("o","w") 
'I lwve pythwn'

#只替换一个"o" 
s.replace("o","w",1)
'I lwve python'

#子字符串可以是多个字符。
s.replace("python","java")
'I love jav
```



### **44、expandtabs()**

描述：将字符串S中的 \t 替换为一定数量的空格。默认N=8。

语法： str.expandtabs(tabsize=8)

tabsize 的默认值为8。tabsize值为0到7等效于tabsize=8。tabsize每增加1，原字符串中“\t”的空间会多加一个空格。

示例：

```python
'01\t012\t0123\t01234'.expandtabs(4)
'01  012 0123    01234'

'01\t012\t0123\t01234'.expandtabs(8)
'01      012     0123    01234'
```



## **十、统计字符次数**

### 45、count()

**描述：**统计字符串里某个字符出现的次数。可以选择字符串索引的起始位置和结束位置。

**语法：**str.count("char", start,end) 或 str.count("char")

- str —— 为要统计的字符(可以是单字符，也可以是多字符)。
- star —— 为索引字符串的起始位置，默认参数为0。
- end —— 为索引字符串的结束位置，默认参数为字符串长度即len(str)。

**示例：**

```python
'abc--qo-ab'.count('ab')
2
#从第二位开始查找
'abc--qo-ab'.count('ab',1)
1
#不包括边界
'abc--qo-ab'.count('ab',1,9)
0
```