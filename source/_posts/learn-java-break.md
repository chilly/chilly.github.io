---
layout: post
title: "java手记---break,continu的特殊用法"
date: 2006-07-26 09:22
comments: true
tags: ["java"]
categories:
  - [learn, java]
---

```java

////////break和continu只能跳出一层循环。
///////如果前面加上标签，就可以随意跳出循环
outer:
while(true) {
    while(true) {
        i++;
        System.out.println("i = " +i);
        if(i == 1) {
        System.out.println("continue");
        continue;
        }
        if(i == 3) {
        System.out.println("continue");
        continue outer;////////这里可以跳到最外层循环再循环。
        }
        if(i == 5) {
        System.out.println("break");
        break;
        }
        if(i == 7) {
        System.out.println("break");
        break outer;///////////跳出out
        }
    }
}

int a = 1;
switch(a)
{
case 1:
System.out.print(a);
case 2:
System.out.print(a+1);
}///////可以打印出12，这是不加break的后果

```
