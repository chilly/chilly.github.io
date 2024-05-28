---
layout: post
title: "python ValueError: need more than XX values to unpack"
date: 2011-4-8
wordpress_id: 383
comments: true
tags: ["python", "python", "valueerror"]
categories:
- [code,python]
---
<meta name="_edit_last" content="1" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="_su_title" content="python valueError" />
<meta name="views" content="368" />
<meta name="_wp_old_slug" content="python-valueerror" />
this error seems you did not return the same number of results
for example:

```python
def fun():
   return 1
a,b = fun()
```
this will throw Exception : ValueError: need more than 2 values to unpack
change code:

```python
def fun():
   return 1,2
a,b = fun()
```


then ok!
