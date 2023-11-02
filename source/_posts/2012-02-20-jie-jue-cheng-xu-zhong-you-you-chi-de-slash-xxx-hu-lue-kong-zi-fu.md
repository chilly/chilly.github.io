---
layout: post
title: "程序中有游离的\\x,忽略空字符"
date: 2012-02-20 13:32
comments: true
tags: ["solve", "解决", "c++", "c", "windows", "linux", "编译"]
categories:
- knowledge
---
今天解决了linux 环境中编译c++或者c文件时产生的错误。
```
程序中有游离的"\xxx"
忽略空字符
```
以上两个错误，产生的原因和字符编码有关系。一般是文件从windows转移到linux下，然后使用c或者cc或者g++编译。产生了一系列的报警和错误信息。
解决方法如下：
```
1. 使用vi打开报错的文件
2. 查看其文件编码. 也就是用vi 查看encoding变量。":set fileencoding?" 
3. 如果编码不是utf-8则转换为utf-8. 使用":set fileencoding=utf-8"
4. 保存文件
```
当然产生这种错误的原因还有可能是由于在代码里输入了全角字符引起的。如果上面的方法不对，还需要再进一步排查`cat -A filename`。

