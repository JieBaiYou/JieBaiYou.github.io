---
title: Linux下使用convert批量压缩图片
tags:
  - linux
categories:
  - 技术
date: 2019-10-27 20:36:02
---
>convert的功能相当强大，先来个示例，再慢慢研究其它的功能
```
find /www/images  -regex '.*\(jpg\|JPG\|png\|PNG\|jpeg\)' -size +1000k -mtime -1 -exec convert -quality 75 {} {} \; 
find /www/images            # 要压缩图片的目录
-regex '.*\(jpg...|jpeg\)'  # 正则匹配需要压缩的文件扩展名
-size +1000k                # 要压缩图片的最小尺寸
-mtime -1                   # -mtime -n +n (按文件更改时间来查找文件，-n指n天以内，+n指n天以前)
convert -quality 75         # 压缩后的图片质量
```
<!-- more -- >

**安装**
如果系统中没有convert命令，就需要先安装了
```
CentOS:yum install ImageMagick #注意包名大小写
Ubuntu:apt install imagemagick
```

**转换图像格式**
支持JPG, BMP, PCX, GIF, PNG, TIFF, XPM和XWD等类型，下面举几个例子: 
```
convert xxx.jpg xxx.png  #将jpeg转成png文件 
convert xxx.gif xxx.bmp  #将gif转换成bmp图像 
convert xxx.tiff xxx.pcx #将tiff转换成pcx图像 
```

**改变图像的大小** 
```
convert -resize 1024x768 xxx.jpg xxx1.jpg #将图像的像素改为1024*768，注意1024与768之间是小写字母x 
convert -sample 50%x50% xxx.jpg xxx1.jpg  #将图像的缩减为原来的50%*50% 
```

**旋转图像**
```
convert -rotate 270 sky.jpg sky-final.jpg #将图像顺时针旋转270度 
```

**加边框**
```
convert -raise 5x5 input.jpg output.jpg
convert +raise 5x5 input.jpg output.jpg
```

**添加文字水印**
使用-draw选项还可以在图像里面添加文字：
在图像的10,80 位置采用60磅的全黑Helvetica字体写上 Hello, World! 
convert还有其他很多有趣和强大的功能，大家不妨可以试试。  
```
convert -fill black -pointsize 60 -font helvetica -draw 'text 10,80 "Hello, World!" ‘  hello.jpg  helloworld.jpg 
```
**批量图像格式转换** 
如果想将某目录下的所有jpg文件转换为png文件，在命令行模式下输入: 
```
for x in *.JPG; do
    convert -sample 25% "$x" "${x%.JPG}_converted.jpg"
    rm "$x"
done
```