---
layout: post
title: "the scale of types"
date: 2010-04-12
wordpress_id: 152
comments: true
tags: ["cc", "code", "int", "long-long", "scale", "type"]
categories:
- code
mathjax: true
---

The scale of type int is$[-2^{31},2^{31}-1]$, and it is a little more than 
$[-2*10^{9},2*10^{9}]$. and the scale of long long type is $[-2^{63},2^{63}-1]$, and it is a little less than $[-10^{19},10^{19}]$

int i = $10^{6}* 10^{6}$; // this is wrong!!!!

long long j = $10^{9}* 10^{9}$ ; // this is correct!

printf("%d",i);

printf("%lld",j);

int some platform or some compiler, long type is equal to int type. They are 32 bits. But long long type is 64 bits in linux.

use #define to declare a const, like this( if the number is too big):
```c
#define BIG (9000000000000000000)ULL // this is unsigned long long
```
