---
layout: post
title: "java 正则式"
date: 2012-07-08 22:52
comments: true
tags: java, 正则式, 正则表达式 
categories:
- knowledge
- java
---
摸索多次终于写出正确的java的关于email匹配的正则式：

```
import java.util.regex.Pattern;
import java.util.regex.Matcher;
Pattern emailPattern = Pattern.compile("^[\\w\\!#\\$%\'\\*\\+/=\\?{\\|}\\~\\^\\.\\-_]+@[\\w\\.\\-]+$");
Matcher m = emailPattern.matcher("xx@xxx.com");
if(m.matches()){
  // if match
}

```

注意其中的"-"，"+"等,这个在正则式中有特殊含义，如果"-"放在[]的中间，则表示从哪个字符到哪个字符,例如:[A-Z]。如果仅仅想表示"-"可能会出现，那么还要进行转移，所以就有"\\-"这样的形式。 上面正则式的意思是：以字母或者数字\w或者以!#$'*+/=?{|}~^.-_开头的1...n个字符，1个@字符，以字母或者数字或者.-结尾的字符串.
