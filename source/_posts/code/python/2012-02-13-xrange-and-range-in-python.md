---
layout: post
title: "xrange and range in python"
date: 2012-02-13 20:14
comments: true
tags: python, xrange, range, difference,区别  
categories:
- [code,python]
---
#### 简单来说
python range会生成一个list对象，然后返回list中的内容，而xrange不会生成list对象。所以xrange相对与range来说更省空间，并且速度较快。

在英文中的官方解释是：
```python
class xrange(object)
 |  xrange([start,] stop[, step]) -> xrange object
 |  
 |  Like range(), but instead of returning a list, returns an object that
 |  generates the numbers in the range on demand.  For looping, this is 
 |  slightly faster than range() and more memory efficient.

```

ok. Do you understand?

