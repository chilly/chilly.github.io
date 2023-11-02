---
layout: post
title: '用python求解组合问题'
subtitle: 'Solve python combination problem'
date: 2009-03-12 08:59
comments: true
tags: ['python','input']
categories:
- code
---

改动一下排列的代码，迅速变成组合问题的求解：
```python

def perm(n,m,current,lists=[]): 
    if m == len(lists): 
        return 1 
    sum = 0 
    for i in range(current,n+1): 
        if i not in lists: 
            sum+=perm(n,m,i+1,lists+[i]) 
    return sum 

n,m = [int(i) for i in raw_input().split()] 
    
print (perm(n,m,1))


```

python 上手果真比较快..

