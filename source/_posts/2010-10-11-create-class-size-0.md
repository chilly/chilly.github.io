---
layout: post
title: "how to create a class , which size is 0?"
date: 2010-10-11
wordpress_id: 335
comments: true
tags: ["c", "cc", "class", "code", "sizeof"]
categories:
- code
---
<meta name="_edit_last" content="1" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="views" content="611" />
<meta name="_wp_old_slug" content="apply-class-size-0" />
how to create a class , which size is 0?

If you create a class with array [0], you have done it.:D

Only in gcc compiler....in visual c++6.0, it is still 1.


{% codeblock %}#include&lt;iostream&gt;
using namespace std;
class A
{
	char a[0];
};
int main()
{
	cout &lt;&lt; sizeof(A);
	return 0;
}{% endcodeblock %}
 
