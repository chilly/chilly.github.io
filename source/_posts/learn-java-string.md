---
layout: post
title: 'java手记---String类'
date: 2006-07-26 10:16
comments: true
tags: ['java','string','equals']
categories:
- [learn, java]
---

```java
String a = "  dfsd";///////////这时输出dfsd。  
String b = a.trim();//////////除开头结尾的空格。  
concat(string s）/////////////把s 加在执行串后。  
substring(int n)///////////////从第n个字符之后的串。  
boolean equals(String s);////////////执行串与s完全相同,但不检查时不是同一个对象。  
boolean equalsIgnoreCase(String s);//忽略大小写对比。  
//String类中的toString()法可以用于debug。如：System.out.println(*.toString());

char charAt(int n);///////返回第n个字符。  
boolean startsWith(String s);/////////如果执行串以s开头返回true。

//////////int x = 0, y = 1, z = 2;  
//////////System.out.println(sString + x + y + z);///////这里把x，y，z都转换为string，而不是相加然后再转换为string

Math.random()//////可以生成随机数。生成0~1的随机小数。并且会随时间的变化而变化。可以产生0，但不能产生1  
java.lang.Character.isLowerCase((char)i) //检测i是否为小写。



```
