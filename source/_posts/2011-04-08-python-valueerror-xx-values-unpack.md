---
layout: post
title: "python ValueError: need more than XX values to unpack"
date: 2011-4-8
wordpress_id: 383
comments: true
tags: ["python", "python", "valueerror"]
categories:
- code
---
<meta name="_edit_last" content="1" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="_su_title" content="python valueError" />
<meta name="views" content="368" />
<meta name="_wp_old_slug" content="python-valueerror" />
<p>this error seems you did not return the same number of results</p>
<p>for example:</p>
<p>
def fun():
   return 1
a,b = fun()
</p>
this will throw Exception : ValueError: need more than 2 values to unpack
change code:
<p>
def fun():
   return 1,2
a,b = fun()
</p>
then ok!
