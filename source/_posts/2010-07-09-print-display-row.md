---
layout: post
title: "print only display one row"
date: 2010-7-9
wordpress_id: 254
comments: true
tags: ["c", "cc", "code", "fflush", "n", "output", "printf", "r"]
categories:
- code
---
<meta name="_edit_last" content="1" />
<meta name="_su_description" content="这篇文章是写如何在界面上只输出一行，而不用写到另外一行的方法。中文不知道专业名称是什么，英语也不知道怎么说这个才能被搜索到。

this article is about how to print one row only, and do not need to write to another row. I don't know how to describe it in Chinese or English. And I also don't know this article how to be searched." />
<meta name="_su_keywords" content="c++,fflush,\n,\r,output,printf" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="_su_title" content="print only display one row process bar" />
<meta name="views" content="690" />
这篇文章是写如何在界面上只输出一行，而不用写到另外一行的方法。中文不知道专业名称是什么，英语也不知道怎么说这个才能被搜索到。

this article is about how to print one row only, and do not need to write infos to another row. I don't know how to describe it in Chinese or English. And I also don't know this article how to be searched.

This effect is like use wget to downloading files. You will see the process bar moving. How to display this effect? Use the following code:


{% codeblock %}#include&lt;cstdio&gt;
#include&lt;unistd.h&gt;
using namespace std;

int main(){
    printf("====&gt;\r");
    fflush(stdout);       #flush buffer
    sleep(2);             # wait
    printf("======&gt;\r");
    fflush(stdout);
    sleep(3);
    printf("========&gt;\n");
    return 0;
}{% endcodeblock %}


In the above code, '\r' is "Enter" character, and '\n' is “newline” character. If input '\n' into printf(), it will go to a new line. If input '\r' into printf(), the output cursor will go to the beginning. If you do not use fflush(stdout); you will never your output without '\n'.
