---
layout: post
title: 'java手记---抛出异常'
date: 2006-07-26 09:59
comments: true
tags: ['java','exception','import']
categories:
- [learn, java]
---

Exception:  
throws出现在方法声明，而throw出现在可执行代码中。  
如：    
```java
public float getrain(float a) throws Exception  
{  
    if(a < 0)  
    {  
        Exception x = new Exception("the db must be wrong!");  
        throw x;  
    }  
    else  
        return 0;  
}  
/////////使用这个方法时要用  
try{  
    ....................  
}  
catch(Exception x)  
{
    ................  
}  

```
  
使用IOException,SQLException,AWTEception时要导入：  
```java
import java.io.IOException;  
import java.awt.AWTException;  
import java.sql.SQLException;  

```

