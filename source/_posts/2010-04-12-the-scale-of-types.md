---
layout: post
title: "the scale of types"
date: 2010-4-12
wordpress_id: 152
comments: true
tags: ["cc", "code", "int", "long-long", "scale", "type"]
categories:
- code
- program
---
<meta name="_edit_last" content="1" />
<meta name="views" content="328" />
The scale of type int is$latex -2^{31}~2^{31}-1$, and it is a little more than $latex -2*10^{9}~2*10^{9}$. and the scale of long long type is $latex -2^{63}~2^{63}-1$, and it is a little less than $latex -10^{19}~10^{19}$

int i = $latex 10^{6}* 10^{6}$; // this is wrong!!!!

long long j = $latex 10^{9}* 10^{9}$ ; // this is correct!

printf("%d",i);

printf("%lld",j);

int some platform or some compiler, long type is equal to int type. They are 32 bits. But long long type is 64 bits in linux.

use #define to declare a const, like this( if the number is too big):

#define BIG (9000000000000000000)ULL // this is unsigned long long
