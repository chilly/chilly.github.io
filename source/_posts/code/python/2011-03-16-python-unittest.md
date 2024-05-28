---
layout: post
title: "python unittest"
date: 2011-3-16
wordpress_id: 359
comments: true
tags: ["code", "python", "python", "test", "unit"]
categories:
- [code,python]
---
<meta name="_edit_last" content="1" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="_su_title" content="python, unittest, unit,test" />
<meta name="views" content="338" />
first you should import unittest, and write a class inherit unittest.TestCase. Then write some function named test_XXX, which uses "test_" as its prefix. Then in main, call the class's main() function.

this is an example:


```python
import unittest

class TestFunctions(unittest.TestCase):    
    def test_range(self):
       for i in range(10):
          self.assertEqual(i,i)

if __name__ == "__main__":
    TestFunctions.main()

```


<div id="_mcePaste">In this example, TestFunctions.main() will call all functions like test_XXX between setUp() and tearDown(). The sequence is 
```
setUp()--->test_XXX()--->tearDown()--->setUp()--->test_XXX()--->tearDown().....
```

